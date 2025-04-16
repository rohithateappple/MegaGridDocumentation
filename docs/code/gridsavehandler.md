# AGridSaveHandler

This is a crucial class responsible for saving and loading grid data, as well as managing level-based grid systems.

## Variables

``FString SaveName;`` - **.sav** file name.

``bool bIsConstructed;`` - Flag to prevent redundant construction calls.

``FTimerHandle DelayTimerHandle;`` - Timer for delaying load data on construction. Doing this to avoid render pipeline crashes.


## LoadSavedData()

Loads grid data from the **.sav** file of the given name. Also loads or updates the visuals.

```cpp
void LoadSavedData(FString SaveFileName);
```

<span class="highlight-text-normal">Inputs</span>

``SaveFileName`` - Name of the **.sav** file.

---

## SaveData()

Saves grid data to the **.sav** file of the given name.

```cpp
void SaveData(FString SaveFileName);
```

## CopyGridSaveToPackagedFolder()

Copies the .sav files created during development to the final packaged directory. All .sav files that need to be saved must be pasted into ``YourGameName/Windows/ProjectName/GridSave`` of the packaged directory.

```cpp
void CopyGridSaveToPackagedFolder();
```


