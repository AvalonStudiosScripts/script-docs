# avalon_mining

Script per il **lavoro da minatore** in FiveM. Struttura identica ad `avalon_lumberjack` ma adattata per la miniera: picconi al posto delle asce, rocce invece di alberi.

**Versione:** 1.0 | **Autore:** Avalon

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |
| core_skills | ❌ (opzionale) |

---

## Installazione

```
ensure avalon_mining
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
Config.scoreGame  = true
Config.scoreToWin = 50
```

### Velocità Base

```lua
Config.BaseFireRateGood = 1000  -- ms tra bersagli (corretto)
Config.BaseFireRateBad  = 1.5   -- moltiplicatore velocità (sbagliato)
```

### Picconi

```lua
Config.pickAxe = {
    ["tool_pickaxe"] = {
        animDict        = "melee@large_wpn@streamed_core",
        anim            = "car_down_attack",
        propModel       = "prop_tool_pickaxe",
        BonusFireRate   = 50,
        BonusLoot       = 2,
        BonusScore      = 1,
        BonusDurability = 0,
    },
    ["tool_jackham"] = {
        animDict        = "amb@world_human_const_drill@male@drill@base",
        anim            = "base",
        propModel       = "prop_tool_jackham",
        BonusFireRate   = 100,
        BonusLoot       = 3,
        BonusScore      = 2,
        BonusDurability = 3,
    },
}
```

### Posizione Prop sull'Osso

```lua
Config.attachmentPositon = {
    ["prop_tool_pickaxe"] = {
        bones = 0xDEAD, x = 0.10, y = 0.02, z = -0.02,
        rx = -90.0, ry = 180.0, rz = 0.0
    },
    ["prop_tool_jackham"] = {
        bones = 0xDEAD, x = 0.1, y = 0.11, z = -0.02,
        rx = -67.0, ry = -9.0, rz = -291.0
    },
}
```

### Sistema Durabilità

```lua
Config.durabilitySystemEnabled = true
Config.MaxDurability            = 200
Config.Durability               = 15  -- durabilità persa ogni colpo
```

### Abilità (core_skills)

```lua
Config.Ability = { name = "minatore", n = 0 }

Config.BonusFireRate = 200
Config.BonusLoot     = 2
Config.BonusScore    = 3
```

### Coordinate Miniera

```lua
Config.MinerCoords = {
    vector3(-1349.4496, 720.0062, 185.9951),
    vector3(-563.63, 540.23, 109.56),
    vector3(-567.52, 545.96, 109.56),
    vector3(-571.36, 551.66, 109.56),
}
```

### Loot

```lua
Config.Loot = {
    { name = "rock",   label = "Roccia", chance = 20, amount = 100, scoreGame = {min=1, max=5} },
    { name = "iron",   label = "Ferro",  chance = 5,  amount = 100, scoreGame = {min=1, max=5} },
    { name = "copper", label = "Rame",   chance = 8,  amount = 100, scoreGame = {min=1, max=5} },
}
```

### Step di Punteggio

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

### Stress

```lua
Config.StressMax   = 1000000
Config.StressValue = 100
```

---

## Export Disponibili

!!! info "Nessun export registrato"
    `avalon_mining` non espone export pubblici.

---

## Differenze rispetto a avalon_lumberjack

| Caratteristica | avalon_lumberjack | avalon_mining |
|----------------|:-----------------:|:-------------:|
| Tool | Asce | Picconi |
| Abilità | `taglialegna` | `minatore` |
| Loot default | Legna, Gomma, Fibre | Roccia, Ferro, Rame |
| Step aggiuntivi | 5 | 8 |
| Coordinate | Foresta | Miniera |
