# Tile Types

## Introduction

If you've gone through the **Getting Started** section, you should already have a basic understanding of ``TileType`` from when we discussed grid population. In this section, we'll explore it in greater detail.

Here are some important facts about tile types:  

- Each tile can have **only one** type.  
- Every tile **must** be assigned a type.  
- Types can be defined using data tables.  
- Types influence pathfinding behavior.  
- Each type has its own properties, such as Movement Cost and Color.  
- A tile without a specific type is considered an **Empty Tile**, which defaults to the **Default** type.

## Adding Custom Types

1. To add a new type, simply navigate to ``Plugins/MegaGridContent/Data/`` and open the ``DT_TileTypeMapping``.
    
    ![alt text](<../images/tile type mapping directory.png>)

2. Here press the <span class="highlight-box-settings">Add</span> button to create a new row.

    ![alt text](<../images/tile type mapping add button.png>)

3. Modify the row values as needed. Ensure that the **Row Name** and **Tile Type Name** are identical.

    ![alt text](<../images/new type add.png>)

4.That's it! You've successfully created your own tile type. To test it, switch to **Tile Editor** mode and locate your new type in the **Select Type** dropdown. You can now use it on your grid.

!!!note 
    If you're already in ``Tile Editor`` mode, you may need to close and reopen it for the new type to appear.

![alt text](<../images/tile editor new type.png>){ width=278 height=444 }

## Deleting Types {#delete-type} 

Deleting types follows the same process as adding them—simply delete the corresponding row in the data table. If any tiles in the level were using the deleted type, they must be reset to **Default** by clicking the <span class="highlight-box-settings">Remap Tiles</span> button in ``BP_GridManager``. **DO NOT** delete core types.

!!!note 
    If you're already in ``Tile Editor`` mode, you may need to close and reopen it for the new type to appear.

![alt text](<../images/remap tiles .png>)

## Getting Tile Type

You can retrieve a tile's type using the ``GetTileData()`` function. This function returns a ``TileData`` struct, which can be broken down to access the ``TileType``. Called via ``BP_GridManager``.

![alt text](<../images/get tile data.png>)

## Setting Tile Type

You can set a tile's type directly using the ``SetTileType()`` function or by overriding its ``TileData`` with ``SetTileData()``. The ``SetTileType()`` function requires the following parameters: 

- ``TileIndex``: The index of the tile to modify.
- ``Type``: The desired tile type.
- ``Context``: Defines whether the operation is for Editor or Runtime (best set to Auto).
- ``bReloadTile``: Determines whether to refresh the tile's visuals immediately after modification.

Called via ``BP_GridManager``.

!!! note
    Previous type is overwritten, keep track when neccessary.

![alt text](<../images/set tile data.png>)

## Removing Tile Type

To remove a type from a tile, you can use the ``RemoveTileType()`` function. This function takes similar arguments to ``SetTileType()``. Since a tile **must** have a type, there's no direct way to "remove" a tile type. Instead, when the specified type is removed, the tile is automatically reset to the **Default** type. Called via ``BP_GridManager``.

## Reloading Tiles {#reloading-tiles}

Whenever a tile's type or state is modified, you have the option to refresh its visuals immediately. However, it's best to use this feature only when necessary, as excessive calls can quickly add up and impact performance. Here’s an example of when refreshing a tile would be appropriate:

![alt text](<../images/relaod tile example 1.png>)

In the example above, the tile is modified using ``SetTileType()``. Since the modification occurs only once within the same call, reloading it is acceptable as there are no unnecessary refreshes. 

![alt text](<../images/reload tile example 2.png>)

In this example, both the tile's state and type are modified within the same call. To optimize performance, we only refresh its visuals after the final modification (``RemoveTileType()``). Since the reload reflects all changes, doing it twice would be redundant.

## Core Types

MegaGrid includes several default tile types in the ``DT_TileTypeMapping`` data table. It’s crucial that you **DO NOT** modify these types, as they are essential for the proper functioning of the grid. If you create your own data table, ensure these default types are still included.

Core Types:

- **Obstacle**
- **Default**

## Custom Tables 

You can create and use your own custom data tables if you prefer. Just make sure to include the core types in your table as well. Once you’ve created your custom table, you’ll need to assign it to the relevant actors, which are:

- GridManager

    ![alt text](<../images/gridmanager type mapping reference.png>)

- GridVisuals

    ![alt text](<../images/gridvisuals type mappin reference.png>)

- Pathfinding Component

    ![alt text](<../images/pathfinding comp typemapping ref.png>)

## Toggling Type Visibility

You can control the visibility of types using the `SetTypeVisibility()` function. This will hide all tiles of the specified type, but the type will remain in the data; only the visuals will be toggled. Called via ``BP_GridManager``.

![alt text](<../images/set type visibility.png>)

Be sure to call `Force Reload Tiles` to refresh the visuals immediately. This isn't called by default in the example projects, so please make this adjustment to ensure the function works as intended.

![alt text](<../images/Screenshot 2025-03-02 150116.png>)

