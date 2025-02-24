# UPathFindingComponent

Pathfinding in MegaGrid is managed by this actor component.

## Variables

``bool bIsHex;`` - Tells pathfinding whether to use hex calculations.

`int32 MaxIterations = 25000;` - Maximum pathfinding iterations. When exceeded, returns an empty path.

`int32 MaxPathLength = 300;` - Maximum tiles in path. When exceeded, returns an empty path.

`bool bUseHeap;` - Whether to use heap optimization. May increase accuracy.

`TArray<FName> TypesToBlock;` - Types that should be treated as obstacles without affecting calculations.  

`TArray<FName> TypesToIgnore;` - Types that should be considered as having zero movement cost.  

`TArray<FName> StatesToBlock;` - States that should be treated as obstacles.

`UDataTable* TileTypeMapping;` - Data table of ``TileTypeMapping``, used for getting movement costs.

`bool bUseDiagonals = false;` - Whether to use diagonals for square grids.

`bool bUseWeightedDiagonals = false;` - Whether to use weighted diagonals.

`float DiagonalCost = 1;` - Higher cost means less diagonal paths.

`TMap<FName, float> TypeMovementCostMap;` - Precomputed movements costs for each tile type.

`bool bUseMovementCost = true;` - Whether to use movement costs? False = faster pathfinding, less accuracy.

`float DistanceBias = 0;` - How much bias does the pathfinder have towards Distance? Lower = Higher Accuracy (Slower). Higher = Lower Accuracy (Faster).

`float HeuristicSizeMultiplier = 500;` - Multiplier for influencing Distance. Lower for smaller tile counts, vice versa

`float MovementCostBias = 1;` - How much bias does the pathfinder have towards MovementCost (GCost)? Lower = Higher Accuracy(Slower), Higher = Lower Accuracy(Faster)

`FRunnable* CurrentRunnable;` - Runnable used for async pathfinding.

`FRunnableThread* CurrentThread;` - Thread used for async pathfinding.


## IsGridGenerated()

Returns a bool indicating if the grid has been populated with tiles.

```cpp
bool IsGridGenerated();
```

## GetDistance()

Heuristic or distance function. Returns the distance between two tiles.

```cpp
float GetDistance(FIntPoint CurrentIndex, FIntPoint TargetIndex);
```

<span class="highlight-text-normal">Inputs</span>

``CurrentIndex`` - Index of start tile.

``TargetIndex`` - Index of target / end tile.

---

## GetHexDistance()
 
Heuristic or distance function for hexagonal grid. Returns the distance between two tiles.

```cpp
float GetHexDistance(FIntPoint CurrentIndex, FIntPoint TargetIndex);
```

<span class="highlight-text-normal">Inputs</span>

``CurrentIndex`` - Index of start tile.

``TargetIndex`` - Index of target / end tile.

---

## GetLowestFCostNode

Returns the tile data of the lowest F Cost in the given Open List.

```cpp
FTileData GetLowestFCostNode(TArray<FTileData>& OpenList) const;
```

<span class="highlight-text-normal">Inputs</span>

``OpenList`` - Tile Data array reference.

---

## GeneratePath()

Generates or retraces path if pathfinding was successful. Returns the tile indices of the final path.

```cpp
TArray<FIntPoint> GeneratePath(FIntPoint StartIndex, FIntPoint TargetIndex,
	TMap<FIntPoint, FTileData>& GridData);
```

<span class="highlight-text-normal">Inputs</span>

``CurrentIndex`` - Index of start tile.

``TargetIndex`` - Index of target / end tile.

``GridData`` - GridData reference.

---

## IsTileWalkable()

Returns a bool indicating if a given tile is walkable.

```cpp
bool IsTileWalkable(FIntPoint TileIndex, TMap<FIntPoint, FTileData>& GridData);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``GridData`` - GridData reference.

---

## IsTileValid()

Returns a bool indicating if a given tile is walkable. Specific to Blueprint.

```cpp
bool IsTileValid(FIntPoint TileIndex);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

---

## GetPrecomputedNeighbors()

Gets precomputed neighbors for the given tile.

!!!warning 
    This is not used in the current implementation. If you want to use this, 
    precompute neighbors during grid generation. Not for async, will crash if used.

```cpp
TArray<FIntPoint> GetPrecomputedNeighbors(FIntPoint TileIndex);
```
 
<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

## StopPathFindingThread();
 
Stops and clears the current async pathfinding thread.

```cpp
void StopPathFindingThread();
```
 
## FPathCompleteDelegate

Blueprint compatible event triggered when async pathfinding completes. Returns an `FPathStruct`.

```cpp
FPathCompleteDelegate OnPathComplete;
```

## FPathFollowDelegate

Blueprint compatible event triggered during each iteration of `StartMovingOnPath()`. Returns an `FTransform`.

```cpp
FPathFollowDelegate FollowPath;
```

## PrecomputeMovementCosts()

Preloads movement costs from `TileTypeMapping` data table into `TypeMovementCostMap`.

```cpp
void PrecomputeMovementCosts();
```

## FindPath()

Finds a path between two tiles. Returns an `FPathStruct` containing the final 
path array and a `EPathCompleteReason`. Synchronous function.

!!! note
    Use in low frequencies.

```cpp
FPathStruct FindPath(FIntPoint StartIndex, FIntPoint TargetIndex);
```

<span class="highlight-text-normal">Inputs</span>

``StartIndex`` - Index of start tile.

``TargetIndex`` - Index of target / end tile.

---

## FindPathAsync()

Finds a path between two tiles. Asynchronous function, triggers the `OnPathComplete` event upon completion. Which in turn returns an
`FPathStruct`.

```cpp
void FindPathAsync(FIntPoint StartIndex, FIntPoint TargetIndex);
```
<span class="highlight-text-normal">Inputs</span>

``StartIndex`` - Index of start tile.

``TargetIndex`` - Index of target / end tile.

---

## GetTotalMovementCost()

Returns the total movement cost of a given path. Use this to calculate how much points are needed to traverse a path.

```cpp
float GetTotalMovementCost(TArray<FIntPoint> Path);
```

<span class="highlight-text-normal">Inputs</span>

``Path`` - Path array.

---

## PathMovementTimerHandle

Timer to handle delays for `StartMovingOnPath()`.   

```cpp
FTimerHandle PathMovementTimerHandle;
```

---

## StartMovingOnPath()

Moves along the given path. Triggers the `FollowPath` event which returns a `FTransform`.

```cpp
void StartMovingOnPath(TArray<FVector> Path, double Interval, double LerpSpeed, double Delay);
```

<span class="highlight-text-normal">Inputs</span>

``Path`` - Path array.

``Interval`` - Interval between the each timer call.

``LerpSpeed`` -  Speed of movement.

``Delay`` - Delay between each movement, increase for tile-to-tile movement.

---