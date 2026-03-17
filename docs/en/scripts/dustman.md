# avalon_dustman

Script for the **garbage collector job** in FiveM. Supports solo mode (small truck) and group mode (large garbage truck). Players collect bags from bins in randomly assigned sectors.

**Version:** 1.0 | **Author:** Mino

---

## Dependencies

| Resource | Required |
|----------|:--------:|
| ox_lib | ✅ |
| ox_target | ✅ |
| ESX (es_extended) | ✅ |

---

## Installation

```
ensure avalon_dustman
```

---

## Configuration

### General Settings

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `Config.Debug` | `boolean` | `true` | Enable debug logs in console |
| `Config.PartyMembers` | `number` | `4` | Maximum players in a group |
| `Config.BagsPerSector` | `number` | `10` | Bags required to complete a sector |
| `Config.StressMax` | `number` | `10000` | Maximum stress to work |
| `Config.StressValue` | `number` | `100` | Stress added each time a bin is collected |

### Items

```lua
Config.BagItem = {
    name  = "spazzatura",           -- item added to inventory when picking up a bag
    label = "Garbage Bag",
}

Config.TicketItem    = "garbage_ticket" -- reward ticket item
Config.MoneyPerTicket = 50              -- $ value per ticket
```

### Work Modes

=== "Solo"

    ```lua
    Config.Single = {
        vehicle    = "utillitruck3",
        ticketRate = 5    -- tickets per bag
    }
    ```

=== "Group"

    ```lua
    Config.Group = {
        vehicle    = "trash",
        ticketRate = 4,        -- slightly less than solo
        maxTrucks  = 4,        -- max concurrent trucks on server
    }
    ```

### Key Points

```lua
Config.VehicleSpawn  = vec4(-340.0, -1560.0, 25.0, 90.0)
Config.VehicleReturn = vec3(-345.2671, -1570.3054, 25.2280)

Config.NPC = {
    job    = vec4(-350.0, -1565.0, 24.22, 90.0),   -- NPC to start job
    ticket = vec4(-345.0, -1565.0, 24.22, 90.0)    -- NPC to exchange tickets
}
```

### Sectors

```lua
Config.Sectors = {
    {
        name  = "Downtown",
        gps   = vec3(215.0, -810.0, 30.0),
        area  = { center = vec3(215.0, -810.0, 30.0), radius = 180.0 },
        props = Config.AllBins
    },
    -- add more sectors here
}
```

---

## Available Exports

!!! info "No exports registered"
    `avalon_dustman` does not expose any public exports.
