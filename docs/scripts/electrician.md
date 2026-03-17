# avalon_electrician

Script per il **lavoro da elettricista** in FiveM. I giocatori riparano centraline elettriche attraverso un sistema di minigiochi su tre livelli di difficoltà.

**Versione:** 1.0 | **Autore:** Mino

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| ESX (es_extended) | ✅ |
| framework:PlayMinigame | ✅ |

---

## Installazione

```
ensure avalon_electrician
```

---

## Configurazione

### Debug

```lua
CONFIG.debug = false  -- true per abilitare log in console
```

### NPC

L'NPC è il punto di inizio del lavoro.

```lua
CONFIG.NPC = {
    model     = "S_M_Y_DockWork_01",
    coords    = vector4(-381.52, -61.21, 53.45, 160.0),
    animDict  = "mini@repair",
    animName  = "fixing_a_ped",
}
```

### Parcheggi Veicolo

Fino a 3 parcheggi dove viene spawnat il furgone.

```lua
CONFIG.PARKS = {
    vector3(-383.12, -64.25, 53.45),
    vector3(-385.67, -59.78, 53.45),
    vector3(-379.45, -58.32, 53.45),
}

CONFIG.VEHICLE     = { model = 'burrito3' }
CONFIG.RETURN_PARK = vector3(-401.2466, -82.0699, 54.4231)
```

### Item Tool

```lua
CONFIG.toolItem    = 'toolbox'                                  -- tool base (obbligatorio)
CONFIG.toolItemPro = { name = 'toolbox_pro', multiply = 1.3 }  -- tool pro: riduce il tempo del 30%
```

### Livelli di Difficoltà

Il lavoro ha **3 livelli** di difficoltà, ognuno con spot, minigiochi, pagamenti e penalità dedicati.

| Livello | Nome | Spot | Pagamento | Penalità fallimento |
|---------|------|:----:|-----------|:-------------------:|
| `1` | Semplice (Apprendista) | 33 | 100–150 ticket | 10 ticket |
| `2` | Media (Standard) | 25 | 200–300 ticket | 25 ticket |
| `3` | Difficile (Pro) | 15 | 400–600 ticket | 50 ticket |

### Spot di Lavoro

I `CONFIG.SPOTS` sono le posizioni delle centraline sulla mappa. Ogni spot ha una difficoltà.

```lua
CONFIG.SPOTS = {
    { difficulty = 1, vector3(1035.19, -3341.68, 6.09) }, -- Semplice
    { difficulty = 2, vector3(1165.89, -2935.47, 11.19) }, -- Media
    { difficulty = 3, vector3(887.80,  -986.57,  44.17) }, -- Difficile
    -- ... aggiungi quanti spot vuoi
}
```

### Minigiochi

Ogni difficoltà ha la sua configurazione del minigioco (`ElectricalBox`).

=== "Difficoltà 1 — Semplice"

    ```lua
    CONFIG.MINIGAMES[1] = {
        game = "ElectricalBox",
        metadata = {
            numberOfStages      = 3,    -- stadi totali
            timePerStage        = 20,   -- secondi per stadio
            simonSequenceLength = 2,    -- lunghezza sequenza Simon
            puzzleTypes = {
                math   = true,
                simon  = false,
                current = false,
            },
            operators = { multiply = true, division = false }
        }
    }
    ```

=== "Difficoltà 2 — Media"

    ```lua
    CONFIG.MINIGAMES[2] = {
        game = "ElectricalBox",
        metadata = {
            numberOfStages      = 5,
            timePerStage        = 20,
            simonSequenceLength = 4,
            puzzleTypes = {
                math   = true,
                simon  = true,
                current = false,
            },
            operators = { multiply = true, division = false }
        }
    }
    ```

=== "Difficoltà 3 — Difficile"

    ```lua
    CONFIG.MINIGAMES[3] = {
        game = "ElectricalBox",
        metadata = {
            numberOfStages      = 10,
            timePerStage        = 20,
            simonSequenceLength = 6,
            puzzleTypes = {
                math    = true,
                simon   = true,
                current = true,
            },
            operators = { multiply = true, division = true }
        }
    }
    ```

### Distanza e Durata

```lua
CONFIG.MISC = {
    [1] = { drawDistance = 10.0, jobDuration = 60 }, -- secondi
    [2] = { drawDistance = 15.0, jobDuration = 90 },
    [3] = { drawDistance = 20.0, jobDuration = 12 },
}
```

### Animazioni

```lua
CONFIG.ANIM = {
    [1] = { dict = "mini@repair",               name = "fixing_a_ped" },
    [2] = { dict = "missheistdockssetup1clipboard@base", name = "base" },
    [3] = { dict = "anim@amb@business@coc@coc_unpacking_hi@", name = "full_unpacking_hi_5_amb_coc" },
}
```

### Stress

```lua
CONFIG.StressMax   = 10000  -- stress massimo per fare il lavoro
CONFIG.StressValue = 100    -- stress aggiunto per ogni centralina
```

### Item Pagamento

```lua
CONFIG.PaymentItem = "ticket"  -- item dato come ricompensa
```

---

## Export Disponibili

!!! info "Nessun export registrato"
    `avalon_electrician` non espone export pubblici.

---

## Funzionamento

```
[Giocatore] → NPC → Scelta difficoltà (1/2/3)
    ↓
Spawn furgone → Assegnati 3/5/10 spot casuali del livello scelto
    ↓
Guida agli spot → Animazione riparazione → Minigioco ElectricalBox
    ↓
Successo: +ticket  |  Fallimento: -penalità ticket
    ↓
Restituzione furgone → Pagamento finale (- penalità danni veicolo)
```

!!! warning "Danno al veicolo"
    Il pagamento finale viene ridotto proporzionalmente alla percentuale di danno subita dal furgone. Guida con attenzione!
