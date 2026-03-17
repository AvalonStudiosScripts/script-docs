# avalon_mining

Script for the **mining job** in FiveM. Same structure as `avalon_lumberjack` but adapted for mines: pickaxes instead of axes, ore instead of wood.

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

## Configuration

### Language

```lua
Lang = "English"
```

### Pickaxes

```lua
Config.pickAxe = {
    ["tool_pickaxe"] = { propModel = "prop_tool_pickaxe", BonusFireRate = 50,  BonusLoot = 2, BonusScore = 1 },
    ["tool_jackham"] = { propModel = "prop_tool_jackham",  BonusFireRate = 100, BonusLoot = 3, BonusScore = 2 },
}
```

### Ore Locations

```lua
Config.MinerCoords = {
    vector3(-1349.4496, 720.0062, 185.9951),
    -- add more coordinates here
}
```

### Loot

```lua
Config.Loot = {
    { name = "rock",   label = "Rock",   chance = 20, scoreGame = {min=1, max=5} },
    { name = "iron",   label = "Iron",   chance = 5,  scoreGame = {min=1, max=5} },
    { name = "copper", label = "Copper", chance = 8,  scoreGame = {min=1, max=5} },
}
```

### Score Steps

```lua
Config.Steps = {
    step1 = { count = 10, reward = 0  },
    step2 = { count = 21, reward = 30 },
    step3 = { count = 31, reward = 25 },
    step4 = { count = 41, reward = 20 },
    step5 = { count = 51, reward = 15 },
    step6 = { count = 61, reward = 10 },
    step7 = { count = 71, reward = 6  },
    step8 = { count = 86, reward = 2  },
}
```

---

## Available Exports

!!! info "No exports registered"
    `avalon_mining` does not expose any public exports.
