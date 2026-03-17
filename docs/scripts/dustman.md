# avalon_dustman

Script per il **lavoro da netturbino** in FiveM. Supporta la modalità singola (furgoncino) e modalità gruppo (camion). I giocatori raccolgono sacchi dai bidoni in settori assegnati casualmente.

**Versione:** 1.0 | **Autore:** Mino

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| ox_target | ✅ |
| ESX (es_extended) | ✅ |

---

## Installazione

Aggiungi nell'`server.cfg`:

```
ensure avalon_dustman
```

---

## Configurazione

Il file `config.lua` si trova nella root dello script.

### Impostazioni Generali

| Opzione | Tipo | Default | Descrizione |
|---------|------|---------|-------------|
| `Config.Debug` | `boolean` | `true` | Abilita log di debug nella console |
| `Config.PartyMembers` | `number` | `4` | Numero massimo di giocatori in gruppo |
| `Config.BagsPerSector` | `number` | `10` | Sacchi da raccogliere per completare un settore |
| `Config.StressMax` | `number` | `10000` | Stress massimo per fare il lavoro |
| `Config.StressValue` | `number` | `100` | Stress aggiunto ogni volta che si raccoglie un cestino |

### Item

```lua
Config.BagItem = {
    name  = "spazzatura",           -- item aggiunto all'inventario quando si prende un sacco
    label = "Sacco dell'immondizia",
}

Config.TicketItem    = "garbage_ticket" -- item ticket dato come pagamento
Config.MoneyPerTicket = 50              -- valore in $ di ogni ticket
```

### Modalità di Lavoro

=== "Singolo"

    ```lua
    Config.Single = {
        vehicle    = "utillitruck3", -- modello veicolo
        ticketRate = 5               -- ticket guadagnati per sacco
    }
    ```

=== "Gruppo"

    ```lua
    Config.Group = {
        vehicle    = "trash",  -- modello camion
        ticketRate = 4,        -- ticket guadagnati per sacco (meno del singolo)
        maxTrucks  = 4,        -- numero massimo di camion contemporanei
    }
    ```

### Punti Importanti

```lua
Config.VehicleSpawn  = vec4(-340.0, -1560.0, 25.0, 90.0)  -- spawn veicolo
Config.VehicleReturn = vec3(-345.2671, -1570.3054, 25.2280) -- punto restituzione

Config.NPC = {
    job    = vec4(-350.0, -1565.0, 24.22, 90.0),   -- NPC per iniziare lavoro
    ticket = vec4(-345.0, -1565.0, 24.22, 90.0)    -- NPC per scambiare ticket
}
```

### Blip e Marker

```lua
Config.BlipSprite  = 318
Config.BlipColour  = 2

Config.MarkerColor = { red = 0, green = 255, blue = 0, alpha = 120 }
Config.MarkerSize  = { x = 6.0, y = 6.0, z = 2.0 }
```

### Bidoni (Prop)

La tabella `Config.AllBins` elenca tutti i modelli riconosciuti come contenitori da svuotare. Puoi aggiungere o rimuovere prop in base alla mappa del tuo server.

```lua
Config.AllBins = {
    -- Cestini piccoli/medi
    "prop_bin_01a", "prop_bin_02a", "prop_bin_03a", "prop_bin_04a",
    "prop_bin_05a", "prop_bin_06a", "prop_bin_07a",
    "prop_cs_bin_01", "prop_cs_rub_binbag_01",
    "prop_rub_binbag_sd_01", "prop_rub_binbag_sd_02",
    "prop_rub_binbag_sd_03", "prop_rub_binbag_sd_04",
    -- Dumpster grandi
    "prop_dumpster_01a", "prop_dumpster_02a", "prop_dumpster_02b",
    "prop_dumpster_03a", "prop_dumpster_03b",
    "prop_dumpster_04a", "prop_dumpster_04b"
}
```

### Settori

Ogni settore rappresenta una zona della mappa in cui vengono assegnati i bidoni. Puoi aggiungere quanti settori vuoi.

```lua
Config.Sectors = {
    {
        name  = "Downtown",
        gps   = vec3(215.0, -810.0, 30.0),  -- punto GPS sul blip
        area  = {
            center = vec3(215.0, -810.0, 30.0),
            radius = 180.0                   -- raggio dell'area in unità
        },
        props = Config.AllBins               -- prop accettati in questo settore
    },
    {
        name  = "Strawberry",
        gps   = vec3(120.0, -1340.0, 29.0),
        area  = { center = vec3(120.0, -1340.0, 29.0), radius = 160.0 },
        props = Config.AllBins
    },
    {
        name  = "Vespucci",
        gps   = vec3(-1220.0, -910.0, 12.0),
        area  = { center = vec3(-1220.0, -910.0, 12.0), radius = 200.0 },
        props = Config.AllBins
    }
}
```

!!! tip "Aggiungere un nuovo settore"
    Copia uno dei blocchi sopra e modifica `name`, `gps`, `area.center` e `area.radius` in base alla zona desiderata.

---

## Export Disponibili

!!! info "Nessun export registrato"
    `avalon_dustman` non espone export pubblici. Tutte le funzionalità sono interne allo script.

---

## Funzionamento

```
[Giocatore] → NPC "Inizia lavoro" → Scelta modalità (Singolo/Gruppo)
    ↓
Spawn veicolo → Assegnazione settore casuale → GPS impostato
    ↓
Raccolta bidoni (ox_target su ogni prop) → Sacco in inventario → Deposito nel camion
    ↓
Completati Config.BagsPerSector sacchi → Nuovo settore assegnato
    ↓
Restituzione veicolo → NPC "Consegna ticket" → Pagamento
```

!!! warning "Modalità Gruppo"
    In modalità gruppo, il leader invita altri giocatori tramite `ox_target` sul loro ped. Il camion può avere un massimo di `Config.Group.maxTrucks` attivi contemporaneamente sul server.
