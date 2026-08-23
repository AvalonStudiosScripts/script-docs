# avalon_mounts

Script per l'utilizzo di **creature cavalcabili** in FiveM. Permette ai giocatori di evocare, montare e gestire creature fantastiche (draghi, grifoni, pegasi, cavalli, alpaca) tramite comandi o item nell'inventario.

**Versione:** 1.0 | **Autore:** Mino

---

## Dipendenze

| Resource | Obbligatoria | Note |
|----------|:---:|------|
| ox_lib | ✅ | Notifiche e utility |
| ox_inventory | ❌ | Solo se `CONFIG.items = true` |
| ox_fuel | ❌ | Mantiene carburante al massimo automaticamente |
| qbx_core | ❌ | Integrazione chiavi QBox |
| qbx_vehiclekeys | ❌ | Integrazione chiavi QBox |

---

## Installazione

```
ensure avalon_mounts
```

---

## Utilizzo Base (Comandi)

Quando `CONFIG.command = true`, i giocatori possono evocare e congedare le creature direttamente dalla chat senza bisogno di item o altri script.

### Evocare una creatura

```
/mount <nome> <variante>
```

| Parametro | Tipo | Descrizione |
|-----------|------|-------------|
| `nome` | `string` | Tipo di creatura (vedi tabella sotto) |
| `variante` | `number` | Numero variante (1, 2, 3...) |

**Esempi:**

```
/mount necrodragon 1
/mount griffon 2
/mount pegaso 1
/mount horse 3
/mount alpaca 1
```

### Congedare la creatura

```
/delMount
```

Rimuove la creatura attualmente evocata dal gioco.

---

## Tasti in Gioco

Una volta evocata la creatura, questi tasti controllano le azioni:

| Azione | Tasto Default | Codice Config |
|--------|:---:|---|
| Monta / Smonta | **E** | `CONFIG.keyMount = 51` |
| Congeda la creatura | **H** | `CONFIG.keyDelete = 74` |
| Attiva / Disattiva volo | **X** | `CONFIG.keyFlymode = 73` |

