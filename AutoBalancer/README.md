# AutoBalancer: Real-time damage modifier

Last updated: 17-05-2019

**Workshop code: 67CK9**

---

## Table of contents

* [Description](#description)
* [Mechanics](#mechanics)
* [Documentation](#documentation)
  * [Rules](#rules)
  * [Variables](#variables)
* [Known bugs](#bugs)
* [Updates](#updates)

## Description

Real-Time damage modifier that decreases your damage output after a kill by a certain value depending on your health. When killed, you get a damage output boost depending on your killer’s health.
Your killer will be marked with a skull until you kill him yourself (you are now a “hunter” to that player).
The only way to get a damage boost yourself is by killing the skulled player for revenge!

Damage output is stated in the top center. Your damage penalty is in the top-left with a count of your hunters.

## Mechanics

* Whenever someone gets a kill, their damage output is reduced by their health percentage ([0:1]) multiplied by a constant value (default is 4).
* The victim gets this same calculated value as a bonus to their damage output.
* The victim will see a red skull icon over the head of their latest killer. It will only disappear after the victim kills this player ("revenge kill").
* Every kill will result in a penalty to the killer's damage output, except for "revenge kills": revenge kills result in a bonus of the same value that would normally be assigned as penalty value.
* The red skull will also be temporarily invisible to "hunters" when a Sombra goes invisible while having a skull above her. All hunters that have her as target, will be notified that she has gone invisible/becomes visible again.
* A player can have multiple hunters, meaning that multiple people can have an icon visible above this player if this player is their latest killer.
* Death by another player than the current lastest killer results in the removal of the icon over that player and creates a new icon over the new killer.

## Documentation

### Rules

1. GLOBAL RULE INIT: simply initializes the Global Variable A which holds the damage modifier with which the health percentage is multiplied.
2. PLAYER ALIVE: adds a small value to a Player Variable E to keep track how long someone is alive. I use this to make sure people who joined mid-game also get the HUD-elements.
3. ROUND RESET ALIVE: resets the Player Variable E at each round to update everything if necessary.
4. ROUND SETUP 2: sets all player variables to starting conditions, destroys all old HUD elements and creates all new HUD elements. This was initially done at the start of a match, but for players joining mid-game, a different method had to be devised which ran throughout the game. Also sets the initial damage dealt by players.
5. TARGET SEEKING BASE: when a player deals final blow to a player and the victim already has the attacker as his latest killer, simply add the damage bonus to the victim (damage penalty to attacker is done later on in different rule).
6. TARGET SEEKING NEW: when a player deals final blow to a player and the victim didn’t have this player as latest killer, set as latest killer and set damage bonus for victim. Also adds a red skull icon (Player Variable Z) to the attacker only seen by victim and notifies the attacker of a new “hunter”. Also adds Victim to array of victims (T) of Attacker.
7. REMOVE DAMAGE MODIFIER/KILLED TARGET: if the player deals final blow to its target, add a damage bonus to attacker and destroy the icon for this target. Also removes Attacker from array of victims (T) of Victim (no longer a hunter of Victim).
8. SAME VICTIM DAMAGE MODIFICATION: give the attacker a damage penalty on final blow dealt if the victim is not the target.
9. SOMBRA SET HUD: sets up Sombra’s specific HUD element which shows whether or not she’s hidden from her hunters.
10. SOMBRA SWITCH REMOVE HUD: removes Sombra’s specific HUD element when changing Heroes.
11. SOMBRA DESTROY ICON IN CAMO: destroys all icons on her. Also notifies all her hunters that she has gone invisible. Gets a message herself that she is hidden from her hunters.
12. SOMBRA CREATE ICONS IN CAMO: recreates all destroyed icons for her hunters when becoming visible again. Also notifies her hunters that she is visible again.

### Variables

The following table holds all variables used, starting with Global variables and then the Player variables.

| Variable | VarType | Type           | Value                | Description                                                                               |
|----------|---------|----------------|----------------------|-------------------------------------------------------------------------------------------|
| A        | Global  | Number         | 4                    | Damage Modification Factor                                                                |
| B        | Global  | Hero           | Current hero of slot | Stores the current hero of each player                                                    |
| C        | Global  | Boolean        | FALSE                | Whether or not console is allowed                                                         |
|          |         |                |                      |                                                                                           |
| A        | Player  | Player         | -                    | Killer of Event Player                                                                    |
| B        | Player  | Number         | -                    | Damage Output (%)                                                                         |
| C        | Player  | Number         | 0                    | Amount of hunters                                                                         |
| D        | Player  | Number         | 0                    | Match round+1                                                                             |
| E        | Player  | Number         | 0                    | Lifetime                                                                                  |
| F        | Player  | Number         | 0                    | Match round+1                                                                             |
| G        | Player  | Number         | 0                    | Time crouched (CONSOLE MODE)                                                              |
|          |         |                |                      |                                                                                           |
| S        | Player  | Element        | -                    | HUD ID Sombra                                                                             |
| T        | Player  | Array Elements | -                    | Player IDs victims                                                                        |
| U        | Player  | Number         | 0                    | Counter for amount of icons deleted as invisible Sombra                                   |
| V        | Player  | Number         | 0                    | Amount of icons on Event Player (index of array T of element of latest victim's icon - 1) |
| W        | Player  | Element        | -                    | HUD ID Damage penalty calculation and Health                                              |
| X        | Player  | Element        | -                    | HUD ID Damage Output                                                                      |
| Y        | Player  | Element        | -                    | HUD ID Hunters                                                                            |
| Z        | Player  | Element        | -                    | Icon ID Target                                                                            |

## Known bugs:

* Sombra’s icon switch does not work perfectly (messages mess up).
* Killing your target does not give a bonus, but merely prevents the penalty (value should be increased).

## Updates:

1. Sombra's skull is invisibile when Sombra cloaks.
2. Added in-game changing of global var A for player slot 0 (if global var C == TRUE).
