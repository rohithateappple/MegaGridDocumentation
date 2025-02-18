# Tile States

## Introduction

``TileState`` is similar to ``TileType`` but the key difference is that it isn't meant to affect pathfinding. Each tile can also
have several tile states, however a single state is picked based on priority. States are meant to be temporary visual representations
of a tile's behavior, e.g path, hover, selection, etc. ``TileState`` also takes priority over ``TileType``, if the state's color alpha is 
less than 1, color mixing is used to fake transperency. 

## Adding Custom States

1. To add a new state, simply navigate to ``Plugins/MegaGridContent/Data/`` and open the ``DT_TileStateMapping``.
    
    ![alt text](<../images/tilestatemapping directory.png>)

2. Here press the <span class="highlight-box-settings">Add</span> button to create a new row.

    ![alt text](<../images/add button state.png>)

3. Modify the row values as needed. Ensure that the **Row Name** and **Tile State Name** are identical.

    ![alt text](<../images/adding new state.png>)

## Deleting States 

Deleting states follows the same process as adding them—simply delete the corresponding row in the data table. **DO NOT** delete core states.

## Getting Tile States

You can retrieve a tile's states using the ``GetTileData()`` function. This function returns a ``TileData`` struct, which can be broken down to access the ``TileStates``. This is done through the ``GridManager``.

![alt text](<../images/get tile states.png>)

## Adding Tile States

Unlike tile types, you can keep adding states to a tile. You can do so using ``AddStateToTile()``, which requires the following parameters: 


- ``TileIndex``: The index of the tile to modify.
- ``State``: The desired tile state.
- ``Context``: Defines whether the operation is for Editor or Runtime (best set to Auto).
- ``bReloadTile``: Determines whether to refresh the tile's visuals immediately after modification.
- `ScopeLock`: This determines whether to lock the data being accessed, ensuring that no other operations interfere with the data during its usage. 

![alt text](<../images/add state to tile.png>)

## Removing Tile States

To remove a state from a tile, you can use the ``RemoveStateFromTile()`` function. This function takes similar arguments to ``AddStatetoTile()``.

![alt text](<../images/remove state from tile.png>)

## Reloading Tiles

[Refer here.](tile-types.md#reloading-tiles)

## Core States

MegaGrid includes several default tile states in the ``DT_TileStateMapping`` data table. It’s crucial that you **DO NOT** modify these states, as they are essential for the proper functioning of the grid. If you create your own data table, ensure these default states are still included.

Core States:

- **Default**
- **Hovered**
- **ValidPath**
- **InvalidPath**

## Custom Tables 

You can create and use your own custom data tables if you prefer. Just make sure to include the core states in your table as well. Once you’ve created your custom table, you’ll need to assign it to ``BP_GridVisuals``.

![alt text](<../images/grid visuals state mapping ref.png>)

## State Priority

Since a tile can have multiple states, the `State Priority` array determines which state takes precedence in rendering. To adjust the order of priority, navigate to `BP_GridVisuals` and look for *"State Priority"*. The priority follows ascending order, meaning states with lower indices take higher precedence. The states are chosen dynamically, so when a state is added or removed, the visuals will automatically update. Make sure to add your custom states (if any) to this list, or they won't be rendered.

![alt text](../images/state-priority.png)