# AGridManager

`AGridManager` is the central class for interacting with MegaGrid, acting as the bridge between the user and the plugin’s data and visual systems. It handles everything from managing grid data to controlling visual elements, making it the primary access point for most MegaGrid functions.

---


## Variables

``UDataTable* TileTypeMapping;`` - Data table of ``TileTypeMapping``, used for getting colors and movement costs.

``TArray<FName> TypesToAutoMap;`` - Types to be mapped during ``GenerateGrid`` if ``bAutoMap`` is checked.

``float HexAutoMapExtent = 0.8;`` - Hexagonal extent of trace during auto-mapping. Higher = more accurate (may lead to bleeds).

``float SquareAutoMapExtent = 0.8;`` - Square extent of trace during auto-mapping.

``float ObstacleSpawnRate = 0.1;`` - Obstacle spawn rate during ``GenerateGrid`` while ``bRandomObstacles`` is checked. [0, 1]

``bool bUseShapeTrace = false;`` - Use shape tracing instead of line trace during auto map?

``TMap<FName, float> TypeMovementCostMap;`` -  Precomputed movements costs for each tile type.

``FIntPoint StartTile, EndTile;`` - Hexagonal start and end tiles, required for bounds calculation.

``float TileMinScale = 2;`` - Min tile z-scale. 

``float TileMaxScale = 8;`` - Max tile z-scale.

---

## GetGridData()

Retrieves a copy of grid data.


```cpp
TMap<FIntPoint, FTileData> GetGridData();
```

---

## GetTileData()

Retrieves a copy of the tile data associated with the tile index.

```cpp
FTileData GetTileData(FIntPoint TileIndex);
```
<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

---

## GetAllTilesOfType()

Gets all tiles of given type.

```cpp
TArray<FIntPoint> GetAllTilesOfType(FName Type);
```

<span class="highlight-text-normal">Inputs</span>

``Type`` - Type to get.

---

## GetAllTilesOfState()

Gets all tiles of given state.

```cpp
TArray<FIntPoint> GetAllTilesOfState(FName State);
```

<span class="highlight-text-normal">Inputs</span>

``State`` - State to get.

---

## SetGridData()

Sets ``GridData``, overwriting it. Use with **caution**.

```cpp
void SetGridData(TMap<FIntPoint, FTileData> NewGridData);
```

<span class="highlight-text-normal">Inputs</span>

``NewGridData`` - New grid data.

---

## SetTileData()

Sets ``TileData`` to the index of the ``NewTileData``.

```cpp
void SetTileData(FTileData NewTileData);
```
<span class="highlight-text-normal">Inputs</span>

``NewTileData`` - New tile data.

---

## SetGridSize()

Sets grid size both for visuals and data.
Wipes existing tile visuals. Try remapping if you intend to use the existing data.

```cpp
void SetGridSize(FVector2D GridSize);
```

<span class="highlight-text-normal">Inputs</span>

``GridSize`` - New grid size.

---

## SetTileCount()

Sets tile count for both visuals and data. Wipes existing tile visuals. Try remapping if you intend to use the existing data.

```cpp
void SetTileCount(FVector2D TileCount);
```

<span class="highlight-text-normal">Inputs</span>

``TileCount`` - New tile count.

---

## SetSurfaceTraceChannel()

Sets the channel to be used for grid surface operations, such as surface hits, across the plugin.

```cpp
void SetSurfaceTraceChannel(ECollisionChannel Channel);
```

<span class="highlight-text-normal">Inputs</span>

``Channel`` - Collision channel to set.

---

## SetAutoMapTraceChannel()

Sets the channel to be used for auto mapping tiles.

```cpp
void SetAutoMapTraceChannel(ECollisionChannel Channel);
```

<span class="highlight-text-normal">Inputs</span>

``Channel`` - Collision channel to set.

---

## Visual Setters

These functions are pretty self explanatory, they only affect the visuals of the grid. 

```cpp

void SetLineWidth(float Width);

void SetLineColor(FLinearColor Color);

void SetLineOpacity(float Opacity);

void SetGridColor(FLinearColor Color);

void SetGridOpacity(float Opacity);

```

---

## SetGridShape()

Sets grid shape (hex or square). Affects both visuals and data.

```cpp

void SetGridShape(EGridShape Shape);

```

<span class="highlight-text-normal">Inputs</span>

``Shape`` - Shape of grid.

---

## AddTileVisualToGrid()

Adds visual to the specified tile. Can be called in runtime and editor. Can be called to simpy refresh a tile's visual.

```cpp

void AddTileVisualToGrid(FIntPoint TileIndex, EGridVisualContext Context);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``Context`` - Play context, whether runtime or editor.

---

## AddStateToTile()

Adds the given state to the given tile. Only compatible in runtime.

```cpp

void AddStateToTile(FIntPoint TileIndex, FName State, bool bReloadTile, bool bScopeLock);

```
<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``State`` - ``TileState`` to add.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

``bScopeLock`` - Whether to lock accessing data from other processes. 

---


## RemoveStateFromTile()

Removes the given state from the given tile. Only compatible in runtime.

```cpp

void RemoveStateFromTile(FIntPoint TileIndex, FName State, bool bReloadTile, bool bScopeLock);

