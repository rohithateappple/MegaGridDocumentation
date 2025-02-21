# Pathfinding

## Pathfinding Component

MegaGrid includes a high-performance pathfinding system, managed by a dedicated ``Pathfinding`` component. Being a component, it allows for **Multi-Agent** pathfinding, enabling each agent to have unique movement characteristics. The algorithm itself is a custom implementation of **A***.

Features:

- Handles large grids (up to 300x300) efficiently using async functionality.  
- Considers **TileTypes**, including movement costs.  
- Compatible with both Hex and Square grids.  
- Supports diagonal pathfinding.  
- Allows for highly customizable agent behavior.  
- Optimized for high-frequency pathfinding requests.  
- Includes built-in a **Move On Path** function.

## How to Path Find

1. To set up pathfinding, simply add the ``Pathfinding`` component to any actor of your choice. If you're starting with a blank actor, refer to [``BP_GridInteractions``](grid-interactions-actor.md#bp_grid-interactions) for a practical example of how pathfinding is implemented. This will give you a solid foundation to build upon.
    
    ![alt text](<../images/adding pf component.png>)

2. After adding the component, you'll need to configure a few settings. First, specify whether the pathfinding operates on a hex grid. Next, assign your ``TileTypeMapping`` data table—this is essential for retrieving movement costs. If your agent needs to avoid obstacles, add the **Obstacle** type to the ``TypesToBlock`` array.

    ![alt text](<../images/pf component basic settings.png>)

3. You can now test your pathfinding by calling the ``FindPath()`` function from the ``Pathfinding`` component. Below is a simple setup demonstrating its usage. 

    ![alt text](<../images/simple pathfinding bp setup.png>)

    The inputs are the **indices** of the start and target tile. Here we just pass in random values. The ``FindPath()`` function returns a ``PathStruct``, which includes the indices of the tiles forming the path and a ``PathCompleteReason``     enum. This indicates whether the pathfinding was successful or, if not, provides the reason for failure.

    ![alt text](<../images/simple pf log.png>)

