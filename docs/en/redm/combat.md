# avalon_combat

**Melee combat** system for RedM. Every style (werewolf, vampire, katana, sword and shield, pugilist, mages...) uses the animations from the Avalon packs. Hits are calculated by the script: every frame it follows hands, feet and blades along the animation and hits the peds they cross. Damage always goes through the server.

It includes: light and heavy combos, paired attacks (two peds animated together), parry and counterattack, wielded weapons and shields, sprint, dodge, leap and flight for creatures, NPCs that fight on their own, and exports to connect levels, abilities, health bars and spells.

**Version:** beta | **Author:** Avalon | **Game:** RedM | **Framework:** standalone

---

## Dependencies

| Resource | Required | What it is for |
|----------|:---:|---|
| OneSync | ✅ | NPC fighters and parry (state bags) |
| `avalon_weapons_rdr2` | ✅ | The wielded weapon models |
| `avalon_shields_tint` | ✅ | The shield models |
| Animation packs `avalon_anims_*` | ✅ at least one | The animations and styles (see [Styles](#styles)) |

No framework required: the script works on peds and players. Link it to inventories, levels or health bars through the [exports](#available-exports).

---

## Installation

1. Copy the `avalon_combat/` folder into `resources/`
2. Start the weapons, the shields and the animation packs, then the resource:

```
ensure avalon_weapons_rdr2
ensure avalon_shields_tint
ensure avalon_anims_werewolf
ensure avalon_anims_vampire
ensure avalon_anims_ka_02
# ... the other packs you use
ensure avalon_combat
```

!!! info "Pack order"
    A pack started after `avalon_combat` (or restarted) is still picked up: its styles are added as soon as it starts.

---

## Styles

Styles come from the **animation packs**. Each pack ships an `avalon_combat.lua` file with its own styles and declares it in its `fxmanifest.lua`. `avalon_combat` collects them on its own through the pack's `CombatStyles` export.

| Style | Pack | Weapon | Guard | Extra |
|-------|------|--------|-------|-------|
| `werewolf` | `avalon_anims_werewolf` | claws | game's (bare hands) | sprint, dodge, leap, paired attacks |
| `vampire` | `avalon_anims_vampire` | claws | own | flight, boost, paired attacks |
| `succube` | `avalon_anims_succube` | claws | game's (bare hands) | flight, boost, paired attacks |
| `katana` `katana1` `katana3` `katana4` | `avalon_anims_ka_02` `_01` `_03` `_04` | `xur_katana` | game's (blade) | |
| `sword` `sword2` `sword3` | `avalon_anims_sw_01` `_02` `_03` | `xur_machete2` | game's (blade) | |
| `sword_shield` `sword_shield2` `sword_shield3` | `avalon_anims_sns_01` `_02` `_03` | `swn2craft` + shield | own / own / game's | |
| `twinsword` | `avalon_anims_twinsword01` | `xur_katana2` in both hands | game's (blade) | |
| `hammer2h` | `avalon_anims_hammer2h` | `hammermoore` | game's (blade) | |
| `orientalmonk` | `avalon_anims_orientalmonk` | `giga` | game's (blade) | |
| `pugilist` | `avalon_anims_p_01` | bare hands | game's (bare hands) | |
| `firemage` `mage` `witch` `wizard` | `avalon_anims_firemage_01` `_mage_01` `_witch_01` `_wizard` | hands / staff | game's (bare hands) | |

!!! warning "Spells"
    For now the mage styles hit with their hands, with a longer reach (as the spell would). Staffs are visual only: the point spells will come from is read with the client export `GetCastPoint()`.

To write a style of your own see [Creating a Style](#creating-a-style).

---

## Commands and Keys

### Combat Mode

| Command | Description |
|---------|-------------|
| `/avalon_combat <style>` | Enter combat with that style (with no argument it lists the styles) |
| `/avalon_combat off` | Leave combat |

| Key | Action |
|-----|--------|
| **Left click** | Light combo (press again during the attack for the next hit) |
| **Right click** | Heavy combo |
| **R** | Paired attack on the ped in front (for styles that have them) or special combo |
| **CTRL** (held) | Guard |
| **E** | Lock on to the ped you aim at with the camera (red diamond above the head): attacks and paired attacks go to them. Press E again to release |
| **Shift** | Sprint (werewolf) · flight (flying styles, in the air) |
| **Alt** | Dodge (werewolf) · boost (flying styles) |
| **Shift + Space** | Leap (werewolf) · takeoff and landing (flying styles) |

!!! info "Who reacts to hits"
    Light and heavy hits only deal damage. The special combo, paired attacks and attacks with `react = true` also make the victim stagger or fall.

### Test and Tuning Commands

| Command | Description |
|---------|-------------|
| `/avalon_attack <id>` | Perform an attack of the current style (id or clip name) |
| `/avalon_pair [name]` | Perform a paired attack on the ped in front |
| `/avalon_pair_offset <name> <x> <y> <yaw>` | Move the victim of a paired attack (this client only) |
| `/avalon_weapon <name\|off>` | Change the weapon in the right hand |
| `/avalon_offhand <name\|off>` | Change the weapon in the left hand |
| `/avalon_shield <name\|off>` | Change the shield |
| `/avalon_weapon_offset <x y z rx ry rz> [bone]` | Adjust the weapon position and print the line to copy into the config (also `_offhand_offset`, `_shield_offset`) |
| `/avalon_sound <name>` | Play a sound from the config |
| `/avalon_sound_game <set> <name> [bank]` | Try any sound from the game |
| `/avalon_npc <style> [team] [model]` | Create an NPC fighter 4 m in front of you |
| `/avalon_npc_clear` | Delete the NPCs you created |
| `/avalon_npc_info` | State of the NPC fighters within 30 m |
| `/avalon_dummy [model]` · `/avalon_dummy_clear` | Stationary dummies to hit |
| `/avalon_hitbox` | Show the hitboxes: bodies (green), limbs (yellow), blades (light blue), hits (red) |
| `/avalon_combat_debug` | Draw the hand movements and print every hit |

---

## How Damage Works

1. The attacker's client checks limbs and blades every frame along the animation. A limb slower than `MinSweepSpeed` does not hit (wind-up and return of the strike).
2. When a limb or a blade touches a ped's hitbox, the client reports it to the server.
3. The server validates the hit (distance, maximum damage, attack unlocked), calculates damage and guard, and runs the *hit handlers* of other resources.
4. Depending on `AvalonCombat.DamageMode`:
    - `'native'` — the damage is applied to the ped's real health
    - `'external'` — no damage: the server emits the `avalon_combat:hit` event for your own system (fake health bars, resistances)

    The victim still performs the reaction.

**Formula:** base damage (the weapon's if the blade hits, otherwise the style's `damage`) × the attack's `mult` × progression multiplier × outgoing multiplier, rounded. Then the guard's defence is subtracted.

---

## Configuration

The configuration files are in `config/`:

| File | Contents |
|------|----------|
| `config/config.lua` | General settings, combos, NPCs, sounds, damage, defence, input |
| `config/guard.lua` | The game's guards usable by name (`'blade'`, `'unarmed'`) |
| `config/weapons.lua` | Right-hand weapons |
| `config/offhand.lua` | Left-hand weapons |
| `config/shields.lua` | Shields (one entry per color) |
| `config/styles/` | Optional local styles and `example.lua` with every field explained |

### General Settings

| Option | Default | Description |
|--------|---------|-------------|
| `TargetRange` | `4.0` | m: peds farther away are not checked |
| `LockRange` / `LockCone` | `3.0` / `45` | An attack turns toward the nearest ped within these meters and degrees |
| `LockOnRange` | `12.0` | m: lock-on radius with E |
| `MinSweepSpeed` | `2.0` | m/s: a slower limb does not hit |
| `SkipStart` | `0.10` | Initial part of every attack that is ignored |
| `Rehit` | `0.45` | s before the same attack can hit the same target again |
| `MaxHitDamage` | `200` | The server rejects hits above this damage |
| `MaxHitDistance` | `6.0` | The server rejects hits on players farther away than this |
| `PairRange` | `3.5` | m: maximum grab distance of a paired attack |
| `HitboxScale` / `HitboxPad` | `1.0` / `0.0` | Enlarges blade hitboxes (to hit more easily) |
| `SoundVolume` | `0.35` | Volume of the audio files |
| `DamageMode` | `'native'` | `'native'` or `'external'` (see above) |

### Combos

```lua
AvalonCombat.ComboWindow = 0.45 -- from this phase of the attack, a press queues the next hit
AvalonCombat.ComboCancel = 1.0  -- phase at which the queued hit starts (1.0 = at the end of the animation)
AvalonCombat.ComboReset = 0.8   -- s: after a pause longer than this the combo restarts from the first hit
```

### Input Mode

```lua
AvalonCombat.InputMode = 'combo' -- 'combo' | 'keys'
AvalonCombat.Keys = { SimulWindow = 0.08, SequenceWindow = 0.6, DirAbove = 0.5 }
```

- `'combo'`: each key performs the next hit of its chain (`light`, `heavy`, `special`)
- `'keys'`: like in a fighting game, keys perform the style's `moves`. You can combine keys pressed together (`'light+heavy'`), a direction (`'F+heavy'`: forward + heavy) or sequences (`{ 'light', 'light', 'heavy' }`)

Each player can have a different one with the `SetInputMode` export.

### Guard and Parry

```lua
AvalonCombat.Defence = {
    Mode = 'always',      -- 'always' | 'timed' | 'stamina' | 'points'
    ParryWindow = 0.25,   -- s: guard raised within this time of the hit = parry
    StaminaCost = 15.0,   -- 'stamina': stamina removed per parried hit
    Points = 100, PointsRegen = 10, PointsCost = 20, -- 'points': guard points
    BreakTime = 1.5,      -- s without guard after it breaks
    Block = { percent = 0.75, flat = 0 }, -- base defence of the guard
}
```

| `Mode` | When the guard stops a hit |
|--------|----------------------------|
| `always` | Always, damage reduced by the defence |
| `timed` | Only within `ParryWindow` of raising it, otherwise the hit lands in full |
| `stamina` | Always, every hit costs stamina; when stamina runs out the guard breaks |
| `points` | Always, every hit costs guard points (`GetGuard` / `SetGuard` exports); at zero it breaks |

A **parry** cancels the damage and makes the attacker stagger. **Defence** = `damage × (1 - percent) - flat`, never below zero. A wielded shield replaces the guard's defence with its own; the one given with the `SetDefence` export is added on top.

### Sounds

Every sound has a name in `AvalonCombat.Sounds`. Styles, attacks, weapons and paired attacks use those names: a single name, a list to pick one from at random, or `{ light = ..., heavy = ..., special = ... }` per chain.

```lua
AvalonCombat.Sounds = {
    punch = { file = melee('punch', 11) },                 -- file in sounds/ (.wav .ogg .mp3)
    my_roar = { file = 'roar.ogg', volume = 0.8, range = 40 },
    beast_roar = { set = 'LSHNT_sounds', name = 'ROAR' },  -- game sound
}
```

The included melee sounds are the game's own, extracted into `sounds/melee_*.wav`.

!!! tip "Sound at the right moment"
    `soundAt` starts the sound at a precise phase of the animation: `0.35` = at 35% of the clip. It works for attacks, for paired attacks and for the style's movements (`soundAt = { leap = 0.27, dodge = 0.15 }`). Without `soundAt` the sound starts at the first fast movement (attacks) or immediately.

### Body Hitboxes

`AvalonCombat.BodyHitbox` lists the capsules every ped is hit on (`{ bone from, bone to, radius }`): head, chest, abdomen, arms and legs. An animal without those bones becomes a 0.4 m sphere. `/avalon_hitbox` shows them in game.

---

## Weapons and Shields

Each entry in `config/weapons.lua`, `config/offhand.lua` and `config/shields.lua` is a wielded model:

```lua
AvalonCombat.Weapons.xur_katana = {
    model = 'weapon_xur_katana',
    bone = 'SKEL_R_Hand',                         -- bone it is attached to
    offset = vector3(0.075, 0.00, -0.03),         -- m
    rot = vector3(-90.0, 0.0, 0.0),               -- degrees
    damage = 25,                                  -- per blade hit, × the attack's mult
    sounds = { swing = 'blade_swipe', hit = { 'machete_slash', 'blade_flesh' } },
    blade = { from = vector3(0, 0, 0.12), to = vector3(0, 0, 1.05), radius = 0.08 }, -- blade hitbox
}
```

| Field | Description |
|-------|-------------|
| `model` | Model archetype |
| `tint` | Optional: color index (shields have one entry per color) |
| `bone`, `offset`, `rot` | Where it sits in the hand. Adjust it in game with `/avalon_weapon_offset`, which prints the line to copy |
| `damage` | Damage per blade hit |
| `sounds` | The blade's `swing` and `hit` |
| `blade` | Blade capsule in model space: it hits in place of the hand |
| `cast` | Staffs only: the point spells will come from |
| `defence` | Shields only: `{ percent, flat }` in place of the guard's |

---

## Creating a Style

A style can be written in two places:

=== "In the animation pack"

    The way the Avalon packs do it. Next to the pack's `fxmanifest.lua` put `avalon_combat.lua`:

    ```lua
    local Styles = {}

    Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
        block = 'blade',   -- a pack names the game's guard as a string
    }

    exports('CombatStyles', function() return Styles end)
    ```

    and in the pack's `fxmanifest.lua`:

    ```lua
    avalon_combat_styles 'yes'
    shared_script 'avalon_combat.lua'
    ```

    An Avalon Explorer build writes these two lines on its own if the file is there.

=== "In config/styles"

    A `config/styles/mystyle.lua` file inside `avalon_combat`:

    ```lua
    AvalonCombat.Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
    }
    ```

    A style written here **wins** over a pack's style with the same name: to modify a pack's style, copy it here.

`config/styles/example.lua` contains a complete style, commented field by field.

### Style Fields

| Field | Description |
|-------|-------------|
| `dict` | Animation dictionary (`.ycd`) |
| `damage` | Damage of a body hit (punches, kicks, claws) |
| `idle` | Looping animation when standing still |
| `weapon` / `offhand` / `shield` | What it wields (keys of `Weapons`, `Offhand`, `Shields`) |
| `hands` | The limbs that hit: `{ bone from, bone to, reach }` |
| `reach` / `radius` | Reach beyond the last bone and thickness of the limbs (m) |
| `light` / `heavy` / `special` | The combo chains: lists of attacks |
| `moves` | The moves of the `'keys'` mode |
| `sounds` | `swing`, `hit`, `knockdown`, `block`, `leap`, `dodge`, `boost`, `float` |
| `soundAt` | Phase at which the movement sounds start |
| `block` | Guard: `{ start, loop, stop, hit }` with its own clips, or `'blade'` / `'unarmed'`. Without `block` the style cannot parry |
| `stagger` / `knockdown` | Hit reaction clips |
| `move` | Special movement: `kind = 'runner'` (sprint, dodge, leap) or `kind = 'flyer'` (flight, boost) |
| `pairs` | Paired attacks |

### An Attack

```lua
light = {
    { clip = 'attack@slash1', mult = 1.0 },
    { clip = 'attack@slash2', mult = 1.25, knockdown = true, soundAt = 0.35 },
},
```

| Field | Description |
|-------|-------------|
| `clip` | The animation |
| `mult` | Damage multiplier |
| `knockdown` | The victim falls |
| `react` | The victim reacts even if it is not a special attack |
| `sound` / `soundAt` | The attack's own sound and the phase it starts at |
| `keepTurn` | The ped stays turned the way the animation leaves it |
| `locked` | Locked until progression unlocks it (`SetAttackEnabled`) |
| `id` | Attack name for the exports (default: the clip) |

### Paired Attack

```lua
pairs = {
    bite = { att = 'bite@att', vic = 'bite@vic', x = 0.5, y = 2.8, yaw = 196,
             mult = 4.0, damageAt = 0.45, sound = 'beast_roar', soundAt = 0.2 },
},
```

`att` / `vic` are the clips of the attacker and of the victim. `x`, `y` and `yaw` say where the victim stands relative to the attacker (m, degrees). `damageAt` is the phase at which the damage lands. Adjust it in game with `/avalon_pair_offset`.

!!! info "Animation movement"
    Clips that move the ped follow their *mover*, read from `data/clip_movers.lua`. The clips of a new pack need their movers, generated with Avalon Explorer; without them the clip stays in place.

---

## NPC Fighters

An NPC fighter is a networked ped that fights on its own with a style: it looks for a target, approaches, attacks in combos, parries, dodges, leaps or flies.

- **Team:** players are `'player'`. An NPC attacks players (unless its team is `'player'`) and NPCs of other teams, never ordinary peds.
- **Skill:** `AvalonCombat.Npc` in `config/config.lua` holds the default values (aggression radius, attack distance, parry chance, dodge, paired attack...). Each NPC can change some of them with `opts.skill`.

```lua
-- server
local net = exports.avalon_combat:SpawnFighter('werewolf', vector3(-300.0, 790.0, 118.0), 90.0, {
    team = 'bandits',
    health = 400,
    skill = { BlockChance = 0.6, Weights = { heavy = 0.6 } },
})

-- later
exports.avalon_combat:StopFighter(net)
```

| `opts` | Description |
|--------|-------------|
| `model` | Ped model (default `AvalonCombat.Npc.Model`) |
| `weapon` / `offhand` / `shield` | What it wields (default: the style's; `shield = false` = none) |
| `team` | Team (default `'npc'`) |
| `health` | Health |
| `skill` | Only the `AvalonCombat.Npc` values that change |

!!! warning "A player is required"
    `SpawnFighter` creates the ped on the client of the player closest to the coordinates: with an empty server it returns `nil`.

---

## Available Exports

A `target` is a player's server ID or `{ net = netId }` for an NPC fighter.

### Server-Side Exports

| Export | Description |
|--------|-------------|
| `SetStyle(src, style \| nil)` | Put the player in combat with that style, or take them out |
| `GetStyle(src)` | The player's current style |
| `SetHeld(src, slot, key \| nil)` | Weapon (`'weapon'`, `'offhand'`) or shield (`'shield'`) in hand |
| `GetHeld(src)` | `{ slot = key }` |
| `SpawnFighter(style, coords, heading, opts)` | Create an NPC fighter, returns the net id |
| `StopFighter(net)` | Remove an NPC fighter |
| `AddHitHandler(fn)` / `RemoveHitHandler(id)` | Function called on every hit (see below) |
| `GetDamageMode()` | `'native'` or `'external'` |
| `React(target, what)` | Make the target perform `'stagger'`, `'knockdown'` or `'death'` |
| `SetAttackEnabled(target, style, attackId, on)` / `IsAttackEnabled(...)` | Unlock or lock an attack for that fighter |
| `SetAttackMultiplier(target, style, attackId, m)` / `GetAttackMultiplier(...)` | An attack's multiplier |
| `SetOutgoingMultiplier(target, m)` / `GetOutgoingMultiplier(target)` | Multiplier on all of its damage |
| `SetDefence(target, { percent, flat })` / `GetDefence(target)` | Own defence, added to the guard's |
| `GetGuard` / `SetGuard` / `GetMaxGuard` / `SetMaxGuard` | Guard points (`'points'` mode) |
| `SetInputMode(target, 'combo' \| 'keys' \| nil)` / `GetInputMode(target)` | The fighter's input mode |
| `GetStyles()` | All loaded styles |
| `GetAttacks(style)` | `{ { id, chain, clip, mult, locked, input } }` |
| `GetWeapon(key)` / `GetShield(key)` | Config entry |

#### The Hit (`hit`)

Hit handlers and the `avalon_combat:hit` event receive this table:

| Field | Description |
|-------|-------------|
| `mode` | `'native'` or `'external'` |
| `attacker` | `{ kind = 'player' \| 'fighter', src, net, style, team }` |
| `victim` | `{ kind = 'player' \| 'fighter' \| 'ped', src, net, style }` |
| `attack` | `{ id, chain, clip, mult, knockdown, react }` (`chain`: `light`, `heavy`, `special`, `moves`, `pair`) |
| `weapon` | `{ slot, key, data }` if a blade hit, otherwise `nil` |
| `base` | The weapon's or style's damage |
| `attackMult`, `progressMult`, `outgoingMult` | The three multipliers |
| `damage` | Damage before the guard |
| `guard` | `{ blocked, parried, broken, percent, flat, cost }` |
| `final` | Damage after the guard (the one applied in `'native'`) |
| `direction` | `{ x, y }` from the attacker to the victim |

Handlers run after the guard is calculated. A handler returns the modified hit, `nil` to leave it as is, or `false` to cancel it. The handlers of a resource that stops are removed automatically.

```lua
-- server: resistances and levels
exports.avalon_combat:AddHitHandler(function(hit)
    if hit.victim.kind == 'player' and HasSilverArmor(hit.victim.src) and hit.attacker.style == 'werewolf' then
        hit.final = hit.final * 0.5 -- rounded by the server after the handlers
        return hit
    end
end)

-- unlock an attack at level 10
AddEventHandler('levels:up', function(src, level)
    if level == 10 then
        exports.avalon_combat:SetAttackEnabled(src, 'katana', 'attack@combo3', true)
    end
end)
```

```lua
-- server: DamageMode = 'external', health handled by your own system
AddEventHandler('avalon_combat:hit', function(hit)
    if hit.victim.kind ~= 'player' then return end
    local hp = MyHealth.Remove(hit.victim.src, hit.final)
    if hp <= 0 then exports.avalon_combat:React(hit.victim.src, 'death') end
end)
```

**Server events** (`AddEventHandler`, they receive the hit): `avalon_combat:hit` (`'external'` mode), `avalon_combat:blocked`, `avalon_combat:parried`, `avalon_combat:guardBroken`.

---

### Client-Side Exports

| Export | Returns |
|--------|---------|
| `GetStyle()` | Current style or `nil` |
| `IsBlocking()` | `boolean` |
| `IsAttacking()` | `boolean` |
| `GetGuard()` | `{ v, max }` guard points (`'points'` mode) |
| `GetStamina()` | The game's stamina, as read by the `'stamina'` mode |
| `SetCombatEnabled(on)` | `false` leaves the controls to the game (menus, cutscenes) until it is `true` again |
| `GetCastPoint()` | `vector3` where the staff's spells start from, or `nil` |
| `GetInputMode()` | `'combo'` or `'keys'` |
| `MakeFighter(ped, style, opts)` / `StopFighter(ped)` | Turn a ped into a fighter, or stop it |

**Local event** `avalon_combat:onHurt` `(ped, info, attacker)`: on every hit taken by a ped handled by this client, for an HUD.

```lua
-- client: health bar and damage numbers
AddEventHandler('avalon_combat:onHurt', function(ped, info, attacker)
    if ped ~= PlayerPedId() then return end
    if info.parried then
        ShowText('PARRY!')
    elseif info.blocked then
        ShowText(('-%d (blocked)'):format(info.damage))
    else
        ShowText(('-%d'):format(info.damage))
    end
end)

-- client: no combat while the menu is open
exports.avalon_combat:SetCombatEnabled(false)
```
