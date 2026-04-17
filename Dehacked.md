# FBF47 Dehacked Specs

## Currently Added

### Thing Flags
  - **SPEEDY** - Performs additional movement checks to prevent noclipping and bypasses the MAXMOVE limit.
  - **NOTICOFFSET** - Disables random tic offsets on spawn and death frames.
  - **IMPACT** - Internal, enemy deals damage upon hitting another enemy (like SKULLFLY)
  - **IMPACTRIP** - Internal, if enemy is using IMPACT, let it pass through enemies like a ripper projectile.
  - **SPRAYREMEMBER** - Internal, used to track when an enemy has been hit by a TracerSpray tracer
  - **SPRAYPIERCED** - Internal, used to track when an enemy has been pierced by a TracerSpray tracer
  - **INFINITEITEM** - Item does not despawn when picked up
  - **ALWAYSPICKUP** - Item can be picked up even if ammo/health/armour is maxed out.
  - **NOMOVECOUNTRESET** - Thing's "movecount" does not reset upon bumping into a wall. "movecount" is the timer that prevents enemies from firing.
  - **TOUCHYDAMAGE** - Thing deals damage upon contact, but does not enter it's death state.

    It is not advised to use internal flags because they may be buggy and will often be toggled on/off by the game itself, regardless of what you do with A_AddFlags / A_RemoveFlags so can not be relied on to stay consistent.

### Weapon Code Pointers

- **A_TracerSpray(maxangle,damagebase,spraycount,particletype,hitscantype,hitscanflags,damagedice,damageoffset,angleoffset,flags,range,damagerecalc,maxpierces)**
  - Parameterized A_BFGSpray.
  - Args:
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
  -  `maxpierces (int)`: Maximum number of enemies that can be pierced through.
 
    **Flags**
  - `FLAG47_SPRAYFROMOWNER (1)`: Fires hitscan in the direction that the thing that shot the projectile is facing, instead of the projectile itself.
  - `FLAG47_SPRAYSMART (2)`: Performs additional checks to avoid narrowly missing targets. Useful for low spraycounts or high maxangles.
  - `FLAG47_SPRAYRANDOM (4)`: Instead of tracers being equally distanced, use the same RNG algorithm as regular bullet attacks for deciding angle.
  - `FLAG47_SPRAYFROMPROJECTILE (8)`: Instead of firing hitscan from the thing that fired the projectile, fire hitscan from the projectile directly.
  - `FLAG47_SPRAYNOREPEAT (16)`: Each enemy can only be hit by a maximum of one hitscan per attack.
  - `FLAG47_SPRAYCANHITOWNER (32)`: Hitscan attack will not ignore the actor that fired the projectile, and will be able to hit them.
  - `FLAG47_SPRAYPASSTHROUGH (64)`: Allows singular hitscan attacks to hit multiple targets, piercing through them.

- **A_PlayerThrust(angle,velocity,pitch,flags,damage)**
  - Launches the player with the following desired velocity.
  - Args:
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `velocity (fixed)`: Velocity, in direction of angle.
  -  `pitch (fixed)`: Pitch (degrees), relative to calling actor's angle
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
  - Replaces the player's current momentum values with the following desired values.
  - Args:
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `xmo (fixed)`: X (forward/back) velocity
  -  `ymo (fixed)`: Y (left/right) velocity
  -  `zmo (fixed)`: Z (up/down) velocity
  -  `flags (int)`: Flags, see below.
  
   **Flags**
  - `FLAG47_ABSOLUTEXYZ (1)`: The code pointer ignore the angle the monster is facing and instead uses the map's coordinates
  - `FLAG47_OVERRIDETARGET (2)`: Instead overrides the target's momentum
 
    **NOTE: The key difference is that MomentumOverride replaces the thing's speed, instead of adding to it, like Thrust does.**

- **A_LightX(light,relative)**
  - Parameterized A_Light0/A_Light1/A_Light2
  - Args:
    - `light (int)`: How high to set the brightness
    - `relative (int)`: Toggle to increase/decrease brightness by amount instead of replacing brightness by amount.

  
