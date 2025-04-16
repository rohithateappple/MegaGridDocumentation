# Saving Your Grid

Now that your grid is set up and populated with tiles, it's crucial to establish a reliable saving system. In this section, we'll cover best practices for storing grid-related data to maintain consistency and prevent data loss. Following these guidelines will ensure your grid remains intact across sessions, whether in the editor or a packaged game.

!!! warning
    The plugin **does not support auto-saving or runtime saves** by default, so you’ll need to manually save your grid data after making changes and implement a custom runtime save system as needed.

## Editor Saves

Anytime you make changes to the grid. Go to ``BP_SaveHandler`` and hit the <span class="highlight-box-settings">Save Grid</span> button.
It is essential that you do this as much as possible, since there are no auto-saves, you may risk losing your progress.

![alt text](../images/save-handler-savegributton.png)

!!! tip
    If you find it tedious to go to ``BP_SaveHandler`` everytime you make changes, I suggest you look in the direction
    of ``Editor Widgets`` and make a system that auto-saves every now and then. 

## Runtime Saves

By default, the plugin does not support runtime saving. This decision was intentional, as I wanted to separate editor data from runtime data. 
I realized that saving both types of data into the same plugin-specific .sav file might not be ideal for most cases. By keeping ``EditorState`` and ``RuntimeState``
separate, you gain more control during development. Instead of mixing all the data, you have a clear distinction between development data and gameplay data. 
For example, if you're testing how auto-mapping works and set up a detailed grid, you can hit play and experiment with custom implementations, 
such as modifying grid-related data. If anything goes wrong, you can easily revert to the editor state once you stop the play session. This separation 
ensures that you can focus on your development work without the concern of accidentally messing up core data.

However, for those wishing to take advantage of the existing MegaGrid save system, you'll need to make some modifications to the source code. Which is what we'll be covering now.

### Steps

1. First open up your code editor and navigate to ``GridSubsystem.cpp``, you can find it here:

    ![alt text](<../images/GridSubsystem directory.png>)

    If you cannot find the ``Plugins`` folder, you may have to **Generate Project Files** by right-clicking your .uproject in file explorer.

