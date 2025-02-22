# AGridManager

`AGridManager` is the central class for interacting with MegaGrid, acting as the bridge between the user and the plugin’s data and visual systems. It handles everything from managing grid data to controlling visual elements, making it the primary access point for most MegaGrid functions.

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

## GetAllTilesOfType(FName Type)

Gets all tiles of given type.

```cpp
TArray<FIntPoint> GetAllTilesOfType(FName Type);
```

<span class="highlight-text-normal">Inputs</span>

``Type`` - Type to get.

---

## GetAllTilesOfState(FName State)

Gets all tiles of given type.

```cpp
TArray<FIntPoint> GetAllTilesOfState(FName State);
```

<span class="highlight-text-normal">Inputs</span>

``Type`` - State to get.

---

## SetGridData(TMap<FIntPoint, FTileData> NewGridData)

Sets ``GridData``, overwriting it. Use with **caution**.

```cpp
void SetGridData(TMap<FIntPoint, FTileData> NewGridData);
```

<span class="highlight-text-normal">Inputs</span>

``NewGridData`` - New grid data.

---

## SetTileData(FTileData NewTileData)

Sets ``TileData`` to the index of the ``NewTileData``.

```cpp
void SetTileData(FTileData NewTileData);
```
<span class="highlight-text-normal">Inputs</span>

``NewTileData`` - New tile data.

---

## SetGridSize(FVector2D GridSize)

Sets grid size both for visuals and data.
Wipes existing tile visuals. Try remapping if you intend to use the existing data.

```cpp
void SetGridSize(FVector2D GridSize);
```

<span class="highlight-text-normal">Inputs</span>

``GridSize`` - New grid size.

---

## SetTileCount(FVector2D TileCount)

Sets tile count for both visuals and data. Wipes existing tile visuals. Try remapping if you intend to use the existing data.

```cpp
void SetTileCount(FVector2D TileCount);
```

<span class="highlight-text-normal">Inputs</span>

``TileCount`` - New tile count.

---

## SetSurfaceTraceChannel(ECollisionChannel Channel)

Sets the channel to be used for grid surface operations, such as surface hits, across the plugin.

```cpp
void SetSurfaceTraceChannel(ECollisionChannel Channel);
```

<span class="highlight-text-normal">Inputs</span>

``Channel`` - Collision channel to set.

---

## SetAutoMapTraceChannel(ECollisionChannel Channel)

Sets the channel to be used for auto mapping tiles.

```cpp
void SetAutoMapTraceChannel(ECollisionChannel Channel);
```

<span class="highlight-text-normal">Inputs</span>

``Channel`` - Collision channel to set.

---

## Visual Setters

The below functions are pretty self explanatory, they only affect the visuals of the grid.

```cpp

void SetLineWidth(float Width);

void SetLineColor(FLinearColor Color);

void SetLineOpacity(float Opacity);

void SetGridColor(FLinearColor Color);

void SetGridOpacity(float Opacity);

```

---

## SetGridShape(EGridShape Shape)

Sets grid shape (hex or square). Affects both visuals and data.

```cpp

void SetGridShape(EGridShape Shape);

```

<span class="highlight-text-normal">Inputs</span>

``Shape`` - Shape of grid.

---

## AddTileVisualToGrid(FIntPoint TileIndex, EGridVisualContext Context)

Adds visual to the specified tile. Can be called in runtime and editor. Can be called to simpy refresh a tile's visual.

```cpp

void AddTileVisualToGrid(FIntPoint TileIndex, EGridVisualContext Context);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``Context`` - Play context, whether runtime or editor.

---

## AddStateToTile(FIntPoint TileIndex, FName State, bool bReloadTile, bool bScopeLock)

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


## RemoveStateFromTile(FIntPoint TileIndex, FName State, bool bReloadTile, bool bScopeLock)

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

## SetTileType(FIntPoint TileIndex, FName Type, EGridVisualContext Context, bool bReloadTile)

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

## RemoveTileType(FIntPoint TileIndex, FName Type, EGridVisualContext Context, bool bReloadTile)

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

## ClearAllTilesOfState(FName State, bool bReloadTiles)

Clears the given state from all tiles of the same state. Only compatible in runtime.

```cpp

void ClearAllTilesOfState(FName State, bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``State`` - ``TileState`` to clear.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## ClearAllStates(bool bReloadTiles)

Clears all states from all tiles in the grid. Only compatible in runtime.

```cpp

void ClearAllStates(bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---


## ClearAllTilesOfType(FName Type, EGridVisualContext Context, bool bReloadTiles)

Clears the given type from all types of the same type. Resets to the **Default** type.

```cpp

void ClearAllTilesOfType(FName Type, EGridVisualContext Context, bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``Type`` - ``TileType`` to clear.

``Context`` - Play context, whether runtime or editor.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## ClearAllTypes(EGridVisualContext Context, bool bReloadTiles)

Clears types from all tiles in the grid. Resets to the **Default** type.

```cpp

void ClearAllTypes(EGridVisualContext Context, bool bReloadTiles);

```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

``bReloadTile`` - Indicates whether the visuals must be refreshed in the same call.

---

## SetStateVisibility(bool Visibility, FName State);

Toggles the visibility of the given state. When hidden, the state is temporarily ignored from other visual calls.

```cpp

void SetStateVisibility(bool Visibility, FName State);

```

<span class="highlight-text-normal">Inputs</span>

``Visibility`` - Hide / Unhide.

``State`` - State to toggle.

---

## SetTypeVisibility(EGridVisualContext Context, bool Visibility, FName TileType);

Toggles the visibility of the given type. When hidden, the type is temporarily ignored from other visual calls.

```cpp

void SetTypeVisibility(EGridVisualContext Context, bool Visibility, FName TileType);

```

<span class="highlight-text-normal">Inputs</span>

``Visibility`` - Hide / Unhide.

``TileType`` - Type to toggle.

---


## GenerateGrid(FIntPoint TileCount, bool bAutoMapTiles, bool bRandomObstacles, bool bRemap);

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

## GenerateGridLocations(FIntPoint TileCount, bool bUseSurface)

Generates locations of all the tiles in the grid. Useful for spawning objects.

```cpp

TArray<FVector> GenerateGridLocations(FIntPoint TileCount, bool bUseSurface);

```

<span class="highlight-text-normal">Inputs</span>

``TileCount`` - Number of tiles to be in grid.

``bUseSurface`` - Whether to trace for surface when generating locations.

---


## RecalculateTileLocations(EGridVisualContext Context)

Recalculates each tile's location in grid. Useful after terrain or surface changes.

```cpp

void RecalculateTileLocations(EGridVisualContext Context);

```
<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## DestroyGrid(EGridVisualContext Context)

Completely destroys the grid including visuals, tiles and data.

```cpp

void DestroyGrid(EGridVisualContext Context);

```
<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## ForceReloadTiles(EGridVisualContext Context)

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

## GetMovementCost(FIntPoint TileIndex)

Returns the movement cost of the given tile.

```cpp
float GetMovementCost(FIntPoint TileIndex);
```
<span class="highlight-text-normal">Inputs</span>

``TileIndex`` -  Index of tile.

---

##  RemapExistingTiles(EGridVisualContext Context)

Remaps the existing tile data to account for any modifications. For example, when a `TileType` is removed, the grid is remapped to reset any tiles that were using the deleted type. This is also used to preserve data when there are changes to `TileCount` or `GridSize`.

```cpp

void RemapExistingTiles(EGridVisualContext Context);

```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

