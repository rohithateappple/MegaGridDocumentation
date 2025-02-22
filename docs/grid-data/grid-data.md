# Grid Data

This section explains grid data and its role within the plugin. Understanding how it works will help you fully utilize it when building custom systems.  

Grid data can be categorized into the following:  

- **Visual Data**  
- **Tile Data**  
- **Actor Exposed Variables**  
- **Core Variables**  

Grid data is stored in **.sav** files located in:  
``YourProjectName/Saved/SaveGames/``  

It's important to distinguish between **grid data** and the ``GridData`` object. While **grid data** refers to any information related to the MegaGrid plugin, ``GridData`` specifically refers to a `TMap` that maps tile indices to their corresponding ``TileData``.  

All grid-related data is managed through the `GridSubsystem` class, which acts as an intermediary between the **.sav** files and the rest of the plugin.

## Visual Data

**Visual data** refers to any setting that directly affects the grid’s appearance. This includes:  

- ``LineWidth``  
- ``GridSize``
- ``TileCount`` 
- ``Tile/Line Opacity``
- ``Tile/Line Color``

These variables are primarily set via ``BP_GridManager``, which then applies them to the ``GridVisuals`` class. While some of these settings may influence other behaviors, most are purely visual. All visual data is stored in the **.sav** file.

## Tile Data

``TileData`` is derived from the ``FTileData`` struct, which stores all relevant information about each tile, including:  

- ``Index``: The tile’s index, indicating its position in the grid.  
- ``GCost``: Movement cost used in pathfinding. Unlike ``MovementCost``, this undergoes additional calculations.  
- ``HCost``: Distance or heuristic cost, used in pathfinding.  
- ``FCost``: The sum of ``GCost`` and ``HCost``.  
- ``MovementCost``: The base movement cost of a tile, set during generation.  
- ``ParentIndex``: The previous tile’s index, used for path reconstruction.  
- ``Neighbors``: The tile's neighboring indices. Not utilized in this plugin.  
- ``TileTransform``: The tile’s transform, determined during generation.  
- ``TileType``: The type of tile, relevant for pathfinding.  
- ``TileState``: An array representing the tile’s state, useful for temporary visual effects.

Understanding how ``TileData`` works is essential for creating interactions with the grid. Whenever you interact with the grid, you'll likely need to work with ``TileData`` in some way. All tile data is stored in the **.sav** file.

## Actor Exposed Variables

Actor-exposed variables are those created within a blueprint instance that aren't explicitly defined in its parent class. For example, the visual settings in the ``BP_GridManager`` class are created in the blueprint but aren't present in its parent ``GridManager`` C++ class. Despite this, they play a crucial role in configuring the grid. These variables aren’t stored separately in the .sav file but are loaded during the blueprint’s construction to match the saved data.

![alt text](<../images/grid settings.png>)

If the save-load procedure isn't properly followed, these variables can cause a mismatch between the actual data. If you encounter this issue, try reloading the data by pressing the <span class="highlight-box-settings">Load Grid</span> button in ``BP_SaveHandler``.

## Core Variables

Aside from ``TileData`` and ``GridData``, there are core variables essential to the grid’s functionality:  

- ``bIsHex``: Determines the grid shape, crucial for all calculations.  
- ``TileCount``: Defines the total number of tiles, used in generation and setting grid bounds.  
- ``GridSize``: Specifies tile size, which can be asymmetric and are required for most calculations.  
- ``StartTile``: Marks the starting tile of the grid bounds (specific to hex grids).  
- ``EndTile``: Marks the ending tile of the grid bounds (specific to hex grids).