2. The changes we're concerned with are in ``UGridSubsystem::SaveGridData(FString SaveName)``:
    
    ```cpp

     // GridSubsystem.cpp

    void UGridSubsystem::SaveGridData(FString SaveName)
    {
    	// Saves all the subsystem variables to the given save game

    	UGridPluginSave* PluginSaveInstance = Cast<UGridPluginSave>(UGameplayStatics::LoadGameFromSlot(SaveName, 0));

    	UE_LOG(LogTemp, Warning, TEXT("UGridSubsystem::SaveGridData -> Save Name: %s"), *SaveName);

    	// Check if the save file already exists, if yes then proceed to save.
    	if (PluginSaveInstance)
    	{
    		PluginSaveInstance->SavedGridData = EditorState.GridData;
    		PluginSaveInstance->GridSize = GridSize;
    		PluginSaveInstance->LastGridSize = LastGridSize;
    		PluginSaveInstance->GridOffset = GridOffset;
    		PluginSaveInstance->bIsHex = bIsHex;
    		PluginSaveInstance->TileCount = TileCount;
    		PluginSaveInstance->StartIndex = StartIndex;
    		PluginSaveInstance->EndIndex = EndIndex;

    		PluginSaveInstance->LineWidth = LineWidth;
    		PluginSaveInstance->LineColor = LineColor;
    		PluginSaveInstance->LineOpacity = LineOpacity;
    		PluginSaveInstance->TileOpacity = TileOpacity;
    		PluginSaveInstance->TileColor = TileColor;

    		UGameplayStatics::SaveGameToSlot(PluginSaveInstance, SaveName, 0);
    	}

    	// Else, create a new one with default values.
    	else
    	{			
    		CreateSaveData(SaveName);
    	}

    }
    ```

    ``PluginSaveInstance->SavedGridData = EditorState.GridData;`` -> In this line, we're only saving to the .sav file from the EditorState.
    If we want to save from the ``RuntimeState`` as well, we can simply check if we're in runtime, if yes then save from the ``RuntimeState``.

    ```cpp

    // GridSubsystem.cpp

    void UGridSubsystem::SaveGridData(FString SaveName)
    {
    	// Saves all the subsystem variables to the given save game

    	UGridPluginSave* PluginSaveInstance = Cast<UGridPluginSave>(UGameplayStatics::LoadGameFromSlot(SaveName, 0));

    	UE_LOG(LogTemp, Warning, TEXT("UGridSubsystem::SaveGridData -> Save Name: %s"), *SaveName);

    	// Check if the save file already exists, if yes then proceed to save.
    	if (PluginSaveInstance)
    	{
    		PluginSaveInstance->SavedGridData = bIsRuntime ? RuntimeState.GridData : EditorState.GridData;
    		PluginSaveInstance->GridSize = GridSize;
    		PluginSaveInstance->LastGridSize = LastGridSize;
    		PluginSaveInstance->GridOffset = GridOffset;
    		PluginSaveInstance->bIsHex = bIsHex;
    		PluginSaveInstance->TileCount = TileCount;
    		PluginSaveInstance->StartIndex = StartIndex;
    		PluginSaveInstance->EndIndex = EndIndex;

    		PluginSaveInstance->LineWidth = LineWidth;
    		PluginSaveInstance->LineColor = LineColor;
    		PluginSaveInstance->LineOpacity = LineOpacity;
    		PluginSaveInstance->TileOpacity = TileOpacity;
    		PluginSaveInstance->TileColor = TileColor;

    		UGameplayStatics::SaveGameToSlot(PluginSaveInstance, SaveName, 0);
    	}

    	// Else, create a new one with default values.
    	else
    	{			
    		CreateSaveData(SaveName);
    	}

    }
    ```
    With this change, the plugin can save in runtime as well! 


3. To save in runtime, you can use a similar setup as you see below. I've made it so that pressing ``CTRL + S`` in-game will save any changes made. Do keep in mind that these changes are ultimately saved to file (.sav). So they will be reflected in the editor as well. If you don't see the changes after you stop PIE, you simply have to press the <span class="highlight-box-settings">Load Grid</span> button via ``BP_SaveHandler``this will refresh the visuals.

    ![alt text](<../images/simple runtime save.png>)

!!! note
    If you're using this system make sure you keep MegaGrid data isolated from the rest of your game.

## Saving Multi-Level Grids {#saving-multi-level-grids}

MegaGrid supports multi-level grids, allowing each level to read from its own `.sav` file. However, this system follows strict guidelines. In the editor, you must manually save the current level's grid before opening another level. This can be done by clicking the <span class="highlight-box-settings">Load Grid</span> button in `BP_SaveHandler`. While loading happens automatically, saving does not.  

Handling level changes at PIE is more complex and requires modifying the source code. Currently, the `GridSaveHandler` class saves or loads data in `BeginPlay()`, depending on the play context—if in the editor, it saves; otherwise, it loads. This workflow is designed for both development and packaged games. However, testing level switching in PIE (Play In Editor) presents challenges, as the default system does not fully support this scenario.  

Fortunately, there's a simple fix. In `void AGridSaveHandler::BeginPlay()`, adding a small delay before calling `SaveData()` ensures the previous level fully unloads before the new level loads its data, preventing any conflicts.

```cpp

// Before
// GridSaveHandler.cpp

void AGridSaveHandler::BeginPlay()
{
	
	Super::BeginPlay();

	GridSubsystem = UGridUtils::GetGridSubsystem();

	/// NOTE: For editor performance, try manually
	/// saving before begin play, takes 3 seconds for 320^2.
	/// SaveData is only called in the editor. Ensure to manually save the data before packaging the game.

#if WITH_EDITOR
	SaveData(SaveName); 
#endif

	/// NOTE: LoadData is only called in packaged builds.
	/// Ensure that the SaveName is specified in the editor properties before shipping.

#if UE_BUILD_DEBUG || UE_BUILD_DEVELOPMENT || UE_BUILD_SHIPPING || UE_BUILD_TEST
	GridSubsystem->LoadGridData(SaveName);
#endif

}

```

