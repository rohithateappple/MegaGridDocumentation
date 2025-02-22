# UGridSubsystem (Internal)

`UGridSubsystem` is an internal class of MegGrid, derived from `UEngineSubsystem`. It acts as a **mediator between `.sav` files and the rest of the plugin**.  

This class is **not directly accessible** to users, but it plays a key role in managing data persistence and grid states behind the scenes. Most other classes refer to this class for its core functions.


## Important Variables

`bool bIsRuntime` - Indicates whether Runtime is active.

`ECollisionChannel GridSurfaceChannel` - Trace channel for grid surface.

`ECollisionChannel AutoMapChannel`- Trace channel for auto-mapping.

## Important Functions

`bool GetIsRuntime()` - Returns ``bIsRuntime``.

`void SetRuntimeMode(bool bEnable)` - Sets `bIsRuntime`. Takes `bEnable`.





