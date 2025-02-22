# Recalculating Tile Locations

## Overview 

Sometimes, during development with landscape-based grids, you may realize that your landscape needs adjustments. However, when you sculpt or switch the landscape, your existing grid will lose its visual alignment. In such cases, you need to **recalculate** each tile's position to ensure it aligns properly with the new grid surface.

!!! note
    This method isn't limited to just landscape-based grids; it can be used to fix any inconsistencies on any grid surface! The above is just an example.
    

## Steps

1. Here's a landscape-based grid populated with random obstacles. 

    ![alt text](<../images/Screenshot 2025-02-22 185007.png>)

2. After some changes to the landscape, you may notice that the tile visuals no longer align with the new adjustments.

    ![alt text](<../images/Screenshot 2025-02-22 185207.png>)

3. Don’t worry! The solution is simple: head to the ``BP_GridManager`` and click the <span class="highlight-box-settings">Recalculate Surface</span> button.

    ![alt text](<../images/Screenshot 2025-02-22 185346.png>)

4. This will automatically remap the positions of the existing tiles to fit perfectly with the new surface.

    ![alt text](<../images/Screenshot 2025-02-22 185543.png>)
