# avalon_coltivation

Script for **plant cultivation** in FiveM. Allows players to plant, water, fertilize, and harvest various crops on valid terrain.

**Version:** 1.0 | **Author:** Mino | **Database:** ✅ MySQL

---

## Dependencies

| Resource | Required |
|----------|:--------:|
| ox_lib | ✅ |
| oxmysql | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Installation

1. Add the `avalon_coltivation` folder to your `resources/` directory
2. The SQL table is created automatically on start
3. Add to `server.cfg`:

```
ensure avalon_coltivation
```

---

## Configuration

### General Settings

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `CONFIG.MIN_PLANT_DISTANCE` | `number` | `1.5` | Minimum distance between two plants |
| `CONFIG.MIN_STAGE_TIME` | `number` | `300000` | Minimum time (ms) to advance a growth stage |
| `CONFIG.StressMax` | `number` | `1000000` | Maximum stress to be able to plant |
| `CONFIG.StressValue` | `number` | `100` | Stress added each time a plant is placed |

### Placement

```lua
CONFIG.PLACEMENT = {
    rotateStep = 5.0,   -- degrees per rotation step
    maxDistance = 5.0   -- max distance to place a plant
}
```

### Growth Stages

```lua
CONFIG.STAGES = {
    { percent = 0,   name = 'seed'  },
    { percent = 33,  name = 'small' },
    { percent = 66,  name = 'grown' },
    { percent = 100, name = 'ready' }  -- ready to harvest
}
```

### Harvest

```lua
CONFIG.HARVEST = {
    tool          = 'shears', -- item required to harvest
    breakChance   = 0.2,      -- chance to break plant without tool (20%)
    toolBreakChance = 0.05,   -- chance to break using the tool (5%)
    skillBonus    = 0.1       -- success bonus with "coltivazione" skill (+10%)
}
```

### Plants

| Plant | Seed | Time | Yield | Water |
|-------|------|------|-------|:-----:|
| `grain` | `seme_grano` | 30 min | 5x `grano` | ✅ |
| `canapa` | `seme_canapa` | 45 min | 3x `canapa` | ✅ |
| `coca` | `seme_coca` | 60 min | 2x `coca` | ✅ |
| `fungus` | `seme_fungo` | 20 min | 4x `funghi` | ❌ |
| `papavero` | `seme_papavero` | 50 min | 2x `papavero` | ✅ |

### Tools

| Tool | Item | Time Multiplier | Single Use | Stages |
|------|------|:---------------:|:----------:|--------|
| Watering Can | `watering_can` | -25% | ❌ | seed, small |
| Fertilizer | `fertilizer` | -40% | ✅ | small, grown |
| Grow Lamp | `grow_lamp` | -15% | ✅ | seed, small, grown |

---

## Available Exports

!!! info "Type: Client-Side"
    These exports are registered on the **client** side and must be called from client scripts.

Each export starts the planting process for the corresponding seed.

### `seme_grano`

Start placing a wheat seed.

```lua
exports['avalon_coltivation']:seme_grano()
```

### `seme_canapa`

Start placing a hemp seed.

```lua
exports['avalon_coltivation']:seme_canapa()
```

### `seme_coca`

Start placing a coca seed.

```lua
exports['avalon_coltivation']:seme_coca()
```

### `seme_fungo`

Start placing a mushroom seed.

```lua
exports['avalon_coltivation']:seme_fungo()
```

### `seme_papavero`

Start placing a poppy seed.

```lua
exports['avalon_coltivation']:seme_papavero()
```

---

## Usage Example

```lua
-- client/main.lua of your script

-- Plant from a command
RegisterCommand('plant_wheat', function()
    exports['avalon_coltivation']:seme_grano()
end)

-- Plant from an ox_target zone
exports.ox_target:addBoxZone({
    coords = vector3(100.0, 200.0, 30.0),
    size   = vector3(2, 2, 2),
    options = {
        {
            label = 'Plant wheat',
            onSelect = function()
                exports['avalon_coltivation']:seme_grano()
            end
        }
    }
})
```
