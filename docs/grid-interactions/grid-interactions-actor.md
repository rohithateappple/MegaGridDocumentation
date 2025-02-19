# Utilizing Examples

## BP_GridInteractions

`BP_GridInteractions` is a sample blueprint included with MegaGrid, designed to demonstrate various grid interaction implementations. I highly recommend exploring this blueprint to understand common interaction patterns. Doing so will give you a solid foundation for designing your own custom interactions. The actor can be found in ``Plugins/MegaGridContent/Blueprints/Core/``. 

![alt text](<../images/bp-grid-interactions directory.png>)

Features:

- Adding ``TileType`` / ``TileState`` under brush.
- Removing the given ``TileType`` / ``TileState`` under brush.
- Toggling visibility of the given ``TileType`` / ``TileState``.
- Clear all states / types.
- Basic Pathfinding.
- Realtime Async Pathfinding.
- Move actor on path.

Place the actor in your level and experiment with different interactions. I strongly encourage you to open the blueprint and adjust the execution pins to explore the full feature set. For example, the below setup changes the tile type by default, but by simply redirecting the execution flow, you can transform it into a state-setting system.

![alt text](<../images/set tile type interactions.png>)

### Controls

- ``LMB`` - Start pathfinding. Set Start and Target.
- ``F`` - Add ``TileType`` / ``TileState`` under brush.
- ``K`` - Remove ``TileType`` / ``TileState`` under brush.
- ``E`` - Toggle ``TileType`` / ``TileState`` visibility.
- ``C`` - Clear all states / types.

### Pathfinding Example

By default ``BP_GridInteraction`` uses a static sync pathfinding workflow. But you can easily switch to the async pathfinding by repinning these functions.

1. In the end of ``BeginPlay``, connect the exec pin to the ``SetTimerByFunction()`` from ``SetShowMouseCursor()``. This calls the async pathfinding every 0.02 seconds.

    ![alt text](<../images/set timer by func realtime pf.png>)

2. Next we'll need to disable static pathfinding by going to the ``LMB`` input and removing the exec pin to the ``DoPathfinding`` branch.

    ![alt text](<../images/do pf.png>)

    ![alt text](<../images/do pf branch.png>)

3. With these changes, you'll now have a real-time async pathfinding in place.

### Drawing Paths

To visualize a path after pathfinding completes, we need to update the states of relevant tiles. Essentially, we track the previous path, remove its "path" state, and apply the same state to the new path. You can refer to the implementation in the ``DrawPath`` event for guidance.

![alt text](<../images/draw path.png>)

## BP_GridManager

``BP_GridManager`` also includes an example implementation for **Highlighting the Tile Under the Cursor**. I placed this feature here because cursor highlights are a common function, and it makes sense to integrate them into a manager class. In contrast, the examples in ``BP_GridInteractions`` tend to be agent-specific. You can find this implementation in ``BeginPlay``, where a ``TimerByFunction`` is used to call ``HighlightTileUnderCursor()`` at regular intervals, handling all necessary state changes.

![alt text](<../images/highlight tile under cursor.png>)

![alt text](<../images/highlight tile under cursor function.png>)

Also feel free to explore the "button" functions since most of them can be called in runtime as well.

![alt text](<../images/useful buttons.png>)

## Example Projects

MegaGrid includes an example project featuring additional implementations such as Multi-Agent Pathfinding and Level Changes. I highly encourage you to explore these as well.

!!! note 
    The example project is different from the demo game. While some features may be missing, they can be easily implemented.

![alt text](<../images/multi-agent pf.png>)

## Making Your Own

By using these examples, you'll gain a solid understanding of how to create your own interactive grid systems. At its core, the process involves accessing / modifying tile data—whether it's ``TileType``, ``TileState``, ``MovementCost``, visibility, or other attributes. If you have difficulty implementing certain features, feel free to reach out!

![alt text](<../images/pathfinding hex grid big.jpg>)

