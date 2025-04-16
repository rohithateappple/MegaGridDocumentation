# BP_MegaGridUnit

`BP_MegaGridUnit` is a blueprint only actor designed to serve as the base class for timeline based path movement logic. It maintains a high degree of customizability and modularity with the help of `BPI_UnitMovement` interface.

## Variables

``TArray<FVector> PathLocations``- Locations of all the tiles in the path. Set via `StartMovingUnitOnPath()`.

``AActor* Caller`` - Calling actor or target. Sends messages to this actor.

``float SpeedMultiplier`` - Playback rate for ``LerpTimeline``.

``TimelineComponent LerpTimeline`` - Timeline responsible for lerping location and rotation.

## StartMovingUnitOnPath() | *Interface Implemetation*

<span class="highlight-text-normal">Inputs</span>

``PathLocations`` - Locations of all the tiles in the path.

``Target`` - Target actor to send completion messages.

``SpeedMultiplier`` - Playback rate for Timeline.

---

## MoveUnit()

Moves and rotates the unit to the next tile location / rotation. Called recursively.

---

# BPI_UnitMovement

A blueprint interface responsible for managing path movement logic. These functions require the interface to be implemented.

## StartMovingUnitOnPath()

Type: Event and Function.

<span class="highlight-text-normal">Inputs</span>

``PathLocations`` - Locations of all the tiles in the path.

``Target`` - Target actor to send completion messages.

``SpeedMultiplier`` - Playback rate for Timeline.

---

## OnMovementComplete()

Type: Event and Function.

<span class="highlight-text-normal">Inputs</span>

``Unit`` - Actor who called this function.

``TileIndex`` - Last index of the path.

---


## OnTileEntered

Type: Event and Function.

<span class="highlight-text-normal">Inputs</span>

``Unit`` - Actor who called this function.

``TileIndex`` - Newly entered tile.

---