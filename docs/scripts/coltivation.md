# avalon_coltivation

Script per la **coltivazione di piante** in FiveM. Permette ai giocatori di piantare, annaffiare, fertilizzare e raccogliere diverse colture su terreni validi.

**Versione:** 1.0 | **Autore:** Mino | **Database:** ✅ MySQL

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| oxmysql | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Installazione

1. Aggiungi la cartella `avalon_coltivation` nella tua directory `resources/`
2. Esegui il file SQL (creato automaticamente all'avvio):

```sql
CREATE TABLE IF NOT EXISTS farming_plants (
    id INT NOT NULL AUTO_INCREMENT,
    plant_type VARCHAR(50) NOT NULL,
    x FLOAT NOT NULL,
    y FLOAT NOT NULL,
    z FLOAT NOT NULL,
    growth FLOAT NOT NULL DEFAULT 0,
    stage VARCHAR(20) NOT NULL DEFAULT 'seed',
    planted_at BIGINT NOT NULL,
    watered TINYINT(1) NOT NULL DEFAULT 0,
    has_lamp TINYINT DEFAULT 0,
    indoor TINYINT(1) DEFAULT 0,
    heading FLOAT DEFAULT 0,
    boosts_used LONGTEXT NULL,
    owner VARCHAR(60) NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

3. Aggiungi nell'`server.cfg`:

```
ensure avalon_coltivation
```

---

## Configurazione

Il file `config.lua` si trova nella root dello script.

### Impostazioni Generali

| Opzione | Tipo | Default | Descrizione |
|---------|------|---------|-------------|
| `CONFIG.MIN_PLANT_DISTANCE` | `number` | `1.5` | Distanza minima (in unità) tra due piante |
| `CONFIG.MIN_STAGE_TIME` | `number` | `300000` | Tempo minimo (ms) per avanzare di stadio |
| `CONFIG.StressMax` | `number` | `1000000` | Stress massimo per poter piantare |
| `CONFIG.StressValue` | `number` | `100` | Stress aggiunto ad ogni pianta posizionata |

### Posizionamento

```lua
CONFIG.PLACEMENT = {
    rotateStep = 5.0,    -- gradi di rotazione per ogni step
    maxDistance = 5.0    -- distanza massima per posizionare una pianta
}
```

### Stadi di Crescita

```lua
CONFIG.STAGES = {
    { percent = 0,   name = 'seed' },    -- Seme
    { percent = 33,  name = 'small' },   -- Piccola
    { percent = 66,  name = 'grown' },   -- Cresciuta
    { percent = 100, name = 'ready' }    -- Pronta al raccolto
}
```

### Raccolta

```lua
CONFIG.HARVEST = {
    tool          = 'shears',  -- item necessario per raccogliere
    breakChance   = 0.2,       -- probabilità di rompere la pianta senza tool (20%)
    toolBreakChance = 0.05,    -- probabilità di rompere usando il tool (5%)
    skillBonus    = 0.1        -- bonus successo con skill "coltivazione" (+10%)
}
```

### Piante

Ogni pianta è configurabile nella tabella `CONFIG.PLANTS`.

```lua
CONFIG.PLANTS = {
    grain = {
        seed        = 'seme_grano',             -- nome item seme
        growthTime  = 30 * 60 * 1000,           -- tempo crescita completa (ms)
        yield       = { item = 'grano', quantity = 5 }, -- raccolto
        waterNeeded = true,                     -- richiede annaffiatura
        props = { ... }                         -- modelli 3D per ogni stadio
    },
    -- ...
}
```

| Pianta | Seme | Tempo | Raccolto | Acqua |
|--------|------|-------|----------|:-----:|
| `grain` | `seme_grano` | 30 min | 5x `grano` | ✅ |
| `canapa` | `seme_canapa` | 45 min | 3x `canapa` | ✅ |
| `coca` | `seme_coca` | 60 min | 2x `coca` | ✅ |
| `fungus` | `seme_fungo` | 20 min | 4x `funghi` | ❌ |
| `papavero` | `seme_papavero` | 50 min | 2x `papavero` | ✅ |

### Tool / Attrezzi

Gli attrezzi accelerano la crescita e si applicano in stadi specifici.

| Tool | Item | Moltiplicatore | Si rompe | Stadi |
|------|------|:--------------:|:--------:|-------|
| Annaffiatoio | `watering_can` | -25% tempo | ❌ | seed, small |
| Fertilizzante | `fertilizer` | -40% tempo | ✅ (uso singolo) | small, grown |
| Lampada | `grow_lamp` | -15% tempo | ✅ (uso singolo) | seed, small, grown |

### Terreni Validi

Lo script valida il terreno tramite collision material. Sono accettati:

- **Erba** — `GrassLong`, `Grass`, `GrassShort`
- **Terra / Fango** — `Soil`, `DirtTrack`, `MudHard`, `MudSoft`, `MudDeep`
- **Palude** — `Marsh`, `MarshDeep`
- **Sabbia** — `SandLoose`, `SandCompact`, `SandWet`, `SandDryDeep`
- **Argilla** — `ClaySoft`, `ClayHard`

!!! tip "Aggiungere terreni personalizzati"
    Aggiungi l'hash del material nella tabella `CONFIG.VALID_SOIL_MATERIALS`:
    ```lua
    CONFIG.VALID_SOIL_MATERIALS["0xTUOHASH"] = true
    ```

---

## Export Disponibili

!!! info "Tipo: Client-Side"
    Questi export sono registrati lato **client** e vanno chiamati da script client.

Ogni export avvia il processo di pianta del seme corrispondente per il giocatore locale.

### `seme_grano`

Avvia il piazzamento del seme di grano.

```lua
exports['avalon_coltivation']:seme_grano()
```

---

### `seme_canapa`

Avvia il piazzamento del seme di canapa.

```lua
exports['avalon_coltivation']:seme_canapa()
```

---

### `seme_coca`

Avvia il piazzamento del seme di coca.

```lua
exports['avalon_coltivation']:seme_coca()
```

---

### `seme_fungo`

Avvia il piazzamento del seme di fungo.

```lua
exports['avalon_coltivation']:seme_fungo()
```

---

### `seme_papavero`

Avvia il piazzamento del seme di papavero.

```lua
exports['avalon_coltivation']:seme_papavero()
```

---

## Esempio di Utilizzo

### Piantare da un altro script

```lua
-- client/main.lua del tuo script

-- Assegna il piazzamento del grano a un tasto o interazione
RegisterCommand('pianta_grano', function()
    exports['avalon_coltivation']:seme_grano()
end)

-- Oppure tramite ox_target
exports.ox_target:addBoxZone({
    coords = vector3(100.0, 200.0, 30.0),
    size   = vector3(2, 2, 2),
    options = {
        {
            label = 'Pianta grano',
            onSelect = function()
                exports['avalon_coltivation']:seme_grano()
            end
        }
    }
})
```

### Piantare tutti i tipi disponibili (menu)

```lua
-- client/main.lua del tuo script
local seeds = {
    { label = 'Grano',    export = 'seme_grano'    },
    { label = 'Canapa',   export = 'seme_canapa'   },
    { label = 'Coca',     export = 'seme_coca'      },
    { label = 'Fungo',    export = 'seme_fungo'     },
    { label = 'Papavero', export = 'seme_papavero'  },
}

RegisterCommand('apri_menu_piante', function()
    local options = {}
    for _, seed in ipairs(seeds) do
        local name = seed.export
        table.insert(options, {
            title = seed.label,
            onSelect = function()
                exports['avalon_coltivation'][name]()
            end
        })
    end
    lib.registerContext({ id = 'menu_piante', title = 'Piante', options = options })
    lib.showContext('menu_piante')
end)
```

---

## Database

La tabella `farming_plants` salva ogni pianta persistentemente.

| Colonna | Tipo | Descrizione |
|---------|------|-------------|
| `id` | INT | ID univoco |
| `plant_type` | VARCHAR | Tipo pianta (grain, canapa, ecc.) |
| `x`, `y`, `z` | FLOAT | Posizione nel mondo |
| `growth` | FLOAT | Percentuale crescita (0-100) |
| `stage` | VARCHAR | Stadio attuale (seed/small/grown/ready) |
| `planted_at` | BIGINT | Timestamp pianta (ms) |
| `watered` | TINYINT | Annaffiata (0/1) |
| `has_lamp` | TINYINT | Lampada attiva (0/1) |
| `indoor` | TINYINT | In interni (0/1) |
| `heading` | FLOAT | Rotazione pianta |
| `boosts_used` | LONGTEXT | JSON tool usati |
| `owner` | VARCHAR | Identifier proprietario |
