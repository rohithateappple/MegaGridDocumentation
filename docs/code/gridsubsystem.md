# UGridSubsystem (Internal)

`UGridSubsystem` is an internal class of MegGrid, derived from `UEngineSubsystem`. It acts as a **mediator between `.sav` files and the rest of the plugin**.  

This class is **not directly accessible** to users, but it plays a key role in managing data persistence and grid states behind the scenes. Most other classes refer to this class for its core functions.

---

## Variables

`struct FGridState` - Struct that holds `GridData`.

`FGridState EditorState;` - State used in editor.

`FGridState RuntimeState;` - State used in runtime, including PIE.

`FCriticalSection Mutex;` - Ensures thread safety by preventing concurrent data access.

`FString CurrentSaveName;` - .sav file that's currently being used.

`bool bIsRuntime` - Indicates whether runtime is active.

`ECollisionChannel GridSurfaceChannel` - Trace channel for grid surface.

`ECollisionChannel AutoMapChannel`- Trace channel for auto-mapping.

`FIntPoint StartIndex, EndIndex;` - Start and End tiles of a hexagonal grid.

`FVector2D LastGridSize;` - Previous `GridSize`;

`FVector2D GridSize;` - Current grid size;

`FVector2D GridOffset;` - Current grid offset.

`FIntPoint TileCount;` - Current tile count.

`bool bIsHex;` - Indicates if the grid is hexagonal.

---

## GetActiveState()

Returns the current `FGridState` being used.

```cpp
FGridState* GetActiveState();
```

---

## GetTileData()

Returns the tile data of the given tile index.

```cpp
FTileData GetTileData(FIntPoint TileIndex);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

---

## GetGridData()

Returns a `GridData` reference of the active state.

```cpp
TMap<FIntPoint, FTileData>& GetGridData();
```

---

## Common Getters

These functions are self explanatory.

```cpp
FVector2D GetGridSize();
FVector2D GetGridOffset();
FIntPoint GetTileCount();
bool GetIsHex();
float GetLineWidth();
float GetOpacity(bool bIsLine);
FLinearColor GetColor(bool bIsLine);
```

---

## SetGridData()

Overwrites the `GridData` of the active state with the given grid data.

```cpp
void SetGridData(TMap<FIntPoint, FTileData> GridData);
```

<span class="highlight-text-normal">Inputs</span>

``GridData`` - New grid data.

---


## AddTileToGrid()

Inserts the specified `TileData` into the given index of the active state's `GridData`. Overwrites previous data.

!!!warning 
    Outdated, use `SetTileData(FTileData TileData)` instead.

```cpp
void AddTileToGrid(FIntPoint TileIndex, FTileData TileData);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.
``TileData`` - Data of tile.

---

## RemoveTileFromGrid()

Removes the entry at the given index of the active state's `GridData`. This practically deletes the tile data.

```cpp
 void RemoveTileFromGrid(FIntPoint TileIndex);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

---

## ClearGridData()

Empties the `GridData` of the active state.

```cpp
void ClearGridData();
```

## SetTileData()

Sets / overwrites tile data at the containing tile index.

```cpp
void SetTileData(FTileData TileData);
```

<span class="highlight-text-normal">Inputs</span>

``TileData`` - Data of tile.

---

## Common Setters

These functions are self explanatory. They only affect the data.

```cpp
FVector2D SetGridSize(FVector2D _GridSize);
FVector2D SetGridOffset(FVector2D _GridOffset); 
FIntPoint SetTileCount(FIntPoint _TileCount);
bool SetIsHex(bool _bIsHex);
float SetLineWidth(float Width);
float SetOpacity(float Opacity, bool bIsLine);
FLinearColor SetColor(FLinearColor Color, bool bIsLine);
```

---

## SetSaveFileName()

Sets ``CurrentSaveName``.

```cpp
void SetSaveFileName(FString SaveName);
```

<span class="highlight-text-normal">Inputs</span>

``SaveName`` - Save name.

---

## SetRuntimeMode()

Sets ``bIsRuntime``.

```cpp
void SetRuntimeMode(bool bEnable);
```

<span class="highlight-text-normal">Inputs</span>

``bEnable`` - Sets `bIsRuntime`.

---

## CreateSaveData()

Creates a .sav file with default grid settings, including an empty `GridData`.

```cpp
void CreateSaveData(FString SaveName);
```

<span class="highlight-text-normal">Inputs</span>

``SaveName`` - Save name.

---

## SaveGridData()

Saves current data to .sav file of the given name. If no file is found, a new file is created.

```cpp
void SaveGridData(FString SaveName);
```

<span class="highlight-text-normal">Inputs</span>

``SaveName`` - Save name.

---

## LoadGridData()

Loads data from the .sav file of the given name. Name must be valid.

```cpp
void LoadGridData(FString SaveName);
```

<span class="highlight-text-normal">Inputs</span>

``SaveName`` - Save name.

---


