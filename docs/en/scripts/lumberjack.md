# avalon_lumberjack

Script for the **lumberjack job** in FiveM. Players chop trees using a score-based minigame with axe types, durability system, and skill bonuses.

**Version:** 1.0 | **Author:** Avalon

---

## Dependencies

| Resource | Required |
|----------|:--------:|
| ox_lib | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |
| core_skills | ❌ (optional) |

---

## Installation

```
ensure avalon_lumberjack
```

---

## Configuration

### Language

```lua
Lang = "English"
-- Supported: Italiano, English, Français, Deutsch, Español,
-- Português, Português (Brasil), Nederlands, Polski, Română,
-- Ελληνικά, 中文, ไทย, Filipino, Русский
```

### Score Minigame

```lua
Config.scoreGame  = true  -- enable score system
Config.scoreToWin = 50    -- score needed to win (nil/0 = endless)
```

### Axes

```lua
Config.Axe = {
    ["money"] = {
        propModel       = "prop_tool_fireaxe",
        BonusFireRate   = 50,
        BonusLoot       = 2,
        BonusScore      = 1,
        BonusDurability = 0,
    },
    ["tool_consaw"] = {
        propModel       = "prop_tool_consaw",
        BonusFireRate   = 100,
        BonusLoot       = 3,
        BonusScore      = 2,
        BonusDurability = 3,
    },
}
```

### Durability

```lua
Config.durabilitySystemEnabled = true
Config.MaxDurability            = 200
Config.Durability               = 15  -- lost per chop
```

### Tree Locations

```lua
Config.LumberCoords = {
    vector3(-527.5469, 537.0093, 111.5338),
    -- add more coordinates here
}
```

### Loot

```lua
Config.Loot = {
    { name = "raw_material_wood", label = "Wood",   chance = 20, scoreGame = {min=1, max=5} },
    { name = "rubber",            label = "Rubber", chance = 5,  scoreGame = {min=1, max=5} },
    { name = "fibers",            label = "Fibers", chance = 8,  scoreGame = {min=1, max=5} },
}
```

### Stress

```lua
Config.StressMax   = 1000000
Config.StressValue = 100
```

---

## Available Exports

!!! info "No exports registered"
    `avalon_lumberjack` does not expose any public exports.