- **A_WeaponMeleeAttack47(damagebase,damagedice,damageoffset,zerkfactor,flags)**
  - Further parameterized A_WeaponMeleeAttack.
  - Args:
  -  `damagebase (int)`: Flat damage to add on top of random damage.
  -  `damagedice (int)`: Maximum times to multiply attack damage by.
  -  `damageoffset (int)`: Amount of damage to be multiplied by damagedice.
  -  `zerkfactor (fixed)`: Berserk damage multiplier; if not set, defaults to 1.0
  -  `hitsound (int)`: Id of sound to play on successful hit
  -  `range (fixed)`: Attack range; if not set, defaults to player mobj's melee range property
  -  `hitstate (int)`: State to enter on successful hit
  -  `angleoffset (int)`: Angle (degrees), relative to calling actor's angle
  -  `pitchoffset (fixed)`: Pitch (degrees), relative to calling actor's angle
  -  `flags (int)`: Flags, see below.

   **Flags**
  - `FLAG47_MELEENORANDOMANGLE (1)`: Disables random attack angle.
  - `FLAG47_MELEENOTURN (2)`: Player does not turn to face their target after hitting them.
  - `FLAG47_MELEECHECKONLY (4)`: No attack is performed, instead the game checks if there's any enemies in range, before adding them as a target and jumping to the hitstate.

### Actor Code Pointers
- **A_MonsterThrust(angle,velocity,pitch,flags,damage)**
  - Launches the calling actor with the following desired velocity.
  - Args:
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
  - Replaces the actor's current momentum values with the following desired values.
  - Args:
  -  `angle (fixed)`: Angle (degrees), relative to calling actor's angle
  -  `xmo (fixed)`: X (forward/back) velocity
  -  `ymo (fixed)`: Y (left/right) velocity
  -  `zmo (fixed)`: Z (up/down) velocity
  -  `flags (int)`: Flags, see below.
   **Flags**
  - `FLAG47_ABSOLUTEXYZ (1)`: The code pointer ignore the angle the monster is facing and instead uses the map's coordinates
  - `FLAG47_OVERRIDETARGET (2)`: Instead overrides the target's momentum

- **A_SpawnOnTarget(angle,x_ofs,y_ofs,z_ofs,x_vel,y_vel,z_vel,flags)**
  - Parameterized A_VileTarget. Spawns a thing in front of the actor's target and set's the spawned thing's tracer to be that target. Does not cause actor to face target.
  - Args:
    - `type (uint)`: Type (dehnum) of actor to spawn
    - `angle (fixed)`: Angle (degrees), relative to calling actor's angle
    - `x_ofs (fixed)`: X (forward/back) spawn position offset
    - `y_ofs (fixed)`: Y (left/right) spawn position offset
    - `z_ofs (fixed)`: Z (up/down) spawn position offset
    - `x_vel (fixed)`: X (forward/back) velocity
    - `y_vel (fixed)`: Y (left/right) velocity
    - `z_vel (fixed)`: Z (up/down) velocity
    - `flags (int)`: Flags, see below.

   **Flags**
  - `FLAG47_SPAWNOWNEDBYENEMY (1)`: Sets the spawned things "owner" (target) to be the enemy that the actor is targeting.
  - `FLAG47_SPAWNONSELF (2)`: Instead of spawning the desired thing on the actor's target, it instead spawns directly on the actor itself.
  - `FLAG47_SPAWNNOTARGET (4)`: Sets the spawned thing's target to be blank.
  - `FLAG47_SPAWNNOTRACER (8)`: Sets the spawned thing's tracer to be blank.
  - `FLAG47_SPAWNNOLOCKON (16)`: Causes the spawned thing to not call A_Fire when spawned.

- **A_BlastAttack(angle,xmo,ymo,zmo,flags)**
  - Parameterized A_VileAttack. If target is in line of sight, deal damage to target, launche target upwards and spawn an explosion in front of the target. 
  - Args:
    - `velocity (fixed)`: Z (up/down) velocity
    - `damage (int)`: Damage to deal to target
    - `massoverride (int)`: If non-zero, replaces the actor's mass with this value when calculating momentum to apply.
    - `blastdamage (int)`: Blast damage of explosion
    - `blastradius (int)`: Radius of explosion
    - `sound (int)`: Id of sound effect to play from attack. If 0, does nothing.
    -  `flags (int)`: Flags, see below.

   **Flags**
  - `FLAG47_DONTFACETARGET (1)`: Actor does not call A_FaceTarget.
  - `FLAG47_DONTCHECKLOS (2)`: Actor does not check for line of sight before attacking, instead always attacking successfully.
  - `FLAG47_THINGBLOCKLOS (4)`: Causes line of sight to be blocked if other things are in the way, instead of only being blocked by walls. 
  - `FLAG47_BLASTFROMTARGET (8)`: Sets the explosion to spawn directly on the target, by default the source of the explosion is the actor's tracer (the flame spawned by A_VileTarget, or the thing spawned by A_SpawnOnTarget).

