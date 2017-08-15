-----------------------------------------------------------------------------------------------------

---------- Hero Support Script for StarWars: BattleFront 2 ----------  
Version 1.2

Script by T. Simpson  
Screen names: Archer01 or Theodranis  
Email: archer01_extra@hotmail.com  
Date: Sept 26, 2006  

This script adds AI hero support to a custom or modded map.

Sample scripts have been included to show how it works. The Conquest sample has a bunch of comments explaining everything. The CTF version does not include the comments, so it is a better example of how your script should "look".

Good luck!

Sample scripts:  
Example_con.lua -- Commented sample  
Example_ctf.lua -- Uncommented sample  
Display_Fix.lua -- Display fix sample (see below for details)  



Note: Do NOT edit the AIHeroSupport script. Override variables from your own script if needed. If you find a bug, please inform me so that I may fix it.

-----------------------------------------------------------------------------------------------------

## Tutorial

These are the steps you need to follow to add the script to your map...

*Setting up the script so your map lua can use it:

1. Go into the "\Common" folder for your map
2. Find the "mission.req" file and open it in a text/script editor
3. You should see a few lists of items each contained in quotaion marks
4. Find the list that begins with "script"
5. Add "AIHeroSupport" to the end of the script list
6. Save and close the file
7. Copy the "AIHeroSupport.lua" file into your map's "\Common\scripts" folder
8. The script can now be used in your map's lua

*Setting up your map's lua:

1. Open your map's lua file in text/script editor
2. You will see a bunch of "ScriptCB_DoFile(something here)" commands at the top
3. Add this just after them:

      ScriptCB_DoFile("AIHeroSupport")

4. Now further down in the script find the line that says something like "conquest:Start()" or "ctf:Start()". Right after this line is where the hero support code should be added...
5. First add this line:

      herosupport = AIHeroSupport:New{AIATTHeroHealth = 5000,	AIDEFHeroHealth = 3000, gameMode = "NonConquest",}

- Note: The stuff in the curly braces are the different settings the script uses. It is a list of variables seperated by commas. All the variables you can set here have default settings, so if you don't set a variable yourself the default setting is used. See below for a list of the variable names and what they do. Variable names ARE case sensitive.

6. After adding that line, you then set your hero classes using:

      herosupport:SetHeroClass(team, "heroClassName")

-Note: If you have more than 7 unit types (including the hero type) on a side, the info boxes for them will not be sized properly during gameplay (the list will go off the end of the screen). There is an easy fix for this. Finish the tutorial, then read the section below relating to this problem.

7. After setting the hero classes, you then need to tell the script the names of the CPs and spawn paths it should use for the heroes. Do that by adding a line like the following for every CP that you want the heroes to be able to spawn at:

      herosupport:AddSpawnCP("CPname","SpawnPathNameForTheCP")

8. After all the CPs have been added, add this line:

      herosupport:Start()

9. Now this is a VERY important part, the "SetHeroClass" function can only be used once for each team. If it is used a second time, nasty stuff will likely happen. Go through the script and remove all uses of the "SetHeroClass" function, except for the two you added earlier of course, so there won't be any problems. Your final script should have at most only two (2) mentions of "SetHeroClass". Don't forget about your text editor's search function when making sure this is so.

10. That's it. Munge your map, and try it out!

-----------------------------------------------------------------------------------------------------

## Unit List Display Issue

When more than 7 unit types are available for selection (ie more than the standard 6 normal and 1 hero), the display boxes in the spawn screen will not be sized correctly if the code block described above is left that way. There are a variety of ways to fix this, but the simplest (and most practical) is as follows:

1. Start with the first three (3) lines of the hero code block -> the "AIHeroSupport:New{}" and the two "herosupport:SetHeroClass()" lines.

2. Now simply CUT those three lines, and paste them at the very end of the "ScriptInit()" function. If you don't know where that spot is... It is just after the "AddCameraShot()" function calls, but just before the "end" statement at the bottom of the file (that "end" statement marks the end of the function). That will fix the display problem.

Note: Make sure you move ONLY those three lines. The "AddSpawnCP" and "Start" functions may not (and likely won't) work correctly if you move them too. Leave those lines where they are.

See the packaged sample script if you need an example.

-----------------------------------------------------------------------------------------------------

## Script Variable List

Variable name: gameMode
- Possible values: (possible values below)
- Default = "NonConquest"

This should be set to the gameMode the map is using. It can be set to any of the following:

"Conquest" -- Uses the bot count from the Conquest map select settings  
"ConquestStyle" -- Like Conquest, but ignores bot count settings  
"UberConquest" -- Use this if it is a conquest map that uses UberMode  
"CTF" -- Uses the bot count from the CTF map select settings  
"Assault" -- Uses the bot count from the Assault map select settings  
"Hunt" -- Uses the bot count from the Hunt map select settings  
"xl" -- Uses this if it is an XL map that uses points  
"NonConquest" -- Like any non-conquest, but ignores bot count settings  

NOTE: For saftey sake, only use the conquest-related gamemodes if the map tracks/uses the reinforcement count. For maps that use team "points" use the other modes.


Variable name: AIATTHeroHealth  
- Possible values: (a number)  
- Default = the hero unit's normal health  
The health the attacking team's bot hero should have (does not affect human hero's health). If this variable is not set, the hero's normal health is used.  

Variable name: AIDEFHeroHealth  
- Possible values: (a number)  
- Default = the hero unit's normal health  
Same, but for the defender's hero.  

