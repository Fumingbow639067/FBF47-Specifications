# FBF47 Dehacked Specs

## Currently Added

### Weapon Code Pointers

- **A_TracerSpray(hhihvhijhbvhjjhbhjhvh)**
  -  bbbbbbadsfdf

- **A_PlayerThrust(angle,velocity,pitch,flags,damage)**
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `velocity (fixed)`: Velocity, in direction of angle.
  -  `pitch (fixed)`: Pitch (degrees), relative to cal
  -  `flags (int)`: Flags, see below.
  -  `damage (int)`: Impact Damage, see below.

   **Flags**
  - `FLAG47_THRUSTENEMY (1)`: Instead of thrusting the player, the monster that the player aims at is thrust.
  - `FLAG47_THRUSTNOHEALTH (2)`: Skips checks for whether the thing has health or not.
  - `FLAG47_THRUST3D (4)`: Horizontal momentum decreases the higher/lower the pitch is, leading to total momentum across all directions adding up to set velocity. 
  - `FLAG47_NOVERTICALTHRUST (8)`: Vertical thrust is set to 0, if THRUST3D is set, horizontal momentum is still decreased.
  - `FLAG47_THRUSTIMPACT (16)`: Causes thrust thing to deal damage to anything it collides with, determined by damage arg.
  - `FLAG47_THRUSTRIP (32)`: Causes thrust thing to pass through solid objects like a ripper projectile.
  - `FLAG47_THRUSTNOCOLLISION (64)`: Skips all checks for whether the thing has any collision.
  - `FLAG47_THRUSTFROMENEMY (128)`: If enabled, the target will be thrust in the direction that the target is facing, instead of from the player.
  - `FLAG47_THRUSTPROJECTILE (256)`: Skips all checks for whether the thing is a projectile.

- **A_PlayerMomentumOverride(angle,xmo,ymo,zmo,flags)**
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `xmo (fixed)`: X (forward/back) velocity
  -  `ymo (fixed)`: Y (left/right) velocity
  -  `zmo (fixed)`: Z (up/down) velocity
  -  `flags (int)`: Flags, see below.
   **Flags**
  - `FLAG47_ABSOLUTEXYZ (1)`: The code pointer ignore the angle the monster is facing and instead uses the map's coordinates
  - `FLAG47_OVERRIDETARGET (2)`: Instead overrides the target's momentum


### Actor Code Pointers
- **A_MonsterThrust(angle,velocity,pitch,flags,damage)**
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `velocity (fixed)`: Velocity, in direction of angle.
  -  `pitch (fixed)`: Pitch (degrees), relative to cal
  -  `flags (int)`: Flags, see below.
  -  `damage (int)`: Impact Damage, see below.

   **Flags**
  - `FLAG47_THRUSTTARGET (1)`: Instead of thrusting itself, the thing thrusts it's current target.
  - `FLAG47_THRUSTTRACER (2)`: Instead of thrusting itself, the thing thrusts it's current tracer. Can stack with THRUSTTARGET to thrust both simultaneously.
  - `FLAG47_THRUST3D (4)`: Horizontal momentum decreases the higher/lower the pitch is, leading to total momentum across all directions adding up to set velocity. 
  - `FLAG47_NOVERTICALTHRUST (8)`: Vertical thrust is set to 0, if THRUST3D is set, horizontal momentum is still decreased.
  - `FLAG47_THRUSTIMPACT (16)`: Causes thrust thing to deal damage to anything it collides with, determined by damage arg.
  - `FLAG47_THRUSTRIP (32)`: Causes thrust thing to pass through solid objects like a ripper projectile.
  - `FLAG47_THRUSTNOAIM (64)`: Doesn't call A_FaceTarget and instead just aims in the current direction.
  - `FLAG47_THRUSTFROMENEMY (128)`: If enabled, the thrust target/tracer will be thrust in the direction that the target/tracer is facing, instead of from the calling actor.
- **A_MonsterMomentumOverride(angle,xmo,ymo,zmo,flags)**
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `xmo (fixed)`: X (forward/back) velocity
  -  `ymo (fixed)`: Y (left/right) velocity
  -  `zmo (fixed)`: Z (up/down) velocity
  -  `flags (int)`: Flags, see below.
   **Flags**
  - `FLAG47_ABSOLUTEXYZ (1)`: The code pointer ignore the angle the monster is facing and instead uses the map's coordinates
  - `FLAG47_OVERRIDETARGET (2)`: Instead overrides the target's momentum

 
### Miscellaneous
- **Player View Height**: Change the height of the player camera, has no impact on hitbox
- **Step Height**: Change the maximum sector height which the player and other things can traverse upwards


## Planned Features

masterprojectile flag to be able to harm things of the same projectile group