- **A_SpawnMonster(angle,x_ofs,y_ofs,z_ofs,x_vel,y_vel,z_vel,sound,flags)**
  - Parameterized A_PainAttack.
  - Args:
    - `type (uint)`: Type (dehnum) of actor to spawn
    - `angle (fixed)`: Angle (degrees), relative to calling actor's angle
    - `x_ofs (fixed)`: X (forward/back) spawn position offset
    - `y_ofs (fixed)`: Y (left/right) spawn position offset
    - `z_ofs (fixed)`: Z (up/down) spawn position offset
    - `x_vel (fixed)`: X (forward/back) velocity
    - `y_vel (fixed)`: Y (left/right) velocity
    - `z_vel (fixed)`: Z (up/down) velocity
    - `sound (int)`: Id of sound effect to play from spawned monster. Default is spawned monster's attack sound.
    - `flags (int)`: Flags, see below.

   **Flags**
  - `FLAG47_SPAWNNOIMPACT (1)`: Spawned thing will not do impact damage.
  - `FLAG47_SPAWNNOFACETARGET (2)`: Actor will not call A_FaceTarget.
  - `FLAG47_SPAWNTHROUGHWALLS (4)`: Ignores checks to see if a wall is between the actor and the location of the spawned thing, which was default behaviour in the original game, but not Boom and it's derivatives.
  - `FLAG47_SPAWNRIP (8)`: Spawned thing is considered a ripper.
 
