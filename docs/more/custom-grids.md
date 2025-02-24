# Creating Custom Grid Shapes

This section covers how to create a non-uniform, shapeless grid. There are two main approaches:  

- **Custom Block-Based Grids**  
- **Custom Landscape-Based Grids**  

The second method isn't a fully custom-shaped grid due to the inherent limitations of landscapes.

## Block Based Grids

Block based grids refer to those that use individual tile "blocks" as their grid surface.

![alt text](<../images/MegaGrid - BlockBasedGrid - frame at 1m8s.jpg>)

### Steps

I'll quickly walk you through the setup process, but if you're using the example project, you can skip this section. You can find the full implementation in the ``BP_PawnSpawner`` actor.

![alt text](<../images/bp-pawnspawner directory.png>)

1. Before spawning anything, you'll need to create a **base** surface. This surface will serve as a temporary reference for tracing. Ensure that this mesh follows the [surface preparation](../getting-started/introduction.md#level-grid-creation) procedure for proper grid setup.

    ![alt text](<../images/base surface.png>)

2. To spawn the "blocks" into the level, you can use the `GenerateGridLocations()` function provided by the `GridManager`. For optimal performance, it’s best to spawn blocks as instances of an `Instanced Static Mesh Component`. This approach will help efficiently scale the grid without performance issues.

    ![alt text](<../images/spawning blocks.png>)

    I won’t dive into the full implementation details, as you can refer to the `BP_PawnSpawner` in the example project for that. Just ensure that the `GridManager` reference is set properly, and then click the <span class="highlight-box-settings">Generate Blocks</span> button to proceed.

    ![alt text](<../images/Screenshot 2025-02-21 104802.png>)

3. Once your **base** surface is properly configured, you should see a lot of blocks populated across your level.

    ![alt text](<../images/Screenshot 2025-02-21 104746.png>)

4. Next, configure the blocks to follow the [surface preparation](../getting-started/introduction.md#level-grid-creation) steps. This involves setting custom depth and adjusting collision responses for the ``Instanced Static Mesh`` component.

    ![alt text](<../images/Screenshot 2025-02-21 105830.png>)

5. Once everything is set up, you can fill the grid with your desired content.

    ![alt text](<../images/Screenshot 2025-02-21 110107.png>)

### Custom Shape

Now that you know how to create a uniform **Block-Based** grid, creating custom grid shapes will be much simpler. The process remains the same, with the only difference being the use of a different **base** surface. For example, if you use a cut-out plane like this:

![alt text](<../images/Screenshot 2025-02-21 110546.png>)

You get a grid like this.

![alt text](<../images/Screenshot 2025-02-21 110636.png>)

## Managing Tile Leaks

In this picture, you can see that the tile visuals don’t align perfectly with the grid lines. This happens because, by default, the instance scale might be slightly off.

![alt text](<../images/Screenshot 2025-02-21 115346.png>)

You can easily fix this by lowering the **Highlight Scale Multiplier** values in `BP_GridVisuals`. After making the adjustment, press the <span class="highlight-box-settings">Force Reload</span> button in `BP_GridManager` to refresh the visuals.
![alt text](<../images/Screenshot 2025-02-21 115516.png>)

![alt text](<../images/Screenshot 2025-02-21 115549.png>)

Voila!

![alt text](<../images/Screenshot 2025-02-21 115538.png>)

## Custom Landscape Grids

The plugin doesn't fully support custom grids on landscapes yet. To work around this, you can either manually cut out sections of the landscape or restrict tile generation in specific areas. However, selectively removing parts of the grid lines isn't possible at the moment.  

Let's go over how to prevent tile generation in certain areas.

### Steps

1. The process is similar to setting up a standard landscape grid. Follow these [steps](../getting-started/introduction.md#level-grid-creation) to prepare your landscape properly.

2. After that, you'll need a **blocking** mesh to prevent tile generation. Here, I'm using a simple plane—just place it over the area you want to block.

    ![alt text](<../images/Screenshot 2025-02-21 140117.png>)

3. Make sure to configure the collision settings so the mesh is detected during tracing. However, do **not** add the "GridSurface" actor tag.

    ![alt text](<../images/Screenshot 2025-02-21 140311.png>)

4. Open ``BP_GridManager`` and press the <span class="highlight-box-settings">Generate Grid</span> button. You’ll notice that the area beneath the plane no longer generates tiles.

    ![alt text](<../images/Screenshot 2025-02-21 140428.png>)

### Custom Highlight Materials

If you absolutely need to remove the grid lines, there's a workaround. This method will eliminate all grid lines entirely.  

The trick is to make the tile highlight itself resemble a grid line. You can achieve this by assigning a different texture to the highlight material instance.

#### Steps

1. First set the line opacity to **0** via ``BP_GridManager``.

    ![alt text](<../images/Screenshot 2025-02-21 145400.png>)

2. Locate the desired highlight material instance in ``Plugins/MegaGrid Content/Materials/``.
    
    ![alt text](<../images/Screenshot 2025-02-21 144322.png>)

3. Assign a different texture, here I'm using an outline texture.

    ![alt text](<../images/Screenshot 2025-02-21 144336.png>)

4. Go to ``BP_GridVisuals`` and enable ``Custom Highlight Material`` and assign the new material instance to the **Highlight Material** reference. In this case I'm using a square grid, so to ``Square Highlight Material``.

    ![alt text](<../images/Screenshot 2025-02-21 144358.png>)
    
    ![alt text](<../images/Screenshot 2025-02-21 144426.png>)

5. Now simply head to ``BP_SaveHandler`` and hit ``Save Grid`` and then ``Load Grid``. You'll now have a landscape grid that conforms to any shape.

    ![alt text](<../images/Screenshot 2025-02-21 143805.png>)

    The only disadvantage of this method is that you lose anti-aliasing properties of Zero Aliasing Grid and cannot dynamically make modifications to say, line width or tile color. 


### Custom Grid Materials

While we're discussing custom grids, you can also assign custom post-process (grid lines) materials to the grid if needed. To do so, go to ``BP_GridVisuals`` and enable the ``Custom Grid Material`` option. This allows you to assign your own materials to either the **Hex Grid Material** or **Square Grid Material** fields.

!!! note
    If you use your own grid material, you'll lose the Zero Aliasing Grid features.

![alt text](<../images/Screenshot 2025-02-21 151055.png>)