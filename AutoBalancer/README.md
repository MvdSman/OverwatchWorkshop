# AutoBalancer: Real-time damage modifier

Last updated: 14-05-2019

---

## Table of contents

* [Description](#description)
* [Mechanics](#mechanics)
* [Variables](#variables)
* [Documentation](#documentation)
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
