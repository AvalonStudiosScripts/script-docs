# avalon_trash

Script per la **ricerca negli oggetti abbandonati** in FiveM. I giocatori possono cercare in cestini, cassonetti e cassette postali per trovare loot randomico, con sistema di cooldown e stash temporaneo.

**Versione:** 1.0 | **Autore:** Mino

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Installazione

```
ensure avalon_trash
```

---

## Configurazione

### Tempi

```lua
CONFIG.SEARCH_TIME        = 5000      -- ms di ricerca (progress bar)
CONFIG.COOLDOWN           = 60        -- secondi prima di poter cercare nello stesso contenitore
CONFIG.TRASH_REFRESH_TIME = 5 * 60    -- secondi prima di rigenerare il loot in tutti i contenitori
```

### Animazione

```lua
CONFIG.ANIMATION = {
    dict = "amb@prop_human_bum_bin@base",
    anim = "base"
}
```

### Stash

```lua
CONFIG.STASH = {
    slots  = 10,    -- slot dello stash temporaneo
    weight = 50000  -- peso massimo dello stash (grammi)
}
```

---

### Categorie Contenitori

Ogni categoria ha item richiesti (per aprire il contenitore) e item bonus che moltiplicano le probabilità.

```lua
CONFIG.TRASH_CATEGORIES = {

    small_bin = {
        label         = "Cestino",
        requiredItems = {},                 -- nessun item richiesto
        bonusItems    = {
            ["trash_gloves"] = { chanceMultiplier = 1.25, amountMultiplier = 1.4 }
        }
    },

    dumpster = {
        label         = "Cassonetto",
        requiredItems = { "trash_gloves" }, -- richiede guanti
        bonusItems    = {
            ["trash_gloves"] = { chanceMultiplier = 1.4, amountMultiplier = 1.6 }
        }
    },

    mailbox = {
        label         = "Cassetta delle lettere",
        requiredItems = { "crowbar" },      -- richiede piede di porco
        bonusItems    = {
            ["crowbar"] = { chanceMultiplier = 1.6, amountMultiplier = 1.0 }
        }
    }
}
```

| Categoria | Item Richiesto | Bonus con item |
|-----------|:---:|---|
| `small_bin` | ❌ | +25% chance, +40% quantity con `trash_gloves` |
| `dumpster` | `trash_gloves` | +40% chance, +60% quantity con `trash_gloves` |
| `mailbox` | `crowbar` | +60% chance con `crowbar` |

---

### Mapping Prop → Categoria

Assegna ogni modello 3D a una categoria tramite la tabella `CONFIG.TRASH_PROPS`.

```lua
CONFIG.TRASH_PROPS = {
    -- Small Bin
    ["prop_bin_01a"] = "small_bin",
    ["prop_bin_02a"] = "small_bin",
    -- Dumpster
    ["prop_dumpster_01a"] = "dumpster",
    ["prop_dumpster_02a"] = "dumpster",
    -- Mailbox
    ["prop_mailbox_01a"] = "mailbox",
}
```

!!! tip "Aggiungere nuovi prop"
    Aggiungi il nome del modello come chiave e la categoria come valore.

---

### Loot per Categoria

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

| Campo | Tipo | Descrizione |
|-------|------|-------------|
| `item` | `string` | Nome item nell'inventario |
| `chance` | `number` | Probabilità (1–100) |
| `amount` | `{min, max}` | Quantità randomica |

---

### Override per Posizione Specifica

Puoi assegnare un loot diverso a un contenitore specifico tramite coordinate.

```lua
CONFIG.TRASH_OVERRIDE_POS = {
    {
        model  = "prop_bin_01a",
        coords = vec3(150.24, -1040.32, 29.37),
        radius = 1.0,
        loot   = {
            { item = "money_dirty", chance = 20, amount = {50, 150} },
            { item = "wallet",      chance = 40, amount = {1, 1}    }
        }
    },
    {
        model  = "prop_dumpster_4a",
        coords = vec3(992.15, -3105.42, 5.90),
        radius = 1.5,
        loot   = {
            { item = "weapon_parts", chance = 15, amount = {1, 2} },
            { item = "scrap",        chance = 80, amount = {2, 6} }
        }
    }
}
```

!!! info "Come funziona l'override"
    Se il contenitore target è entro `radius` dalle coordinate specificate **e** corrisponde al `model`, viene usato il loot dell'override invece del loot standard della categoria.

---

## Export Disponibili

!!! info "Nessun export registrato"
    `avalon_trash` non espone export pubblici.

---

## Funzionamento

```
[Giocatore vicino a un contenitore] → ox_target "Cerca nel cestino"
    ↓
Controllo item richiesti → Animazione → Progress bar (SEARCH_TIME ms)
    ↓
Server: calcola loot (override POS > categoria standard)
        Applica moltiplicatori bonus (item in inventario)
    ↓
Stash temporaneo generato → ox_inventory apre lo stash
    ↓
Cooldown attivato → Refresh globale ogni TRASH_REFRESH_TIME secondi
```
