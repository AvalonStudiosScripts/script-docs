# avalon_robbery

Script for **shop robberies** in FiveM. Players aim a weapon at the cashier NPC to start the robbery, progressively collect cash, and open the lockbox via a minigame.

**Version:** 1.0 | **Author:** Mino

---

## Dependencies

| Resource | Required |
|----------|:--------:|
| ox_lib | ✅ |
| ESX (es_extended) | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Configuration

### Shops

```lua
CONFIG.SHOPS = {
    ["shop00"] = {
        ped = {
            model  = "s_m_m_ammucountry",
            coords = vector4(-1221.71, -908.18, 12.326, 0.0)
        },
        lockbox = {
            coords = vector4(-1221.310, -915.907, 11.096, 123.85),
            weight = 50000
        },
        items = {
            { name = "money", quantity = 1000 },
            { name = "weed",  quantity = 5    }
        },
        policeNotify   = "Suspicious activity at the store!",
        jobToNotify    = "police",
        tools          = "stetoscopio",
        minigame       = { game = "Locked", options = nil },
        timeForRobbery  = 30000,   -- 30 seconds
        startMoney      = 500,
        startMoneyStep  = 50,
        startMoneyDelay = 1000,
        shopResetTime   = 1800000  -- 30 minutes
    }
}
```

#### Shop Options

| Key | Type | Description |
|-----|------|-------------|
| `ped.model` | `string` | Cashier NPC model |
| `ped.coords` | `vector4` | NPC position and heading |
| `lockbox.coords` | `vector4` | Lockbox position and heading |
| `items` | `table` | Items inside the lockbox |
| `policeNotify` | `string` | Alert text sent to police |
| `tools` | `string` | Item required to open lockbox |
| `timeForRobbery` | `number` | Cash robbery duration (ms) |
| `startMoney` | `number` | Total cash from register |
| `startMoneyStep` | `number` | Cash given per animation step |
| `startMoneyDelay` | `number` | Ms between each cash step |
| `shopResetTime` | `number` | Ms before shop can be robbed again |

---

## Available Exports

!!! info "No exports registered"
    `avalon_robbery` does not expose any public exports.