4. To visualize the path, you would typically modify a series of ``TileState``. I won’t go into detail on that here, as you can find a complete implementation in the [``DrawPath()``](grid-interactions-actor.md#draw-path) event inside ``BP_GridInteractions``. However, for now, I'll use debug boxes for a simple visualization.

    ![alt text](<../images/basic path visualization.png>)

## Basic Pathfinding Settings  

- ``MaxIterations``: Sets the upper limit on how many iterations the pathfinding algorithm can run before stopping. If this limit is exceeded, the pathfinding returns an empty path with a ``PathNotFound`` reason. Keep this value as low as possible to avoid long wait times.  

- ``MaxPathLength``: Defines the maximum number of tiles allowed in a path. If this limit is exceeded, the algorithm returns an empty path with a ``TargetTooFar`` reason. This is useful for customizing agent-specific behavior.  

- ``TypesToBlock``: Any tile type added to this array will be **completely avoided** by the pathfinding algorithm. This is more efficient than assigning high movement costs since blocked tiles are excluded from complex calculations altogether.  

- ``TypesToIgnore``: Any tile type added to this array will be **completely ignored** by the pathfinding algorithm. This can be useful when designing characters that can bypass certain tile types.  

- ``StatesToBlock``: Any tile state added to this array will be **completely avoided** by the pathfinding algorithm. While states are not typically used for pathfinding interactions, there are cases where this can be useful—such as temporarily avoiding a building zone during placement.  

## Static Pathfinding

"Static Pathfinding" is a general term referring to one-time or low-frequency pathfinding operations. It can be thought of as a system where pathfinding is triggered only when the user clicks to set a **start** and **target** position. Essentially, it's a "click to activate" system. This should not be confused with synchronous pathfinding, as static pathfinding can also be asynchronous. The key difference lies in the frequency at which pathfinding is triggered.

## Synchronous Pathfinding

``FindPath()`` is a synchronous function. While MegaGrid provides robust async pathfinding, it's best to use synchronous pathfinding whenever possible, as async pathfinding offers little advantage for small to medium-sized grids. Since ``FindPath()``—along with most other MegaGrid functions—runs on the **Game Thread**, Unreal automatically manages the execution order.

## Asynchronous Pathfinding

``FindPathAsync()`` is an asynchronous function. This function calls pathfinding in a different thread altogether, this helps in avoiding performance drops during calculation of long paths and also opens up possibility for a real-time pathfinding system. 

This however must be used **very** cautiously, since we're dealing with multiple threads accessing the same grid data, there can be cases of race conditions. I've thoroughly tested this function with upwards of 20 threads running simultaneously and have encountered no issues but even still I couldn't have accounted for system variables (which are unique to each computer).

Most race conditions can be avoided by using ``ScopeLock``, which locks a piece of data when one process is using it avoiding overlaps. 
See [bScopeLock](tile-states.md#scope-lock)

### How to Async Path Find

1. To perform pathfinding asynchronously, call the `FindPathAsync()` function. This function takes the same arguments as `FindPath()`, requiring a start tile index and an end tile index. The key difference is that it does not return a result immediately. Since the function runs asynchronously, the pathfinding result is only available once the calculation is complete.

    ![alt text](<../images/find path async.png>)

2. To receive the pathfinding result, you need to bind an event to your actor. Select the `Pathfinding` component, go to the **Details** panel, find the **Events** section, and add the `OnPathComplete` event. This will create an event in your event graph, allowing you to handle the pathfinding result once it completes.
    
    ![alt text](<../images/on path complete even section.png>)
    
    ![alt text](<../images/on path complete .png>)

3. Here's a basic Blueprint workflow. It closely resembles the synchronous workflow, except that the output is handled differently due to the asynchronous nature of the function.

    ![alt text](<../images/basic async pf bp.png>)

4. For practical examples on async pathfinding, including **Real-Time** implementations, I highly recommend exploring the example projects and the [`BP_GridInteractions`](grid-interactions-actor.md#bp_grid-interactions) actor. 

    ![alt text](<../images/real-time pathfinding.png>)
    *BP_GridInteractions*

## Advanced Settings {#advanced-pf-settings}

- ``bUseDiagonals``: Activates diagonal pathfinding for square grids.

- ``bUseWeightedDiagonals``: Adds ``DiagonalCost`` to diagonal pathfinding.

- ``DiagonalCost``:  Higher costs will make paths less diagonal.

- ``bUseMovementCost``: Does the pathfinding account for movement costs? When OFF, performance is drastically improved at the cost of accuracy.

- ``DistanceBias``: How much does the pathfinding lean towards distance? Higher the value, less accurate but faster. (Default 0).

- ``HeuristicSizeMultiplier``: Value for affecting distance, depends on ``TileCount``. Use lower values for smaller grids, vice versa.

- ``MovementCostBias``: How much does the pathfinding lean towards movement cost? Higher the value, higher accuracy but slower. (Default 1).

- ``bUseHeap``: Uses heap optimization, may improve accuracy.


## Moving Object on Path

The `Pathfinding` component includes a C++-based function for moving objects along a given path. While this built-in method offers several parameters for control, I recommend implementing a custom Blueprint-based movement system for greater flexibility. However, if you want a quick solution, you can use the `StartMovingOnPath()` function, which takes the following parameters:

- `Path`: An array containing the tile locations that form the path.  
- `Interval`: The time interval between each movement update (functions like a timer).  
- `LerpSpeed`: The speed at which the object transitions between tile locations.  
- `Delay`: The wait time before each movement step. A value of `0` results in smooth movement, while higher values create a tile-by-tile stepping effect.

![alt text](<../images/start moving on path.png>)

### Usage

1. First, bind the `FollowPath()` event through the `Pathfinding` component's **Details** panel. This will automatically add the event to your **Event Graph**.

    ![alt text](<../images/follow path event bind.png>)
    
    ![alt text](<../images/follow path.png>)

2. Next, calculate the path locations by retrieving the location from the `TileTransform` of the `TileData`. You can do this by calling the `GetTileData()` function and using the resulting data to get the locations for each tile in the path index array. 

    ![alt text](<../images/path locations.png>)

    You can also see that I've used the `GetSurfaceLocation()`. While it's not strictly necessary, using it can improve accuracy, especially when dealing with uneven terrain or when you need precise positioning.

3. Call `StartMovingOnPath()` with the necessary parameters.

    ![alt text](<../images/start moving on path ex.png>)

4. The `MoveOnPath` event is triggered during each iteration of the movement process, allowing you to update the position and rotation of the actor. This gives you full control over the actor's movement along the path, allowing for smooth transitions and rotation as it progresses along the tiles.

    ![alt text](<../images/follow path implementation.png>)

    Again, I recommend checking out the [`BP_GridInteractions`](grid-interactions-actor.md#bp_grid-interactions) actor for practical examples.

## Avoiding Obstacles

There are two ways to avoid tiles of a specific `TileType`. 

The first method is by adding it to the `TypesToBlock` array in the `Pathfinding` component. Any type added to this array will be considered invalid, essentially treating it as though it doesn't exist for pathfinding purposes.

The second method involves setting high `MovementCosts` using the relevant data tables. Pathfinding will naturally avoid these high-cost tiles in favor of more efficient paths. If a `TileType` is added to the `TypesToIgnore` array, it will be considered the path of least resistance and be prioritized.

![alt text](<../images/avoiding triple cost.png>)

*Pathfinding avoids triple cost (yellow) tiles.*


## Large Scale Pathfinding

The `Pathfinding` component is optimized for handling large grids. To take full advantage of its capabilities, use `FindPathAsync()` for grids larger than *200x200*. This prevents FPS drops and ensures a smooth player experience. While high-frequency async pathfinding has been tested on large grids, it should still be used cautiously, as certain settings may cause frame drops.

### Recommended Settings

To maintain smooth real-time async pathfinding on massive grids, here are some best practices. These settings have been tested on MegaGrid's maximum supported size of *300x300* tiles.

#### 1. Limiting `MaxPathLength` and `MaxIterations`
Always set limits on these two parameters, regardless of grid size.  

- ``MaxPathLength`` should never exceed an agent’s available movement points.  
- `MaxIterations` rarely needs to be higher than **40,000**.

#### 2. Dynamically Adjusting Biases
As explained in the [Advanced Settings](#advanced-pf-settings), pathfinding uses specific biases that can be adjusted for better performance.

- **Preliminary Pathfinding (Real-Time Cursor Movement):**  
  Increase `DistanceBias` to be **greater** than `MovementCostBias`. This sacrifices accuracy but allows for quick path previews while moving the cursor.

- **Final Path Calculation (When Selecting a Target Tile):**  
  Reverse the biases—set `DistanceBias` **lower** than `MovementCostBias`. This prioritizes movement cost, leading to a more accurate final path.

This method balances performance and accuracy: the initial real-time pathfinding is lightweight, and only the final selection triggers a more precise but expensive calculation.  

You can see this implementation in action in `BP_GridInteractions`.

![alt text](<../images/dynamic biases.png>)

## End Note

There’s likely much more to pathfinding than what I’ve covered here—it could easily fill an entire separate document! I also don’t remember every technical detail that goes into the algorithm.  

If you come across something that isn’t covered, feel free to reach out!  

This guide should give you a solid starting point for pathfinding in MegaGrid. Be sure to check out the provided example projects to get familiar with implementation best practices.

![alt text](<../images/MegaGrid UE5 - 320x320 Tiles Stress Test - frame at 1m49s.jpg>)