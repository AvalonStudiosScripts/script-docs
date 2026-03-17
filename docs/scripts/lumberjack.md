# avalon_lumberjack

Script per il **lavoro da taglialegna** in FiveM. Il giocatore abbatte alberi usando un minigioco a punteggio con supporto a diversi tipi di ascia, sistema di durabilità e bonus abilità.

**Versione:** 1.0 | **Autore:** Avalon

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |
| core_skills | ❌ (opzionale, per bonus abilità) |

---

## Installazione

```
ensure avalon_lumberjack
```

---

## Configurazione

### Lingua

```lua
Lang = "Italiano"
-- Lingue supportate: Italiano, English, Français, Deutsch, Español,
-- Português, Português (Brasil), Nederlands, Polski, Română,
-- Ελληνικά, 中文, ไทย, Filipino, Русский
```

### Minigioco a Punteggio

```lua
Config.scoreGame  = true   -- abilita il sistema a punteggio
Config.scoreToWin = 50     -- punteggio necessario per vincere
                           -- (nil o 0 = il gioco non si ferma mai)
```

### Velocità Base Minigioco

```lua
Config.BaseFireRateGood = 1000  -- ms tra un bersaglio e l'altro (tiro corretto)
Config.BaseFireRateBad  = 500   -- ms (tiro sbagliato, più veloce = più difficile)
```

### Asce

Ogni item ascia può avere bonus diversi.

```lua
Config.Axe = {
    ["money"] = {
        animDict        = "melee@large_wpn@streamed_core",
        anim            = "car_down_attack",
        propModel       = "prop_tool_fireaxe",
        BonusFireRate   = 50,  -- ms ridotti tra bersagli
        BonusLoot       = 2,   -- loot aggiuntivo
        BonusScore      = 1,   -- punti bonus per colpo
        BonusDurability = 0,   -- durabilità risparmiata per colpo
    },
    ["tool_consaw"] = {
        animDict        = "weapons@heavy@minigun",
        anim            = "fire_med",
        propModel       = "prop_tool_consaw",
        BonusFireRate   = 100,
        BonusLoot       = 3,
        BonusScore      = 2,
        BonusDurability = 3,
    },
}
```

### Posizione del Prop sull'Osso del Ped

```lua
Config.attachmentPositon = {
    ["prop_tool_fireaxe"] = {
        bones = 0xDEAD, x = 0.10, y = 0.02, z = -0.02,
        rx = -90.0, ry = 180.0, rz = 0.0
    },
    ["prop_tool_consaw"] = {
        bones = 0x49D9, x = 0.18, y = -0.09, z = 0.18,
        rx = -112.0, ry = -66.0, rz = -3.0
    },
}
```

### Sistema Durabilità

```lua
Config.durabilitySystemEnabled = true  -- abilita durabilità sull'item
Config.MaxDurability            = 200  -- durabilità massima
Config.Durability               = 15   -- durabilità persa ad ogni taglio
```

### Abilità (core_skills)

```lua
Config.Ability = { name = "taglialegna", n = 0 }

-- Bonus se il giocatore ha l'abilità:
Config.BonusFireRate = 200  -- ms in più
Config.BonusLoot     = 2
Config.BonusScore    = 3
```

### Coordinate Alberi

```lua
Config.LumberCoords = {
    vector3(-527.5469, 537.0093, 111.5338),
    vector3(-563.63, 540.23, 109.56),
    vector3(-567.52, 545.96, 109.56),
    vector3(-571.36, 551.66, 109.56),
}
```

!!! tip "Aggiungere nuove zone"
    Aggiungi semplicemente nuovi `vector3` nella tabella `Config.LumberCoords`.

### Loot

```lua
Config.Loot = {
    { name = "raw_material_wood", label = "Legna",  chance = 20, amount = 100, scoreGame = {min=1, max=5} },
    { name = "rubber",            label = "Gomma",  chance = 5,  amount = 100, scoreGame = {min=1, max=5} },
    { name = "fibers",            label = "Fibre",  chance = 8,  amount = 100, scoreGame = {min=1, max=5} },
}
```

| Campo | Tipo | Descrizione |
|-------|------|-------------|
| `name` | `string` | Nome dell'item nell'inventario |
| `label` | `string` | Nome visualizzato nelle notifiche |
| `chance` | `number` | Probabilità (1–100) che l'item droppi |
| `amount` | `number` | Limite stack (non usato direttamente) |
| `scoreGame.min/max` | `number` | Quantità random droppa basata sullo score |

### Step di Punteggio (Ricompensa Extra)

```lua
Config.Steps = {
    step1 = { count = 10, reward = 0  }, -- nessuna ricompensa sotto count
    step2 = { count = 20, reward = 20 },
    step3 = { count = 33, reward = 15 },
    step4 = { count = 66, reward = 10 },
    step5 = { count = 99, reward = 5  },
}
```

!!! info "Come funzionano gli step"
    Quando il giocatore raggiunge il punteggio `count`, riceve `reward` unità extra di loot aggiuntivo.

### Stress

```lua
Config.StressMax   = 1000000  -- stress massimo per fare il lavoro
Config.StressValue = 100      -- stress aggiunto al termine del minigioco
```

---

## Export Disponibili

!!! info "Nessun export registrato"
    `avalon_lumberjack` non espone export pubblici.

---

## Funzionamento

```
[Giocatore con ascia in inventario] → ox_target sulle coordinate albero
    ↓
Controllo ascia (durabilità) → Animazione con prop ascia
    ↓
Minigioco NUI (bersagli mobili) → Punteggio accumulato
    ↓
Fine minigioco → Server calcola loot da score + bonus → AddItem
    ↓
Ascia perde durabilità → Se = 0: item rimosso
```
