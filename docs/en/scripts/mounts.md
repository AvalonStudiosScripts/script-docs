# avalon_mounts

Script for **rideable creatures** in FiveM. Players can summon, ride, and manage fantastical creatures (dragons, griffins, pegasi, horses, alpacas) via commands or inventory items.

**Version:** 1.0 | **Author:** Mino

---

## Dependencies

| Resource | Required | Notes |
|----------|:--------:|-------|
| ox_lib | ✅ | Notifications and utilities |
| ox_inventory | ❌ | Only if `CONFIG.items = true` |
| ox_fuel | ❌ | Auto-maintains fuel at max |
| qbx_core | ❌ | QBox keys integration |
| qbx_vehiclekeys | ❌ | QBox keys integration |

---

## Basic Usage (Commands)

When `CONFIG.command = true`, players can summon and dismiss creatures directly from chat — no items or extra scripts needed.

### Summon a creature

```
/mount <name> <variant>
```

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `string` | Creature type (see table below) |
| `variant` | `number` | Variant number (1, 2, 3...) |

**Examples:**

```
/mount necrodragon 1
/mount griffon 2
/mount pegaso 1
/mount horse 3
/mount alpaca 1
```

### Dismiss the creature

```
/delMount
```

Removes the currently summoned creature from the world.

---

## In-Game Controls

Once a creature is summoned:

| Action | Default Key | Config |
|--------|:-----------:|--------|
| Mount / Dismount | **E** | `CONFIG.keyMount = 51` |
| Dismiss creature | **H** | `CONFIG.keyDelete = 74` |
| Toggle fly mode | **X** | `CONFIG.keyFlymode = 73` |

!!! info "Changing keys"
    Edit the numeric values in `config.lua`. Codes correspond to [FiveM control codes](https://docs.fivem.net/docs/game-references/controls/).

---

## Available Creatures

| Creature | Command | Variants | Can Fly |
|----------|---------|:--------:|:-------:|
| Necrodragon | `/mount necrodragon` | 5 (1–5) | ✅ |
| Griffon | `/mount griffon` | 3 (1–3) | ✅ |
| Pegaso | `/mount pegaso` | 3 (1–3) | ✅ |
| Horse | `/mount horse` | 3 (1–3) | ❌ |
| Alpaca | `/mount alpaca` | 3 (1–3) | ❌ |

---

## Configuration

### General Settings

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `CONFIG.command` | `boolean` | `true` | Enable `/mount` and `/delMount` commands |
| `CONFIG.commandName` | `string` | `"mount"` | Summon command name |
| `CONFIG.delCommand` | `string` | `"delMount"` | Dismiss command name |
| `CONFIG.items` | `boolean` | `true` | Enable item-based spawning (requires ox_inventory) |
| `CONFIG.invincible` | `boolean` | `false` | Make creature invincible |
| `CONFIG.maxrangedespawn` | `number` | `200.0` | Distance before creature auto-despawns |
| `CONFIG.timeForSpawnAfterDeaths` | `number` | `10000` | Wait time (ms) to summon after dying |

### Flight System

```lua
CONFIG.flyWithTime = false   -- false = unlimited flight
CONFIG.flyTimeMax  = 60000   -- ms of flight (only if flyWithTime = true)
CONFIG.flyCooldown = 60000   -- ms cooldown before flying again
```

### Max Speed

Internal multiplier — test and calibrate on your server.

```lua
CONFIG.MaxSpeed = {
    ["necrodragon"] = 100.0,
    ["griffon"]     = 50.0,
    ["pegaso"]      = 70.0,
    ["alpaca"]      = 50.0,
    ["horse"]       = 70.0,
}
```

---

## Available Exports

### `API` — Client-Side

Returns the API object with all public functions.

```lua
local MountsAPI = exports['avalon_mounts']:API()
```

#### Functions

| Function | Description |
|----------|-------------|
| `MountsAPI.spawn(name, color)` | Summon a creature |
| `MountsAPI.despawn()` | Dismiss the active creature |
| `MountsAPI.isSpawned()` | `true` if a creature is summoned |
| `MountsAPI.isOnMount()` | `true` if player is mounted |
| `MountsAPI.getVehicle()` | Returns the creature vehicle entity |
| `MountsAPI.getData(model)` | Config data for a specific creature |
| `MountsAPI.getAllData()` | Data for all creatures |
| `MountsAPI.getBlacklist(vehicle)` | Check if a vehicle is blacklisted |
| `MountsAPI.deleteEnt(ent)` | Delete a specific entity |
| `MountsAPI.playAnimation(prop, anim, dict, loop)` | Play animation on the creature |
| `MountsAPI.syncInvisible(ent, bool)` | Sync visibility across all clients |
| `MountsAPI.syncHair(ent, head)` | Sync head/hair variant |

#### Example

```lua
local MountsAPI = exports['avalon_mounts']:API()

-- Summon griffon variant 2
MountsAPI.spawn("griffon", 2)

-- Check if player is mounted
if MountsAPI.isOnMount() then
    print("Player is riding a creature")
end

-- Get creature position
local mount = MountsAPI.getVehicle()
if mount then
    local coords = GetEntityCoords(mount)
end

-- Dismiss
MountsAPI.despawn()
```

---

### Dynamic Item Exports — Client-Side

!!! info "Requires `CONFIG.items = true`"
    One export per creature variant, auto-created at startup.

Format: **`creatureName_variantNumber`**

```lua
exports['avalon_mounts']:necrodragon_1()   -- variant 1
exports['avalon_mounts']:necrodragon_2()
exports['avalon_mounts']:necrodragon_3()
exports['avalon_mounts']:necrodragon_4()
exports['avalon_mounts']:necrodragon_5()

exports['avalon_mounts']:griffon_1()
exports['avalon_mounts']:griffon_2()
exports['avalon_mounts']:griffon_3()

exports['avalon_mounts']:pegaso_1()
exports['avalon_mounts']:pegaso_2()
exports['avalon_mounts']:pegaso_3()

exports['avalon_mounts']:horse_1()
exports['avalon_mounts']:horse_2()
exports['avalon_mounts']:horse_3()

exports['avalon_mounts']:alpaca_1()
exports['avalon_mounts']:alpaca_2()
exports['avalon_mounts']:alpaca_3()
```
