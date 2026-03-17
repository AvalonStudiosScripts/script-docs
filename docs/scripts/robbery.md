# avalon_robbery

Script per la **rapina ai negozi** in FiveM. I giocatori puntano un'arma verso il commesso per iniziare la rapina, raccogliere denaro progressivamente e aprire la cassaforte tramite minigioco.

**Versione:** 1.0 | **Autore:** Mino

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| ESX (es_extended) | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Installazione

```
ensure avalon_robbery
```

---

## Configurazione

```lua
DEBUG = false  -- true per abilitare log in console
```

### Negozi (`CONFIG.SHOPS`)

Ogni negozio è una voce nella tabella `CONFIG.SHOPS` identificata da una chiave unica.

```lua
CONFIG.SHOPS = {
    ["shop00"] = {
        -- ...
    },
    ["shop01"] = {
        -- ...
    },
}
```

#### Opzioni per ogni negozio

| Chiave | Tipo | Descrizione |
|--------|------|-------------|
| `ped.model` | `string` | Modello del commesso NPC |
| `ped.coords` | `vector4` | Posizione e heading del commesso |
| `lockbox.coords` | `vector4` | Posizione e heading della cassaforte |
| `lockbox.weight` | `number` | Peso della cassaforte (usato internamente) |
| `items` | `table` | Lista item nella cassaforte `{name, quantity}` |
| `policeNotify` | `string` | Testo dell'alert inviato alla polizia |
| `jobToNotify` | `string` | Job ESX a cui inviare la notifica (`"police"`) |
| `tools` | `string` | Item necessario per aprire la cassaforte |
| `minigame.game` | `string` | Tipo di minigioco (`"Locked"`) |
| `timeForRobbery` | `number` | Durata rapina cassa in ms |
| `startMoney` | `number` | Denaro totale dalla cassa all'inizio |
| `startMoneyStep` | `number` | Denaro dato ogni step di animazione |
| `startMoneyDelay` | `number` | Ms tra ogni step di denaro |
| `shopResetTime` | `number` | Ms prima che il negozio sia di nuovo rapinabile |

#### Esempio completo

```lua
CONFIG.SHOPS = {
    ["shop00"] = {
        ped = {
            model  = "s_m_m_ammucountry",
            coords = vector4(-1221.71, -908.18, 12.326, 0.0)
        },
        lockbox = {
            coords = vector4(-1221.310, -915.907, 11.096, 123.85),
            weight = 50000  -- 50 kg
        },
        items = {
            { name = "money",  quantity = 1000 },
            { name = "weed",   quantity = 5    }
        },
        policeNotify  = "Attività sospetta all Negozio di alimentari!",
        jobToNotify   = "police",
        metadataNotify = nil,
        tools         = "stetoscopio",
        minigame      = { game = "Locked", options = nil },
        timeForRobbery  = 30000,   -- 30 secondi
        startMoney      = 500,
        startMoneyStep  = 50,
        startMoneyDelay = 1000,
        shopResetTime   = 1800000  -- 30 minuti
    }
}
```

!!! tip "Aggiungere un nuovo negozio"
    Copia il blocco `["shop00"]` e assegna una nuova chiave (es. `"shop01"`). Cambia coordinate ped, lockbox e contenuto items.

---

## Export Disponibili

!!! info "Nessun export registrato"
    `avalon_robbery` non espone export pubblici.

---

## Funzionamento

```
[Giocatore con arma] → Punta arma verso il commesso NPC
    ↓
NPC si "spaventa" → StartRobbery() → Animazione mani alzate commesso
    ↓
Giocatore riceve denaro ogni startMoneyDelay ms (startMoneyStep $)
    ↓ (rimane in zona, continua a puntare)
Raggiunti tutti i soldi della cassa OR il giocatore si allontana → Stop
    ↓
[Opzionale] ox_target sulla cassaforte → Usa item tools → Minigioco "Locked"
    ↓
Successo minigioco → Stash temporaneo con gli items della cassaforte
    ↓
shopResetTime → Negozio resettato e di nuovo rapinabile
```

!!! warning "Notifica Polizia"
    All'inizio della rapina viene inviato un alert ESX al job `jobToNotify`. Assicurati che il tuo framework gestisca l'evento `SendAlert`.
