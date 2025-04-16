# **Changelog**

Welcome to the changelog, here you can see updates and changes made to each version of MegaGrid. You can also find detailed explanations in their respective sections.

## Update v1.15 (3/29/2025)

🟡 Added ``Offset`` input to ``GetIndicesUnderBrush()`` for greater control.

🟡 Added ``UpdateTileTransform()`` to ``GridManager``. This allows to manipulate the tile location on a per instance basis.

🟡 Added ``UpdateVisualTransform()`` to ``GridVisuals``. Same as above but only affects visuals.

## Update v1.2 (4/16/2025)

🟡 Added ``BP_MegaGridUnit`` actor with new modular timeline based movement logic.

🟡 Added ``BPI_UnitMovement`` blueprint interface for handling unit based movement.

🟡 Added ``CopyGridSaveToPackagedFolder()`` function to ``BP_GridSaveHandler`` for quick and easy data packaging.

🟡 Updated ``BP_UnitManager`` in the example project to feature movement changes.

🟡 Updated ``BP_SaveHandler`` to feature ``CopyGridSaveToPackagedFolder()``.