# **Welcome to MegaGrid Docs!**

![image_1](../images/megagrid_landscape.png)

MegaGrid is a powerful and flexible C++ plugin for Unreal Engine 5, designed to handle grid-based mechanics with precision and efficiency. Whether you're developing a turn-based strategy game, RTS, or an Open-World, MegaGrid provides an optimized solution for managing grids on both flat surfaces and complex landscapes.

## Features

- **Hex and Square Support.**

- **Landscape-Adaptive Grids** – Create landscape based defomable grids with the ability to remap surfaces.

- **Flat & Custom Surfaces** – Supports both uniform and non-uniform grid placements, making it ideal for various gameplay scenarios.

- **High-Performance Pathfinding** – Very quick pathfinding with async and realtime capabilities.

- **Dynamic Tiles** – Customize movement costs, tile types, and tile states. 

- **Scalable & Modular** – Designed for large-scale maps, ensuring smooth performance across tens of thousands of tiles. Supports upto *[300x300 tiles](#performance-note)*.

- **Grid Editor Mode** - For easy tile manipulation in the editor.

- **Multi-Level Grid Saves** - Uses .sav files to store grid data, making it modular across multiple levels.

- **Blueprint-Friendly API** – Easily integrate with Unreal Engine’s Blueprint system for rapid prototyping and customization.


MegaGrid is the ultimate grid solution for developers looking to create immersive, grid-based gameplay while maintaining high performance in large, open-world environments.


## Performance Note {#performance-note}
!!! note
    Although the grid itself supports upto 90,000 tiles. The realtime pathfinding is not recommended for bigger grids. However
    low frequency async pathfinding can be used with no issues! For more information, see [pathfinding](../grid-interactions/pathfinding.md#large-scale-pathfinding).