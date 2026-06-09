# Basic knowledge related to `Arma3` and `Eden Editor`

😃 Please install Arma3 via Steam under C Drive of your Windows PC in advance!

* This document explains the knowledge required to understand, use, and improve the `G-MAD` in the future.
* Most of the content in this document is also available on the official Arma3 online community wiki, but because the wiki is extensive, only the necessary parts are summarized here.
* For more information than what is included in this document, please refer to the [Arma3 Online Community Wiki](https://community.bistudio.com/wiki/Category:Arma_3:_Scripting_Commands).
* If there are any errors in this document or if you would like to add more information, please feel free to contact us.

## 0. Preparation

* A basic `parameter.txt` file must be placed under `C:/Users/{username}/Documents/Arma 3`.
* Move the `missions` folder included in the code to `C:/Users/{username}/Documents/Arma 3/missions`.
* Note that while the code is running, you must not open or modify any files that the code uses or creates.

## 1. Arma3?

Arma3 allows users to create various missions through the features of the EDEN Editor and scripts. Arma3 players can create their own missions using the editor and distribute them through the export function.

The image below shows the main screen of Arma3. Depending on the applied mods and settings, it may look slightly different.

<p align="center">
    <img alt="Welcome" src="./figs/md_img/main_screen.png" />
</p>

When you move the mouse cursor over the area where there is one person on the screen, the following screen appears.

<p align="center">
    <img alt="Welcome" src="./figs/md_img/main_screen_cursor.png" />
</p>

On this screen, you can use `EDITOR` to create and debug the desired mission, and then run it through `SCENARIO` to obtain the desired dataset.

> ⚠️ You must distinguish between `EDITOR` and `SCENARIO`. The files used by `EDITOR` are located in the Documents folder (`Documents\ARMA 3`), while the folders used by `SCENARIO` are located under the ARMA 3 folder inside the Steam directory (`C:\Program Files (x86)\Steam\steamapps\common\Arma 3`). There may be multiple `EDITOR` folders depending on the user profile, but there is only one folder used by `SCENARIO`.

> ⚠️ The mission folder name handled in `EDITOR` is `mission`, while the mission folder name handled in `SCENARIO` is `Mission` (the first letter differs in lowercase/uppercase).

> ⚠️ When running a scenario from the default profile, screenshots are saved in the `Documents\Arma 3\Screenshots` folder. However, when running it from another profile, screenshots are saved in `Documents\Arma 3 - Other Profiles\{ProfileName}\Screenshots`.

> ⚠️ The reason for distinguishing between the two is error handling. If you run a scenario created through the `EDEN Editor`, all errors are displayed, so production may stop due to non-critical errors. However, if you export the created scenario and then run it through `Scenarios`, errors are not printed during actual gameplay, so scene production can continue without being interrupted by errors. In addition, in `Scenarios`, various warnings can be ignored through mods.


## 2. Workflow of `G-MAD`

At a high level, `G-MAD` operates as follows.

1. It receives user-specified information such as camera type, time of day, map, weather, image size, sampling rate, and so on.

2. It creates a mission based on the user's information.

3. It moves the mission created by the user to the Steam directory and runs it through `Play Scenario`.

4. While Arma3 is running, Python reads and organizes the folders containing the generated scenario files.

5. When data generation is finished, it cleans up dummy files.

Among these steps, steps 2 and 3 require knowledge of the Arma3 game itself rather than the SQF language.


## 3. `Mission` creation

Whether you edit a mission in the `EDEN Editor` or later work under the Steam directory, when the created mission is opened, the basic execution flow inside Arma3 is as follows.

1. The `mission.sqm` file is opened.
    
    * The `mission.sqm` file contains basic item placement, basic weather, and related information.
2. The `init.sqf` file is executed.

    * The `init.sqf` file is automatically executed after the `mission.sqm` file is loaded.
    * The `init.sqf` file contains SQF commands that execute files such as `play1.sqf`, `play2.sqf`, and so on.

3. Code that creates the scene, such as `play1.sqf`, is executed.

Since `init.sqf` is generated in Python, this section will cover the `mission.sqm` file that is created and distributed within Arma3.

### 3.1 Writing `mission.sqm`

The `mission.sqm` files are created and distributed by the developer. They contain one basic player and a Zeus game master placement.  
However, because they contain elements that affect the overall map and the data, this document will explain them briefly.

After placing a basic character and a Zeus game master in the EDEN Editor, open the General section under Attributes as shown in the image below.

<p align="center">
    <img alt="Welcome" src="figs\md_img\edenEditor_Attribute.png" />
</p>

When opened, the following window appears.

<p align="center">
    <img alt="Welcome" src="figs\md_img\edenEditor_General.png" />
</p>

The parts that require attention are the States and Misc sections.

When you click States, the following window appears.

<p align="center">
    <img alt="Welcome" src="figs\md_img\edenEditor_General_States.png" />
</p>

The settings should be checked as shown in the screenshot above.  
The Show briefing/debriefing options are processes that explain related information before and after the mission starts. Since they are unnecessary, both should be unchecked.  
In particular, disabling debriefing is important because the game returns to the main screen after data generation only when that option is unchecked. Therefore, you should check these two options carefully.  
The remaining settings were left at their default values.

In the Misc section, there is an item like the one below.

<p align="center">
    <img alt="Welcome" src="figs\md_img\edenEditor_General_Misc.png" />
</p>

In this section, `Binarize the Scenario File` must be unchecked. This does not have a major impact, but if binarization is enabled, the `mission.sqm` file is saved in an unreadable format.  
There is also an option to binarize when saving the `mission.sqm` file, so make sure to check it when saving.

There are many other settings, but these seem to be the important ones.

Among the current maps, the maps whose `mission.sqm` files have been modified are Altis, Malden 2035, Stratis, Weferlingen, and Weferlingen (Winter). The modified `mission.sqm` files are located in the mission folder of the project.

Since the purpose of this document is to explain Arma3 itself, it will skip writing files such as `init.sqf`.

## 4. MOD and DLC Management

In Arma3, users subscribe to various mods from the Steam user community and load those mods into the game.  
You must distinguish between DLCs and mods, and also between subscribing to a mod and loading a mod. The differences are as follows.

> DLC &rarr; officially distributed by Arma3  
> MOD &rarr; distributed through the Steam user community  
> Subscribing to a mod in the Steam community &rarr; automatically downloads the mod to disk. It is not applied to the game.  
> Loading a mod into the game &rarr; downloads the mod to disk and applies it to the game.

To subscribe to a MOD, go to the Steam Library, enter the Community Hub, and then go to the Workshop, where various MODs are available.  
After finding the desired mod, click Subscribe, and it will be downloaded automatically.

<p align="center">
    <img alt="Welcome" src="figs\md_img\steamCommunityHub.png" />
</p>

<p align="center">
    <img alt="Welcome" src="figs\md_img\steamCommunity.png" />
</p>

The MODs and DLCs currently subscribed to and loaded in `G-MAD` are as follows.

* DLC
    1. GM (Global Mobilization)
    1. AoW (Art of War)

* MOD
    1. **3CB Factions**
    1. **A3 Thermal Improvement**;
    1. **Automatic Warning Suppressor**
    1. CBA_A3
    1. **Cold War Rearmed III**
    1. **CUP Terrains - Core**
    1. CUP Units
    1. CUP Vehicles
    1. CUP Weapons
    1. Debug Console
    1. **Global Mobilization - Cold War Germany- Compatibility Data for Non-Owners**
    1. **Pook ARTY Pack**
    1. **POOK Camonets**
    1. **POOK SAM PACK**
    1. RHSAFRF
    1. RHSGREF
    1. RHSSAF
    1. RHSUSAF
    1. **Global Mobilization - Cold War Germany - Community Language Pack**

DLCs are located under `C:\Program Files (x86)\Steam\steamapps\common\Arma 3` with folder names such as GM and AoW. Mods are located under `C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop` with folder names such as @CBA_A3.

Inside each folder, the data is organized as `.pbo` files. These files can be inspected using a program such as [`Pbo Manager`](https://github.com/winseros/pboman3). In particular, they contain `.p3d` files, which are 3D objects, as well as various textures. Therefore, the contents of mods can be created or used in other programs with 3D tools such as `Blender`.


## 5. Startup Parameters

This is an important part that enables automatic execution of `G-MAD`.  
Using startup parameters, you can run the created scenario with a single command-line execution.

Detailed information can be found in [Arma3: Startup Parameters](https://community.bistudio.com/wiki/Arma_3:_Startup_Parameters), but this document describes only the parameters needed for `G-MAD` and those that may be applicable in the future.

In general, strings are written in the form `"text"`. For paths, use backslashes as in `C:\User\"ARMA 3"\`, and if a folder name contains spaces, enclose it with `\"`.

### 5.1 Performance-Related Parameters

| parameter | Parameter explanation | ✓ / ✗ / ？|
|---|---|---|
|-enableHT | Enable HyperThreading and Multicore Processing | ✓|
|-malloc=\<string> | Change memory allocation type | ✗ works well but is not currently used|
|-hugePages| Change page size from 4 KB to 2 MB | ✗ used with -malloc, but does not work well|
|-noPause | Enable the game to keep running in the background | ✓|
|-noPauseAudio| Enable game sound to keep running in the background | ✓|
|-filePatching| Enable unloaded mods to be loaded | ✓, but the unloaded mod must be subscribed|
|maxFileCashSize=\<int>| Maximum size of file cache | ？ Not sure whether it works|
|-mods=\<string> | Mod to be loaded | ✗ use -par instead|
|-par=\<path> | Use parameters in `XXX.txt` | ✓|


### 5.2 Gameplay and Debug-Related Parameters

| parameter | Parameter explanation | ✓ / ✗ / ？|
|---|---|---|
|-skipIntro| Skip the ARMA III logo | ✓|
|-noSplash | Skip the Nvidia and other logos | ✓ |
|-window| Force the game to run in windowed mode| ✓|
|-init=\<sqf script>| Execute an SQF script at the main menu | ✓|
|-debug| Enable a specific debug mode| ✓|
|-noLogs| Stop writing logs|✓|
|-posX| X coordinate of the top-left corner of the window|✓|
|-posY| Y coordinate of the top-left corner of the window|✓|

Debug files are saved as `***.rpt` files under `C:\Users\{username}\AppData\Local\Arma 3\`.

The ARMA 3 executable is `C:\Program Files (x86)\Steam\steamapps\common\Arma 3\arma3_x64.exe`. Therefore, entering a command like the one below automatically loads mods and runs the mission.

```powershell
# on Windows Powershell or Terminal
C:\Program Files (x86)\Steam\steamapps\common\Arma 3\arma3_x64.exe -skipIntro -noSplash -window -enableHT -hugePages -noPause -noPauseAudio -filePatching -maxFileCacheSize=2048 -debug -par=C:\Users\{username}\Documents\"Arma 3"\parameter.txt
```

For reference, `parameter.txt` looks as follows.

```text
-posX=20
-posY=20
-mod="GM;AoW;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@CBA_A3;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@CUP Weapons;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@RHSUSAF;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@RHSAFRF;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@CUP Weapons;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@CUP Units;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@RHSGREF;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@RHSSAF;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@CUP Vehicles;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@CUP Terrains - Core;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@3CB Factions;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@Cold War Rearmed III;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@A3 Thermal Improvement;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@Pook ARTY Pack;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@POOK Camonets;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@POOK SAM PACK;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@Global Mobilization - Cold War Germany- Compatibility Data for Non-Owners;C:\Program Files (x86)\Steam\steamapps\common\Arma 3\!Workshop\@Automatic Warning Suppressor;"
```

For `parameter.txt`, the important points are that mods must be separated with `;`, there must be no spaces (` `) or line breaks inside the string, and all entries must be written continuously. Also, paths can be relative or absolute, but absolute paths are recommended for MODs.

### 5.3 Summary of Map-Related Mods in the `mission` Folder

There are various maps in the current mission folder. Some of these require purchasing ARMA 3 Ultimate Edition or downloading specific MODs. The corresponding information is summarized in the table below. If you want to use various maps, please subscribe to the relevant mods or purchase Ultimate Edition.

Map-adding mods:
1. CUP Terrains - Maps
1. CUP Terrains - Maps 2.0
1. CUP Terrains - CWA


| Folder Name | Map Name | Arma3 / DLC / MOD |
|---|---|---|
|altis.Altis|Altis| Arma3|
|bukovina.Bootcamp_ACR|Bukovina|CUP Terrains - Maps|
|bystrica.Woodland_ACR|Bystrica|CUP Terrains - Maps|
|chernarus_autumn.chernarus | Chernarus 2020|CUP Terrains - Maps 2.0|
|chernarus_summer.chernarus_summer|Chernarus Autumn|CUP Terrains - Maps|
|chernarus_winter.Chernarus_Winter|Chernarus Winter|CUP Terrains - Maps|
|everon.eden|Everon|CUP Terrains - CWA|
|kolgujev.cain|Everon|CUP Terrains - CWA|
|livonia.Enoch|Livonia|Arma3 Contact|
|malden.abel|Malden|CUP Terrains - CWA|
|malden2035.Malden|Malden 2035|Arma3 Free DLC|
|nogova.noe|Nogova|CUP Terrains - CWA|
|proving_grounds.ProvingGrounds_PMC|Proving Grounds|CUP Terrains - Maps|
|sahrani.sara|Sahrani|CUP Terrains - Maps|
|shapur.Shapur_BAF|Shapur|CUP Terrains - Maps|
|southern_sahrani.saralite|Southern Sahrani|CUP Terrains - Maps|
|stratis.Stratis|Stratisis| Arma3|
|takistan.takistan|Takistan|CUP Terrains - Maps|
|takistan_mountains.Mountains_ACR|Takistan Mountains|CUP Terrains - Maps|
|tanoa.Tanoa|Tanoa|Arma3 Apex|
|united_sahrani.sara_dbe1|United Sahrani|CUP Terrains - Maps|
|weferlingen.gm_weferlingen_summer|Weferlingen|Arma3 Global Mobilization| 
|weferlingen_winter.gm_weferlingen_winter|Weferlingen (Winter)| Arma3 Global Mobilization|
|||