```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``State`` - ``TileState`` to remove.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

``bScopeLock`` - Whether to lock accessing data from other processes. 

---

## SetTileType()

Sets the given type to the given tile. Overrides existing ``TileType``.

```cpp

void SetTileType(FIntPoint TileIndex, FName Type, EGridVisualContext Context, bool bReloadTile);

```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``Type`` - ``TileType`` to set.

``Context`` - Play context, whether runtime or editor.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## RemoveTileType()

Removes the given type from the given tile. Resets to the **Default** type.

```cpp

void RemoveTileType(FIntPoint TileIndex, FName Type, EGridVisualContext Context, bool bReloadTile);

```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``Type`` - ``TileType`` to remove.

``Context`` - Play context, whether runtime or editor.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## ClearAllTilesOfState()

Clears the given state from all tiles of the same state. Only compatible in runtime.

```cpp

void ClearAllTilesOfState(FName State, bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``State`` - ``TileState`` to clear.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## ClearAllStates()

Clears all states from all tiles in the grid. Only compatible in runtime.

```cpp

void ClearAllStates(bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---


## ClearAllTilesOfType()

Clears the given type from all tiles of the same type. Resets to the **Default** type.

```cpp

void ClearAllTilesOfType(FName Type, EGridVisualContext Context, bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``Type`` - ``TileType`` to clear.

``Context`` - Play context, whether runtime or editor.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## ClearAllTypes()

Clears types from all tiles in the grid. Resets to the **Default** type.

```cpp

void ClearAllTypes(EGridVisualContext Context, bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## SetStateVisibility()

Toggles the visibility of the given state. When hidden, the state is temporarily ignored from other visual calls.

```cpp

void SetStateVisibility(bool Visibility, FName State);

```

<span class="highlight-text-normal">Inputs</span>

``Visibility`` - Hide / Unhide.

``State`` - State to toggle.

---

## SetTypeVisibility()

Toggles the visibility of the given type. When hidden, the type is temporarily ignored from other visual calls.

```cpp

void SetTypeVisibility(EGridVisualContext Context, bool Visibility, FName TileType);

```

<span class="highlight-text-normal">Inputs</span>

``Visibility`` - Hide / Unhide.

``TileType`` - Type to toggle.

---


## GenerateGrid()

Core function of MegaGrid, generates tiles and populates them with ``TileType``. By default each tile is assigned the **Default** type.

```cpp

void GenerateGrid(FIntPoint TileCount, bool bAutoMapTiles,
 bool bRandomObstacles, bool bRemap);

```

<span class="highlight-text-normal">Inputs</span>

``TileCount`` - Number of tiles to be in grid.

``bAutoMapTiles`` - Whether to automatically map tile types.

``bRandomObstacles`` - Whether to randomly generate obstacles. Takes precedence over ``bAutoMapTiles``.

``bRemap``- When active, types are remapped from previous data rather than overwritting them.

---

## GenerateGridLocations()

Generates locations of all the tiles in the grid. Useful for spawning objects.

```cpp

TArray<FVector> GenerateGridLocations(FIntPoint TileCount, bool bUseSurface);

```

<span class="highlight-text-normal">Inputs</span>

``TileCount`` - Number of tiles to be in grid.

``bUseSurface`` - Whether to trace for surface when generating locations.

---


## RecalculateTileLocations()

Recalculates each tile's location in grid. Useful after terrain or surface changes.

```cpp

void RecalculateTileLocations(EGridVisualContext Context);

```
<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## DestroyGrid()

Completely destroys the grid including visuals, tiles and data.

```cpp

void DestroyGrid(EGridVisualContext Context);

```
<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## ForceReloadTiles()

Forcefully reloads tile visuals. Useful after making data changes affecting visuals (like data tables).

```cpp

void ForceReloadTiles(EGridVisualContext Context);

```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## PrecomputeMovementCosts()

Preloads movement costs from a relevant data table for efficient retrieval.

```cpp

void PrecomputeMovementCosts();
```

---

## GetMovementCost()

Returns the movement cost of the given tile.

```cpp
float GetMovementCost(FIntPoint TileIndex);
```
<span class="highlight-text-normal">Inputs</span>

``TileIndex`` -  Index of tile.

---

##  RemapExistingTiles()

Remaps the existing tile data to account for any modifications. For example, when a `TileType` is removed, the grid is remapped to reset any tiles that were using the deleted type. This is also used to preserve data when there are changes to `TileCount` or `GridSize`.

```cpp

void RemapExistingTiles(EGridVisualContext Context);

```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---


## NotifyGridChanged()


Notifies any interested parties when grid has changed. Call this when you need to update the TileEditor when you set ``GridSize`` or ``GridShape``. Simply acts as an event to re-run code of interest.

```cpp

void NotifyGridChanged();

```

## NotifyOnDataTableAssigned()

Notifies any interested parties when a data table has changed. Call this when you need to tell other classes that you've updated any of your data table references. 

```cpp
void NotifyOnDataTableAssigned();
```

---

##  void UpdateTileTransform();

Updates the transform of the given TileIndex both visually and in data. Usually used for moving tile instances in the vertical axis.

```cpp

void UpdateTileTransform(EGridVisualContext Context, FIntPoint TileIndex, FTransform InTransform);

```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

``TileIndex`` - Tile to modify.

``InTransform`` - New transform.

---