!!! info "Cambio tasti"
    Per cambiare i tasti modifica i valori numerici in `config.lua`. I codici corrispondono ai [FiveM control codes](https://docs.fivem.net/docs/game-references/controls/).

---

## Creature Disponibili

| Creatura | Comando | Varianti | Può volare |
|----------|---------|:--------:|:----------:|
| Necrodragon | `/mount necrodragon` | 5 (1–5) | ✅ |
| Griffon | `/mount griffon` | 3 (1–3) | ✅ |
| Pegaso | `/mount pegaso` | 3 (1–3) | ✅ |
| Horse | `/mount horse` | 3 (1–3) | ❌ |
| Alpaca | `/mount alpaca` | 3 (1–3) | ❌ |

---

## Configurazione

### Impostazioni Generali

| Opzione | Tipo | Default | Descrizione |
|---------|------|---------|-------------|
| `CONFIG.command` | `boolean` | `true` | Abilita i comandi `/mount` e `/delMount` |
| `CONFIG.commandName` | `string` | `"mount"` | Nome del comando evoca |
| `CONFIG.delCommand` | `string` | `"delMount"` | Nome del comando congeda |
| `CONFIG.items` | `boolean` | `true` | Abilita spawn tramite item (richiede ox_inventory) |
| `CONFIG.invincible` | `boolean` | `false` | Rende la creatura invincibile |
| `CONFIG.maxrangedespawn` | `number` | `200.0` | Distanza (unità) per il despawn automatico |
| `CONFIG.timeForSpawnAfterDeaths` | `number` | `10000` | Attesa (ms) per evocare dopo la morte |

### Sistema Volo

```lua
CONFIG.flyWithTime = false   -- false = volo illimitato
CONFIG.flyTimeMax  = 60000   -- ms di volo (solo se flyWithTime = true)
CONFIG.flyCooldown = 60000   -- ms di ricarica prima di poter rivolare
```

### Velocità Massima

Valore di moltiplicazione interno — da testare e calibrare sul proprio server.

```lua
CONFIG.MaxSpeed = {
    ["necrodragon"] = 100.0,
    ["griffon"]     = 50.0,
    ["pegaso"]      = 70.0,
    ["alpaca"]      = 50.0,
    ["horse"]       = 70.0,
}
```

### Notifiche

```lua
CONFIG.notify          = true
CONFIG.notifyTitle     = "Avalon Mounts"
CONFIG.notifyPosition  = "top"       -- top, bottom, top-left, top-right, ecc.
CONFIG.notifyIcon      = "ban"
CONFIG.notifyIconColor = '#C53030'
CONFIG.notifyDuration  = 5000        -- ms
```

### Testi UI

```lua
CONFIG.lang = {
    mount        = "Sali",
    unmount      = "Scendi",
    deleteMount  = "Congeda",
    description  = "La creatura è stanca",
    description1 = "La creatura non si è ancora ripresa",
    description2 = "La creatura si è ripresa",
    description3 = "Sei morto recentemente, devi aspettare per richiamare la creatura",
}
```

---

## Export Disponibili

### `API` — Client-Side

!!! info "Tipo: Client-Side"
    Restituisce l'oggetto API con tutte le funzioni pubbliche. Da usare in altri script per controllare il sistema mounts in modo programmativo.

```lua
local MountsAPI = exports['avalon_mounts']:API()
```

#### Funzioni

| Funzione | Descrizione |
|----------|-------------|
| `MountsAPI.spawn(name, color)` | Evoca una creatura (`name` = tipo, `color` = numero variante) |
| `MountsAPI.despawn()` | Congeda la creatura attiva |
| `MountsAPI.isSpawned()` | `true` se una creatura è evocata |
| `MountsAPI.isOnMount()` | `true` se il giocatore è montato |
| `MountsAPI.getVehicle()` | Restituisce l'entità veicolo della creatura |
| `MountsAPI.getData(model)` | Dati di configurazione di una creatura specifica |
| `MountsAPI.getAllData()` | Dati di tutte le creature |
| `MountsAPI.getBlacklist(vehicle)` | Controlla se un veicolo è in blacklist |
| `MountsAPI.sendInfo(data)` | Invia dati di stato |
| `MountsAPI.deleteEnt(ent)` | Elimina un'entità specifica |
| `MountsAPI.playAnimation(prop, anim, dict, loop)` | Riproduce un'animazione sulla creatura |
| `MountsAPI.syncInvisible(ent, bool)` | Sincronizza visibilità tra tutti i client |
| `MountsAPI.syncHair(ent, head)` | Sincronizza variante testa |

#### Esempi

```lua
-- client/main.lua del tuo script
local MountsAPI = exports['avalon_mounts']:API()

-- Evoca un griffon variante 2
MountsAPI.spawn("griffon", 2)

-- Controlla se il giocatore è a cavallo
if MountsAPI.isOnMount() then
    print("Giocatore montato")
end

-- Ottieni la posizione della creatura
local mount = MountsAPI.getVehicle()
if mount then
    local coords = GetEntityCoords(mount)
    print(("Mount a: %.1f, %.1f, %.1f"):format(coords.x, coords.y, coords.z))
end

-- Congeda
MountsAPI.despawn()
```

---

### Export Item Dinamici — Client-Side

!!! info "Tipo: Client-Side — richiede `CONFIG.items = true`"
    Un export per ogni variante creatura, creati automaticamente all'avvio. Usati internamente da `ox_inventory` quando il giocatore usa l'item, ma possono essere chiamati anche da altri script.

Il formato è: **`nomeCreatura_numeroVariante`**

```lua
exports['avalon_mounts']:necrodragon_1()   -- Necrodragon variante 1
exports['avalon_mounts']:necrodragon_2()   -- Necrodragon variante 2
exports['avalon_mounts']:necrodragon_3()
exports['avalon_mounts']:necrodragon_4()
exports['avalon_mounts']:necrodragon_5()

exports['avalon_mounts']:griffon_1()
exports['avalon_mounts']:griffon_2()
exports['avalon_mounts']:griffon_3()

exports['avalon_mounts']:pegaso_1()
exports['avalon_mounts']:pegaso_2()
exports['avalon_mounts']:pegaso_3()

exports['avalon_mounts']:horse_1()
exports['avalon_mounts']:horse_2()
exports['avalon_mounts']:horse_3()

exports['avalon_mounts']:alpaca_1()
exports['avalon_mounts']:alpaca_2()
exports['avalon_mounts']:alpaca_3()
```

!!! tip "Configurazione item in ox_inventory"
    Ogni item mount va registrato in ox_inventory con il nome uguale al nome export (es. `necrodragon_1`). Lo script gestisce automaticamente l'azione di uso quando `CONFIG.items = true`.

---

## Note Integrazione

### ox_fuel
Se presente, il carburante della creatura viene mantenuto al massimo ogni **5 secondi** automaticamente. Nessuna configurazione richiesta.

### QBox (qbx_core + qbx_vehiclekeys)
Se presenti, le chiavi del veicolo creatura vengono assegnate automaticamente al giocatore ogni **10 secondi** mentre la creatura è attiva.
