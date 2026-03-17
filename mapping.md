### IN DEVELOPMENT

Extended Generalized Actions
- More speed settings
- More wait time settings (for doors and lifts)
- floor/ceiling toggle for crushers
- more lock types for doors (including powerups)
- teleporters (z-height toggles, shootable)
- lights

linedef flag toggles

### PLANNED

sinusodal texture scrolling, perhaps horizontal width of line for x scroll, vertical width of line for y scroll? also have options for syncronised scrolling vs desyncronised, coordinates of line determines start time for desyncronised

sinusodal floor/ceiling move

change colormap for map, change colormap for tag

change palette? not sure what will happen here, if it can be changed during the map or even between them. hopefully won't need to be scrapped.

change music linedef actions

perhaps add control line actions for floor movement speed?

COUNTASSECRET flag to determine whether picking up an item counts as a secret or killing a monster counts as a secret. for things that can't be picked up or killed, secret count might break, so either say that when they're collided with they trigger a secret or just say it's the mappers fault. i'm leaning towards the latter for cheaper collision checks.

TURRET flag to prevent monsters from resetting their movecount when bumping into walls, allowing for turret enemies to attack regularly even if on a tight platform?
