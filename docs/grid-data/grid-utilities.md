# Grid Utilities

## Overview

``GridUtils`` is a utility class that offers essential grid functionality and can be accessed from anywhere in the project. It’s particularly useful when you need grid-related functions but don’t have direct access to a ``GridManager`` instance.  

In C++, simply include ``GridUtils.h`` and call its functions using ``UGridUtils::``. In Blueprints, these functions are available globally.  

![alt text](<../images/get tile data utils bp.png>)  

!!! note  
    Utilities only provide getter functions to ensure data integrity.  

Several useful functions, such as ``GetSurfaceLocation()`` and ``TileUnderCursor()``, are frequently used throughout the project. For a full list of available functions, refer to the code documentation.

