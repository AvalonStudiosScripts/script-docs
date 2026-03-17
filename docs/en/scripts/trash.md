# avalon_trash

Script for **searching abandoned objects** in FiveM. Players can search bins, dumpsters, and mailboxes for random loot, with a cooldown system and temporary stash.

**Version:** 1.0 | **Author:** Mino

---

## Dependencies

| Resource | Required |
|----------|:--------:|
| ox_lib | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Configuration

### Timing

```lua
CONFIG.SEARCH_TIME        = 5000    -- ms for search progress bar
CONFIG.COOLDOWN           = 60      -- seconds before searching same container again
CONFIG.TRASH_REFRESH_TIME = 5 * 60  -- seconds before all loot regenerates
```

### Container Categories

| Category | Required Item | Bonus with item |
|----------|:---:|---------|
| `small_bin` | ❌ | +25% chance, +40% qty with `trash_gloves` |
| `dumpster` | `trash_gloves` | +40% chance, +60% qty with `trash_gloves` |
| `mailbox` | `crowbar` | +60% chance with `crowbar` |

### Loot per Category

```lua
CONFIG.TRASH_LOOT = {
    small_bin = {
        { item = "weed",      chance = 50,  amount = {1, 20} },
        { item = "griffon_2", chance = 100, amount = {0, 1}  }
    },
    dumpster = {
        { item = "scrap",       chance = 70, amount = {2, 6} },
        { item = "electronics", chance = 20, amount = {1, 2} }
    },
    mailbox = {
        { item = "letter",      chance = 80, amount = {1, 2}   },
        { item = "money_dirty", chance = 15, amount = {20, 60} }
    }
}
```

### Position Override

Override loot for a specific prop at specific coordinates:

```lua
CONFIG.TRASH_OVERRIDE_POS = {
    {
        model  = "prop_bin_01a",
        coords = vec3(150.24, -1040.32, 29.37),
        radius = 1.0,
        loot   = {
            { item = "money_dirty", chance = 20, amount = {50, 150} }
        }
    }
}
```

---

## Available Exports

!!! info "No exports registered"
    `avalon_trash` does not expose any public exports.
