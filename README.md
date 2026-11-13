# Stationeers DOOM

A playable first person shooter written entirely in IC10 and running in a vanilla Stationeers save.

- 4 levels with doors, secret doors and an exit
- 2 enemy types, fireballs, pickups and 2 weapons
- map view, music and sound effects

## Playing

Load the save (single player), pull the lever and turn around. The save is large (about 20 MB, about
10 million lines of IC10), so loading takes a while.

### Key mappings

The game reads your suit and gear, so the controls are Stationeers key actions.
Make sure to set them in **Settings > Controls** first for a good experience.

| Stationeers action | Suggested key | In the game |
|---|---|---|
| Suit Pressure Increase | Up arrow | speed up: stop > walk > run (from walking backwards: stop) |
| Suit Pressure Decrease | Down arrow | slow down: run > walk > stop > walk backwards |
| Suit Temperature Decrease | Left arrow | turn left (11.25 degrees per press) |
| Suit Temperature Increase | Right arrow | turn right |
| Jetpack Toggle Stabilizer | K | map on / off |
| Toggle Light (helmet lamp) | L | fire |
| Toggle Hand Power | O or right mouse button | interact: open doors, use the exit, start / continue / next level, back to the menu after dying |

- Movement is a throttle: a press changes the speed step, there is no need to hold the key.
- Several turn presses within one tick add up.
- The second weapon replaces the first once picked up (no weapon switch).
- The map key needs the jetpack in the back slot (the save provides it).
- The menu screen at the start shows the same controls.

Disclaimer 1: This uses a lot of [shenanigans.md](shenanigans.md) and might break with future Stationeers updates.
Disclaimer 2: This was done using AI (quite a lot actually), still it was a lot of work and needed some good knowledge and ideas to make it work and performant enough. 
