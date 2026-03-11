# FBF47 Dehacked Specs

## Currently Added

### Weapon Code Pointers

- **A_TracerSpray(maxangle,damagebase,spraycount,particletype,hitscantype,hitscanflags,damagedice,damageoffset,angleoffset,flags,range,damagerecalc)**
  -  `maxangle (fixed)`: Horizontal spread (degrees, in fixed point). If an input of 90 degrees is provided, the range is 45 degrees left to 45 degrees right.
  -  `damagebase (int)`: Flat damage to add on top of random damage.
  -  `spraycount (int)`: Amount of hitscan tracers to fire.
  -  `particletype (int)`: Thing ID of thing that should be spawned on top of target.
  -  `hitscantype (int)`: Specified type of hitscan attack.
  -  `hitscanflags (int)`: Specified flags for hitscan attack.
  -  `damagedice (int)`: Maximum times to multiply attack damage by.
  -  `damageoffset (int)`: Amount of damage to be multiplied by damagedice.
  -  `angleoffset (int)`: Additional rotation to hitscan attack.
  -  `flags (int)`: Flags, see below.
  -  `range (fixed)`: Maximum distance, in map units, that attack can reach.
  -  `damagerecalc (int)`: Repeats random damage calculations this many times. (kind of stupid but it's what the bfg does, 15 times, so i gotta add it...)
 
    **Flags**
  - `FLAG47_SPRAYFROMOWNER (1)`: Fires hitscan in the direction that the thing that shot the projectile is facing, instead of the projectile itself.
  - `FLAG47_SPRAYSMART (2)`: Performs additional checks to avoid narrowly missing targets. Useful for low spraycounts or high maxangles.
  - `FLAG47_SPRAYRANDOM (4)`: Instead of tracers being equally distanced, use the same RNG algorithm as regular bullet attacks for deciding angle. 

- **A_PlayerThrust(angle,velocity,pitch,flags,damage)**
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `velocity (fixed)`: Velocity, in direction of angle.
  -  `pitch (fixed)`: Pitch (degrees), relative to cal
  -  `flags (int)`: Flags, see below.
  -  `damage (int)`: Impact damage when colliding with things, like lost souls.

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
 


    **NOTE: The key difference is that MomentumOverride replaces the thing's speed, instead of adding to it, like Thrust does.**

  
- **A_WeaponMeleeAttack47(angle,xmo,ymo,zmo,flags)**
  -  `damagebase (int)`: Angle (degrees), relative to calling actor's angle
  -  `damagedice (int)`: X (forward/back) velocity
  -  `damageoffset (fixed)`: Y (left/right) velocity
  -  `zmo (fixed)`: Z (up/down) velocity
  -  `flags (int)`: Flags, see below.

  
   **Flags**
  - `FLAG47_MELEENORANDOMANGLE (1)`: Disables random attack angle.
  - `FLAG47_MELEENOTURN (2)`: Player does not turn to face their target after hitting them.
  - `FLAG47_MELEECHECKONLY (4)`: No attack is performed, instead the game checks if there's any enemies in range, before adding them as a target and jumping to the hitstate.

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

A_PlaySoundExt(sound,global,looping,volume,pitch,nooverride,attenuation)
no override means that if this thing is already playing a sound, it won't start a new one

A_WeaponSoundExt(sound,global,looping,volume,nooverride,pitch)
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

i want to add Sin, Cos, Tan stuff for counters but not certain how to handle it?

doom's trigonometric stuff is handled kinda weirdly

let counters add damage dealt, perhaps through hitscan flags?

A_SelfRaise

+PARTICLE - thing can't collide with other things but enters death state upon touching walls
+THINGMISSILE - needs a better name but thing enters death state only when colliding with other things, not walls
