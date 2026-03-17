# avalon_electrician

Script for the **electrician job** in FiveM. Players repair electrical boxes through a minigame system with three difficulty levels.

**Version:** 1.0 | **Author:** Mino

---

## Dependencies

| Resource | Required |
|----------|:--------:|
| ox_lib | ✅ |
| ESX (es_extended) | ✅ |
| framework:PlayMinigame | ✅ |

---

## Installation

```
ensure avalon_electrician
```

---

## Configuration

### Difficulty Levels

| Level | Name | Spots | Payment | Fail Penalty |
|-------|------|:-----:|---------|:------------:|
| `1` | Simple (Apprentice) | 33 | 100–150 tickets | 10 tickets |
| `2` | Medium (Standard) | 25 | 200–300 tickets | 25 tickets |
| `3` | Hard (Pro) | 15 | 400–600 tickets | 50 tickets |

### Tools

```lua
CONFIG.toolItem    = 'toolbox'
CONFIG.toolItemPro = { name = 'toolbox_pro', multiply = 1.3 }  -- 30% time reduction
```

### Payment

```lua
CONFIG.PaymentItem = "ticket"  -- reward item

CONFIG.PAYMENT = {
    [1] = { min = 100, max = 150 },
    [2] = { min = 200, max = 300 },
    [3] = { min = 400, max = 600 },
}

CONFIG.PENALTY = {
    [1] = 10,
    [2] = 25,
    [3] = 50,
}
```

### Stress

```lua
CONFIG.StressMax   = 10000
CONFIG.StressValue = 100
```

---

## Available Exports

!!! info "No exports registered"
    `avalon_electrician` does not expose any public exports.