Variable name: minSpawnTime  
- Possible values: (a number in seconds)  
- Default = 25 seconds  
The minmal delay between a bot Hero death and it's respawn. This is only the "minimum" amount of time. Usually the bot will spawn in just fine, but not every spawn attempt will end successfully. In the unsuccessful case (team full, unit memory not fully cleared, etc), it may take a little longer for the actual spawn to occur.  

Variable name: initSpawnTime  
- Possible values: (a number in seconds)  
- Default = 10 seconds  
This is like minSpawnTime but is only used once at the start of the match. This is how long from match-start before the first bot hero spawn attempt.  

Variable name: minAfterHuman  
- Possible values: (a number in seconds)  
- Default = 25 seconds  
This is like minSpawnTime but is used after a human controlled hero dies.  

Variable name: botImmuneHeroes  
- Possible values: (true or false)  
- Default = false  
Make it so the bot heroes can only be killed by a human player.  

Variable name: botDamageThreshold  
- Possible values: (a number between 0 and 1)  
- Default = 0.3  
If botImmuneHeroes is set to true, this is how much health the hero unit will have remaining before it stops taking damage from other bots. It is represented as a percentage (example: 0.3 == 30% health remaining).  

Variable name: disableInNetGame  
- Possible values: (true or false)  
- Default = false  
Disables the AI Hero Support aspect if it is a multiplayer match. When disabled this way, the AIHeroSupport script never starts.  

-----------------------------------------------------------------------------------------------------

### Global Functions (can be used at any time):

Function name -> AIHeroSupport:New{}  
Returns the hero support script object reference.  


### Object Functions (functions that can be used anytime AFTER New{} is called):

Function name -> objectName:AddSpawnCP(cpName,pathName)  
Adds the CP and associated spawn path data to the script's list.  

Function name -> objectName:Start()  
Starts the hero script. Only use this once.  

Function name -> objectName:EnableHeroes(boolean)  
Use true or false. Enables or disables the AI hero spawn for both teams.  

Function name -> objectName:EnableHeroTeam(teamPtr,inVal)  
Enables or disables the AI hero spawn for the specified team. Use true or false for inVal.  


### Special case Object Functions (functions that can be used AFTER New{} is called):

Function name -> objectName:SetHeroClass(teamPtr,className)  
Sets the hero class className for teamPtr and records necessary information needed to run the script.  
NOTE: This has to be used BEFORE the "Start()" function call. It will not work correctly if used afterwards.  

-----------------------------------------------------------------------------------------------------

### Known glitches:  
- The AI heroes will still spawn even when heroes are disabled in the game options. The human players will not be able to play as the heroes, but the hero unit will still be on the map as a bot. I'm not sure if this can be fixed... (I'm also not sure if I want this "fixed")  



### Known bugs (things like crashing, stuff that can cause problems with gameplay, etc):  
- None that I know of.  

-----------------------------------------------------------------------------------------------------

***Legal Stuff:
You are welcome to use this script in your custom made mods and maps so long as they are not being rented or sold. If you use this script, please credit me in the readme of the project you used it in. Do not claim this script as your own (it may not be much, but I did spend some time writing it after all). Do not edit this script. If you need to override any of the variables, please do so from your own script. I am not responsible for any damages that might be incurred through the use of this script.

THIS SCRIPT IS NOT MADE, DISTRIBUTED, OR SUPPORTED BY LUCASARTS, A DIVISION OF LUCASFILM ENTERTAINMENT COMPANY LTD.

-----------------------------------------------------------------------------------------------------




-----------------------------------------------------------------------------------------------------

## STUFF FOR ADVANCED SCRIPT-WRITERS ONLY

### VARIBALE LIST
***NOTE: It is very unlikely that you will need to change the following variables (so don't unless you know what you're doing)...

Variable name: cycleTime  
- Possible values:  (a number in seconds)  
- Default = 10  
How often the script looks for heroes on the map. When the cycleTime has passed, the script checks the units for hero status. If the script happened to not catch a human changing to a hero class (thus resulting in the AI hero never being removed), the AI hero will also be removed at this time. This check does a lot of "loop" work (may slow down a map if done too often). NOTE: This is just here as an extra option for flexability, I would suggest you leave this variable alone.  

Variable name: netCycleTime  
- Possible values: (a number in seconds)  
- Default = 20  
Same as the cycleTime but is used in multiplayer games. NOTE: Also just like cycleTime, this is just here as an extra option for flexability, I would suggest you leave this variable alone.  

Variable name: enforceSingleHero  
- Possible values: (true of false)  
- Default = false  
Considering the way I setup the script, this option is not really needed, but I decided to have it available "just in case". When enabled, this option makes the cycle test more comprehensive when looking for heroes on the map. Usually it will remove all bot controlled hero units except for one. If a hero is human controlled, it will remove all bot controlled heroes from the map. This check is executed as part of the "cycle test". The ONLY time this should be enabled is if you see more than one of the AI units using the hero at once, and on more than one occasion. This test does a lot of "loop" work so it might be a problem on slower computers.  

### FUNCTION LIST

Function name -> objectName:SetBotCount(number)  
Overrides and sets the bot count to be used. This setting is applied to all teams (that's the way it works in the game).   
!!! Note: The Start() function sets the bot count itself, so using this function before the script is started won't do anything. Also remember that the conquest, ctf, tdm, etc game scripts set the bot count themselves, so they too will override this function if it is used before them.  


-----------------------------------------------------------------------------------------------------
