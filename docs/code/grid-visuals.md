# AGridVisuals

`AGridVisuals` is an actor class responsible for managing all grid-related visuals in MegaGrid. Typically, this class is not intended for direct interaction or modification. However, if you need to extend its functionality, this documentation may be helpful.

---

## Variables

``UDataTable* TileTypeMapping;``- Data table of ``TileTypeMapping``, used for getting colors.

``UDataTable* TileStateMapping;`` - Data table of ``TileStateMapping``, used for getting colors.

``UPostProcessComponent* GridPostProcess;`` - Post-process component used for Zero Aliasing Grid.

``float SQHighlightScaleMultiplier = 1.02;`` - Tile material (highlight) texture size.

``float HexHighlightScaleMultiplier = 1.08;`` - Tile material (highlight) texture size.

``bool bCustomGridMaterial;`` - Whether to use custom grid materials instead of Zero Aliasing Grid?

``bool bCustomHighlightMaterial;`` - Whether to use custom tile (highlight) materials? 

``UMaterialInterface* HexGridMaterial;`` - Parent grid lines material (hex).

``UMaterialInterface* SquareGridMaterial;`` - Parent grid lines material (square).

``UMaterialInterface* HexHighlightMaterial;`` - Parent tile (highlight) material (hex).

``UMaterialInterface* SquareHighlightMaterial;`` - Parent tile (highlight) material (square).

``UStaticMesh* TileMesh;`` - Mesh used for tile instances.

``int32 ChunkSize = 50;`` - How many tiles belong to one HISMC? (e.g. 50 = 50x50 tiles per HISM).

``TArray<FName> TypesToNotRender;`` - Tile types to blacklist. Their colors will be temporarily set to 0,0,0,0.

``TArray<FName> StateToNotRender;`` -  Tile states to blacklist. Their colors will be temporarily set to 0,0,0,0.

``TArray<FName> StatePriority { "Hovered", "Path", "Selected", "None" };`` - Render priority for states—states with lower indices take precedence over those with higher indices.

``TMap<FName, FVector4> TypeColorMap;`` - Map holding type to color.

``TMap<FName, FVector4> StateColorMap;`` - Map holding state to color.

---

## PrecomputeColors()

Loads tile type and state colors into `TypeColorMap` and `StateColorMap` in advance.

```cpp
void PrecomputeColors();
```

---

## SetupDynamicMaterials()

Sets up all dynamic materials including Zero Aliasing Grid and tile highlights.

```cpp
void SetupDynamicMaterials();
```

---

## SwitchGridShape()

Changes grid shape.

```cpp
void SwitchGridShape(EGridShape GridShape);
```

---

## LoadAllVisuals()

Loads all visuals from existing grid data.

```cpp
void LoadAllVisuals(EGridVisualContext Context);
```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## SetVisualsVisibility()

Toggles visibility of all tiles in grid.

```cpp
void SetVisualsVisibility(EGridVisualContext Context, bool Visibility);
```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.
``Visibility`` - Hide/Unhide.

---

## SetInstanceCulling()

Set culling distances for tiles. This reduces overdraw at eye-level perspectives.

```cpp

void SetInstanceCulling(EGridVisualContext Context, float StartCullDistance,
	float EndCullDistance);
```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.
``StartCullDistance`` - Start cull distance.
``EndCullDistance`` - End cull distance.

---

## AddTileVisualEditor()

Adds / updates visual to the tile in editor. Can be used to refresh a tile's visuals.

```cpp
void AddTileVisualEditor(const FIntPoint& TileIndex);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Tile to add visuals to.

---

## AddTileVisualRuntime()

Adds / updates visual to the tile in runtime. Can be used to refresh a tile's visuals.

```cpp
void AddTileVisualRuntime(const FIntPoint& TileIndex);
```

<span class="highlight-text-normal">Inputs</span>

``TileIndex`` - Tile to add visuals to.

---

## DestroyVisuals()

Destroys all tiles of the given context.

```cpp
void DestroyVisuals(EGridVisualContext Context);
```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## ProcessVisualsAsync()

Starts visual processing of all tiles. Tiles are updated or created depending on use case.

```cpp
void ProcessVisualsAsync(EGridVisualContext Context);
```

<span class="highlight-text-normal">Inputs</span>

``Context`` - Play context, whether runtime or editor.

---

## GetColorFromType()

Returns the color of a given ``TileType``.

```cpp
FVector4 GetColorFromType(FName TileType);
```

<span class="highlight-text-normal">Inputs</span>

``TileType`` - Tile type.

---

## GetColorFromState()

Returns the color of a given ``TileData``. Unlike ``GetColorFromType()``, this function considers both
``TileType`` and ``TileState`` depending on circumstance.

```cpp

FVector4 GetColorFromState(FTileData TileData);

```

<span class="highlight-text-normal">Inputs</span>

``TileData`` - Data of tile.

---


## Parameter Setters

The following functions simply set parameters in the relevant materials.


```cpp
void SetGridSize(FVector2D GridSize);
void SetTileCount(FVector2D TileCount);
void SetLineWidth(float Width);
void SetColor(FLinearColor Color, bool bIsLine);
void SetOpacity(float Value, bool bIsLine);
```

---