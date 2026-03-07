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

  
- **A_WeaponMeleeAttack47(angle,xmo,ymo,zmo,flags)**
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

A_JumpIfTrue(state,type)
onground, moveforward, movebackward, moveleft, moveright,

A_SwapWeaponTo(weapon,backup)

A_GiveWeapon(weapon,swapto)

A_TakeWeapon(weapon,swapto)

A_ConsumeAmmoOfType(amount,type)
note: negative values can be used to give ammo. this is even possible in mbf21 with A_ConsumeAmmo.

A_ConsumeHealth(amount,takearmour)
again, can be negative.

A_ConsumeArmour(amount)
sure whatever. again, negative allowed.

A_CheckAmmoOfType(state,amount,type)

A_WeaponOffset(x,y,relative)
maybe add scaling control? haven't looked into weapon display code too much yet so i dunno how feasible it is.

A_PlaySoundExt(sound,global,looping,volume,pitch,attenuation)

A_WeaponSoundExt(sound,global,looping,volume,pitch)
maybe add option for seperate channel but isn't that already possible via spawning a thing to play the sound globally

A_PlayMusic(music,singleplay)
not using musinfo just get sound file

A_JumpIfMap(state,startmap,endmap)
startmap is the first map to check from, endmap the last to check for people who want to lump maps together instead of lots of little checks

A_RandomJumpFromList(state1, chance1, state2, chance2, state3, chace3...)
i'd have to check how many args are possible to add, either 32 or 64 i think?

A_CounterAdd(counter,number)
A_CounterSubtract(counter,number)
A_CounterMultiply(counter,number) so you want me to mulcify it?
A_CounterDivide(counter,number)
A_CounterModuloAdd(counter,number)
A_CounterBitShiftLeft(counter,number)
A_CounterBitShiftRight(counter,number)
A_CounterAnd(counter,number)
A_CounterOr(counter,number)
A_CounterXOr(counter,number)
A_CounterNot(counter,number)

A_JumpIfCounterEquals(state,counter,number)
A_JumpIfCounterLessThan(state,counter,number)
A_JumpIfCounterGreaterThan(state,counter,number)
