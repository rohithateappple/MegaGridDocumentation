# Remapping Grid

## Preserving Data After Changing TileCount

During development, you may encounter situations where your grid is already populated, but you realize that the current `TileCount` is either too small or too large. In such cases, if you change variables like `TileCount` or `GridSize`, the grid will **lose** all previously stored data. However, you can avoid this issue by using the `Remap` function.

## Steps

1. In this example, I have a 50x50 grid with tiles already populated.

    ![alt text](<../images/Screenshot 2025-02-22 182938.png>)

2. If I modify the ``TileCount`` in ``BP_GridManager``, all the tiles will be removed.

    ![alt text](<../images/Screenshot 2025-02-22 183117.png>)

3. No need to worry! Just click the <span class="highlight-box-settings">Remap Tiles</span> button. Be sure **NOT** to press the <span class="highlight-box-settings">Create Grid</span> button, as this will replace the existing data with new data.

    ![alt text](<../images/Screenshot 2025-02-22 183346.png>)

4. This will map your previous data into your new grid!

    ![alt text](<../images/Screenshot 2025-02-22 183509.png>)

You can do the same for ``GridSize`` as well. 

## Caveats

While this method works well initially, repeatedly using it on the same original data will eventually lead to data loss. 

## Remapping Deleted Types

If a [deleted](../grid-interactions/tile-types.md#delete-type) type exists in your level, remapping is helpful to reset any tiles that were using the deleted type to **Default**. This ensures that there are no referencing issues in the future.