- **A_SelfRaise(state,sound)**
  - Resurrects calling actor. If there is an obstacle in the way, it will do nothing.
  - Args:
    - `state (int)`: State to jump to (Default is Actor's "Raise" state)
    - `sound (int)`: Id of sound effect to play.
   
- **A_JumpIfBlocked(state,x_ofs,y_ofs,z_ofs,radius,height)**
  - Checks whether the desired location has enough free space for the actor to move to. If there is, jump to that state, else do nothing.
  - Args:
    - `state (int)`: State to jump to
    - `x_ofs (fixed)`: X (forward/back) position to check relative to calling actor
    - `y_ofs (fixed)`: Y (left/right) position to check relative to calling actor
    - `z_ofs (fixed)`: Z (up/down) position to check relative to calling actor
    - `radius (fixed)`: Radius of area to check, default is calling actor's radius.
    - `height (fixed)`: Height of area to check, default is calling actor's radius.

- **A_Listen()**
  - Alternate A_Look that only checks sound and not sight.


 
### Thing Properties

- **spawnheight** - When thing is spawned into the level, it's z offset will be this many map units off the ground. If it has the SPAWNCEILING flag, it will instead be offset down from the ceiling.
- **friction** - Sets the thing's default friction to this value, can be replaced by Boom's Friction floors.
- **damagemod** - Controls damage calculations
- **damageflat** - Controls damage calculations, by adding this value at the end
- **damagerecalc** - Repeats damage calculations this many times
 
### Counters

Counters are arbitrary values that can be stored as part of a thing's properties. Counters do not do anything on their own, but can be read by codepointers to change values or decide when to jump to another state. 

The modder may define as many counters as they wish.

Counters also have the following settings that can be changed.

- `Counter Min`: The minimum value a counter can be. Default behaviour is to clamp the number so that it can't reach any lower.
- `Counter Max`: The maximum value a counter can be. Default behaviour is to clamp the number so that it can't reach any higher.
- `Counter Flags`:
  - `FLAG47_UNDERFLOW (1)`: When attempting to reach a value below Counter Min, it instead wraps back around to Counter Max and continues to count down from there.
  - `FLAG47_OVERFLOW (2)`: When attempting to reach a value above Counter Max, it instead wraps back around to Counter Min and continues to count up from there.

 
### Miscellaneous
- **Player View Height**: Change the height of the player camera, has no impact on hitbox. Measured in map units, default is 41
- **Step Height**: Change the maximum sector height which the player and other things can traverse upwards. Measured in map units, default is 24
- **Player Speed Multiplier**: Multiplies the player's movement speed by this amount. Fixed Point value, default is 65536 (1.0)
- **Player Friction Multiplier**: Multiplies the amount of friction the player receives by this amount. This value is overridden by Boom's Friction floors. Fixed Point value, default is 59392 (0.90625)
- **Max Damage Screen Timer**: Changes the maximum duration that the red screen effect from taking damage can last for to this amount. Measured in tics, default is 100
- **Max Item Screen Timer**: Changes the maximum duration that the yellow screen effect from picking up items can last for to this amount. Measured in tics, default is infinite (-1)
- **Vertical Hitscan Offset**: Offsets the source of the player's hitscan attacks vertically by this amount, which usually originate from 36 map units from the position the player is standing. Measured in map units, default is 0
- **Horizontal Hitscan Offset**: Offsets the source of the player's hitscan attacks horizontally by this amount. Measured in map units, default is 0
- **Air Control**: Changes how much control the player has to move while midair. Measured in fixed point, default is 0
- **Jump Delay**: Number of tics the player must stand on the ground for before being able to jump again. Default is 7
- **Coyote Time**: How many tics the player is still allowed to jump for after walking off a platform. Default is 0
- **Player Bob Scaling**: Multiplies how far the player's head bobs up and down while moving, seperate from the weapon bobbing animation. Measured in fixed point, default is 65536 (1.0)
- **Maximum Player Bob**: The maximum distance the player's head can bob up and down by. Measured in fixed point map units, default is 16
- **Weapon Bob Horizontal Scaling**: Multiplies how far the player's weapon bobs side to side while moving. Measured in fixed point, default is 65536 (1.0)
- **Weapon Bob Vertical Scaling**: Multiplies how far the player's weapon bobs up and down while moving. Measured in fixed point, default is 65536 (1.0)
- **Maximum Horizontal Weapon Bob**: The maximum distance the player's head can bob sideways by. Measured in pixels, default is infinite
- **Maximum Vertical Weapon Bob**: The maximum distance the player's head can bob up and down by. Measured in pixels, default is infinite
- **Player Bob Speed**: Multiplies how fast the player's head bobs up and down while moving, seperate from the weapon bobbing animation. Measured in fixed point, default is 65536 (1.0)
- **Weapon Bob Speed**: Multiplies how fast the player's weapon bobs while moving. Measured in fixed point, default is 65536 (1.0)
- **Autoaim Range**: How far player's autoaim is still able to work, if an enemy is further than this distance then autoaim will not account for them. Measured in fixed point map units, default is 67108864 (1024.0)
- **Hitscan Range**: Default range for player hitscan attacks to travel, if an enemy is further than this distance then they will not be hit. Measured in fixed point map units, default is 134217728 (2048.0)

## Planned Features

seen referenced Hitscan flags and types? What are those? i think it would be cool to add different types to attacks to give them custom properties, such as modders using Type 1 for fire themed attacks, then adding an ice enemy that takes double damage from said attack type or enemies that are immune to fire damage. also possibility to add custom death states for specific attack types. as for the flags, i think we should have settings for weapons to use to control these damage types like saying it should ignore custom death states or ignore immunities, etc, not fully sure what to do yet but added it as future-proofing so that if you lot come up with ideas on how to use them, i can add them.

hitscangroup, like mbf21's projectile and infighting and splash groups, but for hitscan attacks.

counters, these are just numbers that can be stored in things, you can do math with them and store data and then use A_JumpIfCounter... to jump to a certain state based on that number or maybe set damage values to equal counter values 

masterprojectile flag to be able to harm things of the same projectile group

masterhitscan flag to be able to harm things of the same hitscan group

mastersplash flag to be able to harm things of the same splash group

masterinfighting flag to be able to infight with things of the same infighting group? this would mean for example hell knights getting angry at barons, but barons not retaliating.

A_JumpIfTrue(state,type)

onground, moveforward, movebackward, moveleft, moveright, need more ideas for general stuff that can be on or off

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

A_SetThingAttribute(attribute,value)

what's a thing attribute? stuff like health, damage, mass, radius, height, spawn state, death state, etc. this code pointer will let you change these values on the fly!

also should add a new thing attribute called maxhealth, so that healing code pointers know not to go any higher than that value because currently doom does not give a flying fuck and will let you give zombiemen 24000/20 health

A_CounterSet(counter,number)

A_CounterAdd(counter,number)

A_CounterSubtract(counter,number)

A_CounterMultiply(counter,number)

A_CounterDivide(counter,number)

A_CounterModuloAdd(counter,number)

A_CounterBitShiftLeft(counter,number)

A_CounterBitShiftRight(counter,number)

A_CounterAnd(counter,number)

A_CounterOr(counter,number)

A_CounterXOr(counter,number)

A_CounterNot(counter,number)

A_CounterSetToAttribute(counter, attribute)

A_CounterSetToSpeed(counter,flags)
- USEXY : Use speed from X and Y axis
- USEZ : Use speed from Z axis
- ABSOLUTE : Use absolute values instead of calculating values from player direction
- UNSIGNED : Convert negative values to positive values in calculation
- TARGETSPEED : Use speed from target instead. If called by player, call P_AimLineAttack to find linetarget, if none available, do nothing.
- TRACERSPEED : Use speed from tracer instead.



A_JumpIfCounterEquals(state,counter,number)

A_JumpIfCounterLessThan(state,counter,number)

A_JumpIfCounterGreaterThan(state,counter,number)

i want to add Sin, Cos, Tan stuff for counters but not certain how to handle it?

doom's trigonometric stuff is handled kinda weirdly

let counters add damage dealt, perhaps through hitscan flags?

also counters should be able to get distances and coordinates and momentum values and yadda yadda. as always, leave suggestions!



A_SelfRaise

A_HealRaiseExt(healstate,sound,raisestate,health,flags)
- IGNOREFLAG : Ignores whether thing has +NOTRESURRECTABLE
- IGNOREDURATION : Ignores duration of tic that enemy is on (vanilla can only resurrect if it's -1 duration)
- HEALABSOLUTE : Instead of using "health" as a percent of total health, heal by the specific integer.
- USEMAXHEALTH : Instead of checking for enemy's "Spawn Health" value, check for it's "Max Health" instead

+PARTICLE - thing can't collide with other things but enters death state upon touching walls

+THINGMISSILE - needs a better name but thing enters death state only when colliding with other things, not walls

+COUNTSECRET - thing counts as a secret, if it's an item then it adds to secret count when picked up, if living it adds to secret count when killed. probably will be a UDB flag so it can be added while mapping instead of being dehacked only.

+NOTRESURRECTABLE - can't be resurrected, even if it has a resurrect state.



### Powerups

Damage Scale (like Quake Quads)

Defence Scale (seperate defence from armour, timed)

Speed Scale (changes player movement speed)

Friction Scale (Changes player friction)

Gravity Scale (Changes player gravity)

Defense To Damage Type (Damage Types System, Splash Damage, Projectile / Hitscan / Melee, set to 100% for Immunity (Note to self: if full defence then prevent pain sound/state)

Visibility Distance (How far enemies can see you from, also perhaps add time/random system so they can see you after enough time has passed with you in front of them?)

Attack Chance (Change how likely enemies are to attack you, also add distance scaling options)

Enemy Accuracy (Change how accurate other enemies are)

Player Accuracy (Change how accurate your own attacks are)

Friend Powerup? (Makes enemies peaceful towards you?)

Infinite Ammo (i dont really know about adding full scaling to this, most attacks just use 1 ammo but i should do it for completeness, the question is how to handle the rounding...)

Wall Climbing? (Would probably require reuse of flying code, might be awkward to add for having to disable gravity temporarily... just add check in P_SlideMove segment i guess)

Flying (Already mentioned gravity but one that allows players to move freely would be good, might be freelook/jumping exclusive though due to how it would function)

Size Control (Change hitbox + camera size, will be awkward accounting for unshrinking in small areas)

Health Regeneration (heal by X amount every Y tics)

Time Scaling (Change game speed? perhaps add seperate thing for changing enemy timing because tic rate changing has it's jank, do by changing enemy movement to scale by X, change tic duration to scale by X, would be awkward and annoying i imagine, maybe need scrapped...)

Touchy Powerup (Kill enemies you touch, maybe say "If enemy closer than X, kill/damage by Y. If X = 0, use hitbox instead.)

Time Freeze (Far less jank and easier than Time Scaling, just apply FREEZE to level, maybe make it togglable between Freeze Level vs Freeze Enemies...)

Extra Backpack (like backpack but with finer control over how much extra space and also can stack with other custom backpacks)

Lighting Control (256 and higher will be identical to fullbright)

All powerups will have duration control. set to INT_MAX to be effectively infinite, like Berzerk (2147483647 tics)

Powerups will have Palette control, to set it to use specific palette pages or lists of palette pages (like berzerk using a few red shades), will have seperate duration control for palettes.

All powerup values can be set to negative numbers, to allow for finer control.

Perhaps add powerup-like system for enemies, like for poison debuffs and stuff...

- A_JumpIfPowerup(state,powerup)
- A_GivePowerup(powerup)
- A_TakePowerup(powerup)

There should be flags to ignore powerup effects, like enemies that stay active when level is frozen, enemies that can bypass invulnerability, enemies that can ignore partial invis (so it isn't a powerdown to projectile enemies !!!)



Mapping specs coming sometime soon hopefully, but will require deeper discussion! Nodebuilders currently only design maps to store 16 bit values for line actions, line flags, sector actions and sector flags so we will actually have limitations here. However, we can be much more free in a UDMF-Lite extension that comes after, designed to greatly extend mapping possibilities while simplifying the process greatly from UDMF to make it more manageable.
