# Creating Custom Grid Shapes

In this section we'll go over how to create a non uniform shapeless grid. There are two ways to do this:

- Custom Block Based Grids.
- Custom Landscape Based Grids.

The second option isn't a true custom shaped grid because of the nature of landscapes.

## Block Based Grids

Block based grids refer to those that use individual tile "blocks" as their grid surface.

![alt text](<../images/MegaGrid - BlockBasedGrid - frame at 1m8s.jpg>)

### Steps

I'm going to briefly go over the process of setting this up, but you can skip this part entirely if you're using an example project. You can find the complete implementation inside the ``BP_PawnSpawner`` actor.

![alt text](<../images/bp-pawnspawner directory.png>)

1. Before we spawn anything, we need a **base** surface. This will act as a temporary surface so that we something trace to. Make sure this mesh obeys the [surface preparation](../getting-started/introduction.md#level-grid-creation) procedure.

    ![alt text](<../images/base surface.png>)

2. To spawn the "blocks" into the level, we can use a handy ``GridManager`` function called ``GenerateGridLocations()``. For best performance, I recommend spawning blocks as instances of a ``Instanced Static Mesh Component``, this will help scale the grid with no issues. 

    ![alt text](<../images/spawning blocks.png>)

    I'm not going into the specifics of the implementation because you can refer to the ``BP_PawnSpawner`` in the example project. Just make sure to set the ``GridManager`` reference. And hit the <span class="highlight-box-settings">Generate Blocks</span> button.

    ![alt text](<../images/Screenshot 2025-02-21 104802.png>)

3. If you've properly configured your **base** surface, you'll now have a lot of blocks in your level.

    ![alt text](<../images/Screenshot 2025-02-21 104746.png>)

4. Next you need to configure the blocks to obey the [surface preparation](../getting-started/introduction.md#level-grid-creation) procedure. This includes settting custom depth and collision responses for ``Instanced Static Mesh`` component.

    ![alt text](<../images/Screenshot 2025-02-21 105830.png>)

5. Once that's all done, you can populate the grid as you like.

    ![alt text](<../images/Screenshot 2025-02-21 110107.png>)

### Custom Shape

Now you know how to create a unifrom **Block Based** grid, creating custom grid shapes will be much easier. In fact the procedure is the exact same, the only difference is that we use a different **base** surface. So if I use a cut out plane like this:

![alt text](<../images/Screenshot 2025-02-21 110546.png>)

I get a grid like this.

![alt text](<../images/Screenshot 2025-02-21 110636.png>)
