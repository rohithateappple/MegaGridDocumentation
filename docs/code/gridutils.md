# UGridUtils

This utility class contains numerous helper functions, all of which are accessible in Blueprint.

## Variables

`static UGridSubsystem* CachedGridSubsystem;` - Cached GridSubsystem that can be referenced anywhere in the project.

## GetGridSubsystem()

Returns a GridSubsystem pointer.

```cpp
static UGridSubsystem* GetGridSubsystem();
```

---

## GetGridSize()

Returns current `GridSize`.

```cpp
static FVector2D GetGridSize();
```

---

## GetIsHex()

Returns `bIsHex`.

```cpp
static bool GetIsHex();
```

---

## GetTileCount()

Returns current `TileCount`.

```cpp
static FIntPoint GetTileCount();
```

---

## Visual Getters

These functions are self explanatory, they return visual parameters.

```cpp
static float GetTileOpacity();
static float GetLineOpacity();
static float GetLineWidth();
static FLinearColor GetLineColor();
static FLinearColor GetTileColor();
```

---

## GetGridOffset()

Returns current `GridOffset`.

```cpp
static FVector2D GetGridOffset();
```

---

## GetIndexFromLocation()

Returns the tile index at the given location (doesn't have to be snapped to grid).

```cpp
static FIntPoint GetIndexFromLocation(const FVector& Location, bool bIsHexagon);
```

<span class="highlight-text-normal">Inputs</span>

``Location`` - World location.

``bIsHexagon`` - Is the grid a hex?

---

## GetLocationFromIndex()

Returns the location of the given tile index. Always snapped to grid.

```cpp
static FVector GetLocationFromIndex(const FIntPoint& TileIndex, bool bIsHexagon);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of Tile.

``bIsHexagon`` - Is the grid a hex?

---

## GetSurfaceLocation()

Returns the surface location of the given location using line traces. 

```cpp
static FVector GetSurfaceLocation(UObject* WorldContextObject, const FVector& Location, 
	UObject* IgnoredObject, bool& bOutHit);
```

<span class="highlight-text-normal">Inputs</span>

``WorldContextObject`` - World Context, in C++ pass in `GetWorld()`. In blueprints, it's passed automatically.

``Location`` - World location.

``IgnoredObject`` - Optional. You can pass in components and actors.

``bOutHit`` - Optional. Out variable that's changed depending on whether there's a hit.

---

## GetTileUnderCursor()

Returns the index of the tile under cursor.

```cpp
static FIntPoint GetTileUnderCursor(UObject* WorldContextObject,
	APlayerController* PlayerController);
```

<span class="highlight-text-normal">Inputs</span>

``WorldContextObject`` - World Context, in C++ pass in `GetWorld()`. In blueprints, it's passed automatically.

``PlayerController`` - Player controller, pass in `GetPlayerController()`.

---

## SnapLocationToGrid()

Snaps given location to grid.

```cpp
static FVector SnapLocationToGrid(const FVector& Location,
	bool LimitToGrid);
```

<span class="highlight-text-normal">Inputs</span>

``Location`` - World location.

``LimitToGrid`` - Limits new location to grid bounds.

---

## IsTileOfState()

Returns a bool indicating if the given tile is of given state.

```cpp
static bool IsTileOfState(FIntPoint TileIndex, FName State);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``State`` - `TileState` to check.

---

---

## IsTileOfType()

Returns a bool indicating if the given tile is of given type.

```cpp
static bool IsTileOfType(FIntPoint TileIndex, FName Type);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``Type`` - `TileType` to check.

---

---

## IsEven()

Returns a bool indicating if the given number is even. 

```cpp
static bool IsEven(const int& Num);
```

<span class="highlight-text-normal">Inputs</span>

``Num`` - Number.

---

## IsTileWithinBounds()

Returns a bool indicating if the given tile is within grid bounds. 

```cpp
static bool IsTileWithinBounds(FIntPoint TileIndex, bool bIsHex);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``bIsHex`` - Is the grid a hex?

---

## DoubleToAxial()

Converts doubled coordinate to axial coordinate. Returns an axial coord.

```cpp
static FIntPoint DoubleToAxial(FIntPoint Index);
```

<span class="highlight-text-normal">Inputs</span>

``Index`` - Index of tile (Doubled).

---

## AxialToDouble()

Converts axial coordinate to doubled coordinate. Returns an doubled coord.

```cpp
static FIntPoint AxialToDouble(FIntPoint Axial);
```

<span class="highlight-text-normal">Inputs</span>

``Axial`` - Index of tile (Axial).

---

## ConvertLocationToAxial()

Returns the axial coordinate of the given location.

```cpp
static FIntPoint ConvertLocationToAxial(FVector Location);
```

<span class="highlight-text-normal">Inputs</span>

``Location`` - World location.

---

## AxialToCube()

Coverts axial coordinate to cube coordinate. Returns cube coord.

```cpp
static FVector AxialToCube(FIntPoint Axial);
```

<span class="highlight-text-normal">Inputs</span>

``Axial`` - Index of tile (Axial).

---

## CubeToAxial()

Coverts cube coordinate to axial coordinate. Returns axial coord.

```cpp
static FIntPoint CubeToAxial(FVector Cube);
```

<span class="highlight-text-normal">Inputs</span>

``Cube`` - Index of tile (Cube).

---

## GetIndicesUnderBrush()

Retrieves the tile indices within the specified `BrushSize` radius around the given tile. Set an offset to even number of tiles.

```cpp
static TArray<FIntPoint> GetIndicesUnderBrush(FIntPoint CursorIndex, int32 BrushSize, FIntPoint Offset);
```

<span class="highlight-text-normal">Inputs</span>

``CursorIndex`` - Index of tile.

``BrushSize`` - Radius of brush (counted as tiles).

``Offset`` - Usually -1 if there are even number of tiles in either axis. For example a 3x3 area will have a
center tile (doesn't require offset). Whereas a 4x4 area without a central tile will need a (1, 1) offset. 

---

## GetNeighboringIndices()

Returns the neighboring tiles of the given tile.

```cpp
static TArray<FIntPoint> GetNeighboringIndices(FIntPoint TileIndex, bool bIsHex, bool bUseDiagonals);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Index of tile.

``bIsHex`` - Is the grid a hex?

``bUseDiagonals``  - Whether to get diagonal neighbors.

---

## ResolveGridVisualContext()

Resolves `EGridVisualContext`.

```cpp
static EGridVisualContext ResolveGridVisualContext(EGridVisualContext Context);
```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Context to resolve.

---


## GetScaleBasedOnNormal()

Returns a scale based on the surface normal. Used for tile instance transforms.

```cpp
static float GetScaleBasedOnNormal(const FVector& Normal, const float Min,
		const float Max);
```

<span class="highlight-text-normal">Inputs</span>

``Normal`` - Surface normal.

``Min`` - Min scale.

``Max`` - Max scale.

---


