# Tile Types

## Introduction

If you've been following along with the documentation, it's safe to assume you've already completed the **Getting Started**
section. In there we briefly talked about ``TileType`` in the context of populating the grid. In this section we'll cover them in a little more detail.

Here are some key points about types:

- Each tile can only have **one** type.
- Each tile **must** have a type.
- Types can be added via data tables.
- Types affect pathfinding.
- Types have their own data: Movement Cost & Color.
- An empty tile refers to a tile having the **Default** type.

## Adding Custom Types

1. To add new types simply navigate to ``Plugins/MegaGridContent/Data/`` and open the ``DT_TileTypeMapping``.
    
    ![alt text](<../images/tile type mapping directory.png>)

2. Here hit the <span class="highlight-box-settings">Add</span> button to create a new row.

    ![alt text](<../images/tile type mapping add button.png>)

3. Change the contents to your desired values. It's important that your ``Row Name`` and ``Tile Type Name`` are exactly the same.

    ![alt text](<../images/new type add.png>)

4. That's it you've now created your own type! To test it you can enter the ``Tile Editor`` mode and find your new type in the ``Select Type``
dropdown. You can now use this type in your grid.

    !!!note 
        If you're already in the ``Tile Editor`` mode, you may have to close and reopen it, in order for it to update.

    ![alt text](<../images/tile editor new type.png>){ width=278 height=444 }

## Removing Types

Removing types is the same as adding types, this time you simply delete the desired row in the data table. If any tiles in the level are of
the deleted type, it must be reset to **Default** by pressing the <span class="highlight-box-settings">Remap Tiles</span> button in ``BP_GridManager``.

!!!note 
    If you're already in the ``Tile Editor`` mode, you may have to close and reopen it, in order for it to update.

![alt text](<../images/remap tiles .png>)

## Getting Tile Type

We can get a tile's type via ``GetTileData()``. This function returns ``TileData`` and can broken to access the ``TileType``. 
Accessed via ``GridManager``.

![alt text](<../images/get tile data.png>)

## Setting Tile Type

We can set a tile's type directly via ``SetTileType()`` or by overriding it's ``TileData`` via ``SetTileData()``. ``SetTileType()`` takes
a ``TileIndex``, ``Type``, ``Context``(Editor or Runtime, best set to Auto) and ``bReloadTile`` (tells whether to refresh visuals immediately).  

!!! note
    Previous type is overwritten, keep track when neccessary.

![alt text](<../images/set tile data.png>)

## Reloading Tiles

Whenever a tile's type or state is modified, you have the option to refresh its visuals immediately. However, it's best to use this feature only when necessary, as excessive calls can quickly add up and impact performance. Here’s an example of when refreshing a tile would be appropriate:

![alt text](<../images/relaod tile example 1.png>)

In the example above, the tile is modified using ``SetTileType()``. Since the modification occurs only once within the same call, reloading it is acceptable as there are no unnecessary refreshes.

![alt text](<../images/reload tile example 2.png>)

In this example, both the tile's state and type are modified within the same call. To optimize performance, we only refresh its visuals after the final modification (``RemoveTileType()``). Since the reload reflects all changes, doing it twice would be redundant.