```cpp

// After
// GridSaveHandler.cpp

void AGridSaveHandler::BeginPlay()
{
	
	Super::BeginPlay();

	GridSubsystem = UGridUtils::GetGridSubsystem();

	/// NOTE: For editor performance, try manually
	/// saving before begin play, takes 3 seconds for 320^2.
	/// SaveData is only called in the editor. Ensure to manually save the data before packaging the game.

if WITH_EDITOR

	FTimerManager& TimerManager = GetWorld()->GetTimerManager();
	TimerManager.SetTimer(
		DelayTimerHandle,
		[this]()
		{
			SaveData(SaveName);
		},
		0.06f,  // Add a slight delay to avoid save overlaps.
		false
	); 

#endif

	/// NOTE: LoadData is only called in packaged builds.
	/// Ensure that the SaveName is specified in the editor properties before shipping.

#if UE_BUILD_DEBUG || UE_BUILD_DEVELOPMENT || UE_BUILD_SHIPPING || UE_BUILD_TEST
	GridSubsystem->LoadGridData(SaveName);
#endif

}

```
Next, we need to manually call `LoadGrid()` in the `BeginPlay()` function of `BP_SaveHandler`, also with a slight delay. The exact duration may vary, but it should never need to be high enough to impact game performance.

![alt text](<../images/savehandeler bp_begin play.png>)

With this you can switch levels in any scenario, both PIE and packaged games. In fact this is the same system used in the demo game.

## Maintaining Backups

Despite thorough testing, some edge cases may naturally go unnoticed due to the complexity of the project. The save system, in particular, has been carefully designed to ensure data integrity, and following the best practices outlined above will help prevent issues. However, as with any system, prevention is better than cure.  

To safeguard your progress, it's important to maintain stable copies of your saves in a backup folder. Fortunately, this is extremely easy to do! The `.sav` files are stored in the following directory: ``YourProjectName / Saved / SaveGames``. Simply copy your saved files and store them in a separate backup folder. This way, if anything ever goes wrong, you can easily revert to a previous state.

![alt text](<../images/save location.png>)

## Packaging Saves

MegaGrid relies heavily on `.sav` files, which function seamlessly within the editor. However, an important consideration arises when packaging your game—by default, Unreal Engine does not include `.sav` files in the packaged build. This means any progress made in the editor will not carry over to the final game.  

Fortunately, there’s a straightforward solution: copying the existing save files into Unreal’s default save location when the packaged game runs. 

1. `BP_GridSaveHandler` already does the copying part for you in it's `BeginPlay()` via the `CopyGridSaveToPackagedFolder()` function. Now all you need to do is copy the .sav files into the appropriate directories.

	![alt text](<../images/Screenshot 2025-04-16 201749.png>)

2. Once you've packaged your game, navigate to your packaged folder (here I'm using MegaGrid Demo as example). 

	![alt text](<../images/packaged directoy.png>)

3. Within it, open your game folder and create a new ``GridSave`` (Target) folder. 

	![alt text](<../images/packaged game dorectory.png>)

4. In it, copy all your MegaGrid .sav files from ``YourProjectName / Saved / SaveGames``.

	![alt text](<../images/GridSave directory.png>)

5. Now when you open your game, everything should work as expected. If you take a look at the defaut save folder
(usually ``C:\Users\YourUser\AppData\Local\YourGame\Saved\SaveGames``), you can see the copied files. 

	![alt text](<../images/game save directory.png>)

The key advantage of this approach is that it only copies the necessary files if they are not already present in the save folder. If the required files exist, the copy process is skipped, ensuring that your runtime progress remains intact every time the game launches. 

!!! Tested
	This system is currently used in the MegaGrid Demo game.



