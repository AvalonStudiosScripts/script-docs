# avalon_party

Sistema **party in stile MMORPG** per FiveM. Permette ai giocatori di formare gruppi con HUD in tempo reale, barre vitali personalizzabili, ready check, waypoint condiviso, spettatore automatico, blip e marker sui compagni, marcatura bersagli con simboli e istanze private (routing bucket).

**Versione:** 1.0.0 | **Autore:** Avalon | **Framework:** ESX / QBCore / QBox / Standalone

---

## Dipendenze

| Resource | Obbligatoria |
|----------|:---:|
| ox_lib | ✅ |
| ox_target | ✅ |

---

## Installazione

1. Copia la cartella `avalon_party/` in `resources/`
2. Aggiungi la risorsa al `server.cfg` **dopo** `ox_lib` e `ox_target`

```
ensure ox_lib
ensure ox_target
ensure avalon_party
```

!!! info "Framework"
    Il rilevamento è automatico: `es_extended` → ESX, `qbx_core` → QBox, `qb-core` → QBCore, altrimenti standalone. I nomi personaggio vengono letti dal framework rilevato.

---

## Lingue

I testi stanno in `locales/<codice>.lua`, un file per lingua. In dotazione: **inglese, italiano, portoghese, spagnolo, russo, cinese semplificato, thailandese**.

```lua
-- config.lua (va lasciata in cima al file)
Config.locale = 'it'   -- en | it | pt | es | ru | zh | th
```

- Una chiave mancante in una traduzione ricade automaticamente sull'inglese
- Le traduzioni valgono sia per le notifiche di gioco che per i testi dentro l'HUD
- Per aggiungerne una: copia `locales/en.lua`, rinominalo (es. `de.lua`), traduci i valori, cambia `Locales['en']` in `Locales['de']` e imposta `Config.locale = 'de'`

!!! tip "Label personalizzate"
    Ogni `label` in `Config.Bars`, `Config.Statuses` e `Config.TargetSymbols` può essere una chiave `Locale.*` (tradotta) oppure una stringa scritta a mano.

---

## Comandi

| Comando | Argomento | Descrizione |
|---------|-----------|-------------|
| `/party` | — | Apre il menu gestione party |
| `/partyinvite` | `[id giocatore]` | Invita un giocatore nel party |
| `/partyaccept` | — | Accetta l'invito ricevuto |
| `/partydecline` | — | Rifiuta l'invito ricevuto |
| `/partyleave` | — | Lascia il party |
| `/partydisband` | — | Scioglie il party (solo leader) |
| `/partykick` | `[id giocatore]` | Espelle un membro (solo leader) |
| `/partytransfer` | `[id giocatore]` | Trasferisce la leadership (solo leader) |
| `/partyready` | — | Avvia il ready check (solo leader) |
| `/partywp` | — | Condivide il waypoint attuale (solo leader) |

!!! info "Chiunque può invitare"
    Non serve essere già in un party: il primo invito crea il gruppo. I permessi (leader, membro, party esistente) sono comunque validati lato server.

---

## Tasti

| Tasto | Azione |
|-------|--------|
| **F5** | Apre il menu gestione party |
| **F1** | Accetta invito in arrivo |
| **F2** | Rifiuta invito in arrivo |
| **F3** | Conferma "Pronto" (ready check) |
| **F4** | Conferma "Non pronto" (ready check) |
| **TAB** | Cicla i bersagli vicini (va attivato dal menu) |
| **CTRL** (tenuto) | Mirino di marcatura, solo leader — click destro su un ped apre la scelta del simbolo |
| **← →** | Cambia giocatore osservato in modalità spettatore |

!!! info "Cambia i tasti"
    I tasti di default sono in `config.lua` sotto `Config.Keys`. Essendo registrati con `lib.addKeybind`, ogni giocatore può rimapparli dalle impostazioni tasti di FiveM (`Impostazioni → Comandi → FiveM`).

---

## Interazione con ox_target

Puntando un altro giocatore compare l'opzione **"Invita nel Party"** (raggio 3 m). È il modo più rapido di invitare senza conoscere l'ID server.

---

## Menu party (F5)

Il menu è una schermata NUI e contiene:

- **Lista membri** con kick e trasferimento leadership (solo leader)
- **Lascia party** / **Sciogli party** (solo leader)
- **Avvia ready check** e **Condividi waypoint** (solo leader)
- **Toggle bersaglio TAB** e **toggle marcatura simboli**
- **Visibilità HUD**: ogni barra e il blocco stati si accendono/spengono singolarmente
- **Visibilità elementi 3D**: blip sulla mappa e marker sopra la testa
- **Scala HUD** regolabile da `0.5` a `3.0`

!!! tip "Le impostazioni restano"
    Le scelte del menu sono **per giocatore** e vengono salvate nel `localStorage` della NUI: zero traffico server e nessuna tabella database. Restano anche dopo un riavvio del gioco.

---

## Configurazione

### Impostazioni Generali

| Opzione | Default | Descrizione |
|---------|---------|-------------|
| `Config.locale` | `'en'` | Lingua dei testi (`en` `it` `pt` `es` `ru` `zh` `th`) |
| `Config.maxPartySize` | `6` | Numero massimo di membri per party |
| `Config.inviteExpiry` | `30000` | ms prima che un invito scada |
| `Config.offlineGracePeriod` | `120000` | ms in cui un membro disconnesso mantiene il suo posto |
| `Config.readyCheckTimeout` | `15000` | ms di durata del ready check |

!!! info "Riconnessione"
    Entro il `offlineGracePeriod` il giocatore che rientra torna nel suo party. Se era leader, la leadership passa subito a un altro membro ma gli viene restituita al rientro.

### Feature (Attiva/Disattiva)

```lua
Config.Features = {
    distance   = true,  -- distanza in metri sui member card
    readyCheck = true,  -- sistema ready check
    waypoint   = true,  -- waypoint condiviso dal leader
    spectator  = true,  -- spettatore automatico alla morte
    targetMark = true,  -- marcatura bersagli con simboli
}
```

Disattivare una feature ne rimuove anche i comandi, i tasti e le voci di menu collegate.

### HUD

```lua
Config.UI = {
    position       = 'top-left',  -- 'top-left' | 'top-right' | 'bottom-left' | 'bottom-right'
    pickClick      = 'right',     -- tasto mouse per selezionare un membro: 'right' | 'left'
    offsetX        = 20,
    offsetY        = 20,
    hudWidth       = 260,         -- larghezza card in px, prima della scala
    scale          = 1.0,         -- 1.0 = automatico (vedi sotto)
    showStatuses   = true,        -- mostra il blocco icone di stato
    updateInterval = 500,         -- ms tra gli aggiornamenti dei valori vitali
    criticalPct    = 20,          -- sotto questa percentuale la barra pulsa in rosso
}
```

!!! tip "Scala automatica"
    Lasciando `scale = 1.0` l'HUD si adatta da solo alla risoluzione: `1.5` da 2560 px, `1.6` da 3440 px (ultrawide), `2.0` da 3840 px (4K). Qualunque altro valore viene usato così com'è. Il giocatore può comunque sovrascriverlo dal menu F5.

### Vita e stamina

Vita e stamina non passano dal server: ogni client le legge dai nativi GTA per tutti i membri visibili. Fuori dal raggio di streaming (~200 m) i valori restano l'ultimo noto e vengono marcati come non aggiornati.

```lua
Config.Health = {
    displayMode     = 'usable',  -- 'usable' | 'raw' | 'percent'
    syncMaxHealth   = true,      -- condivide il massimale HP tra i client (statebag)
    applyMaxToClone = true,      -- alza il tetto HP dei ped clonati sugli altri client
    autoDeadStatus  = true,      -- accende da solo lo stato 'dead' a vita zero
    deadStatusId    = 'dead',    -- id dello status usato per la morte
}
```

| `displayMode` | Cosa mostra |
|---------------|-------------|
| `usable` | `87/100` uomo, `62/75` donna — vita meno la soglia di morte |
| `raw` | `187/200` e `162/175` — valori nativi, barra a metà alla morte |
| `percent` | `87/100` per tutti — stessa lunghezza barra per uomo e donna |

!!! warning "Massimali personalizzati"
    GTA non sincronizza il massimale HP. Con `syncMaxHealth = false` le donne appaiono al 75% sugli HUD altrui e i potenziamenti vita restano invisibili. Con `applyMaxToClone = false` un membro da 400 HP viene mostrato a 200 sullo schermo degli altri.

### Elementi nel mondo

```lua
Config.World = {
    showBlips       = true,
    blipSprite      = 1,      -- 1 = punto giocatore standard
    blipColorMember = 5,      -- giallo
    blipColorLeader = 28,     -- oro
    blipScale       = 0.75,

    showHeadMarker    = true,
    headMarkerDist    = 50.0,   -- distanza massima, unità GTA
    headMarkerOffsetZ = 1.5,

    headMarker = {
        text  = '🛡',
        scale = 0.30,
        r = 255, g = 220, b = 60, a = 230,
    },
}
```

I valori qui sono i **default**: ogni giocatore può poi spegnere blip e marker dal menu F5.

### Marcatura bersagli

Con `Config.Features.targetMark = true` il leader può segnare un ped con un simbolo visibile a tutto il party: tiene premuto **CTRL**, punta il ped e fa click destro per scegliere il simbolo. Il **TAB** invece cicla i ped vicini ed è disponibile a tutti i membri, una volta attivato dal menu.

```lua
Config.TargetMark = {
    range         = 25.0,    -- raggio di rilevamento TAB
    oxTargetRange = 25.0,    -- raggio ox_target per "Segna Bersaglio"

    tabMarker    = { r = 255, g = 130, b = 0, a = 200 },
    tabRingType  = 1,        -- 1 = cerchio piatto, 28 = sottile, 36 = checkpoint
    tabShowRing  = true,
    tabArrowType = 27,       -- 27 = freccia giù, 0 = freccia su
    tabShowArrow = true,

    ctrlIcon      = 'fa-solid fa-arrows-to-dot',  -- icona mirino (classe Font Awesome)
    symbolScale   = 0.70,    -- dimensione del simbolo sopra il ped
    symbolOffsetZ = 1.5,     -- altezza sopra il ped
    symbolMaxDist = 50.0,    -- oltre questa distanza il simbolo non viene disegnato
    arrowScale    = 0.65,
    ringScale     = { x = 1.0, y = 1.0, z = 0.15 },
}
```

#### Simboli disponibili

| ID | Label | Icona |
|----|-------|:-----:|
| `warning` | Attenzione | ⚠️ |
| `target` | Bersaglio | 🎯 |
| `attack` | Attacca | ❌ |
| `defend` | Difendi | 🛡️ |
| `heal` | Cura | 💊 |
| `focus` | Focus | ⭐ |

Puoi aggiungerne o rimuoverne liberamente in `Config.TargetSymbols`.

### Barre Vitali

Le barre con `builtin = true` si aggiornano automaticamente dai nativi GTA (zero traffico server). Le barre con `builtin = false` restano **nascoste finché il tuo script non manda un valore** con l'export `SetMemberBar`.

| ID | Label | Builtin | Colore | Visibile di default |
|----|-------|:-------:|--------|:---:|
| `health` | HP | ✅ | Rosso | ✅ |
| `stamina` | Stamina | ✅ | Verde | ✅ |
| `mana` | Mana | ❌ | Blu | ✅ |
| `shield` | Scudo | ❌ | Grigio | ✅ |
| `rage` | Furia | ❌ | Arancio | ✅ |

#### Aggiungere una barra

```lua
-- config.lua, dentro Config.Bars
{
    id         = 'energy',
    label      = 'Energia',     -- oppure una chiave Locale.*
    color      = '#f39c12',
    bgColor    = '#3d2200',
    icon       = '⚡',           -- emoji o path sotto ui/dist/img/ (es. 'img/energy.png')
    defaultMax = 100,
    builtin    = false,         -- IMPORTANTE: false per le barre gestite da script esterni
    visible    = true,          -- stato iniziale del toggle nel menu
},
```

!!! tip "Gestire vita o stamina con il tuo sistema"
    Metti `builtin = false` sulla barra `health` (o `stamina`): la risorsa smette di scriverci e il valore passa a essere quello che mandi tu con `SetMemberBar`. Utile con sistemi di ferite o scale HP personalizzate.

### Status

| ID | Label | Icona |
|----|-------|:-----:|
| `combat` | In combattimento | ⚔️ |
| `poisoned` | Avvelenato | ☠️ |
| `buffed` | Potenziato | ✨ |
| `dead` | Morto | 💀 |
| `afk` | AFK | 💤 |

Tutti gli stati si accendono e spengono con `SetMemberStatus`, tranne `dead` che è automatico finché `Config.Health.autoDeadStatus = true`.

#### Aggiungere uno stato

```lua
-- config.lua, dentro Config.Statuses
{ id = 'stunned', label = 'Stordito', icon = '💫', color = '#9b59b6', duration = 0 },
```

`duration` a `0` significa che lo stato resta finché non lo spegni tu.

---

## Export Disponibili

---

### Export Server-Side

#### `IsInParty(source)`

Controlla se un giocatore è attualmente in un party.

**Parametri:** `source` — ID server del giocatore
**Ritorna:** `boolean`

```lua
-- server/main.lua
local inParty = exports['avalon_party']:IsInParty(source)

if inParty then
    -- blocca accesso a una zona solo-player
    TriggerClientEvent('zone:denied', source, "Non puoi entrare in party")
end
```

---

#### `GetPartyMembers(source)`

Restituisce la lista degli ID server di tutti i membri del party del giocatore (leader incluso).

**Parametri:** `source` — qualsiasi membro del party
**Ritorna:** `table` di sources, oppure `nil` se non è in party

```lua
-- server/main.lua
local members = exports['avalon_party']:GetPartyMembers(source)

if members then
    print("Il party ha " .. #members .. " membri")
    for _, memberId in ipairs(members) do
        -- dai un item a tutti i membri del party
        exports.ox_inventory:AddItem(memberId, 'health_potion', 1)
    end
end
```

---

#### `GetPartyLeader(source)`

Restituisce l'ID server del leader del party.

**Parametri:** `source` — qualsiasi membro del party
**Ritorna:** `number` (source del leader), oppure `nil`

```lua
-- server/main.lua
local leader = exports['avalon_party']:GetPartyLeader(source)

if leader == source then
    print("Questo giocatore è il leader del suo party")
end
```

---

#### `SetMemberBar(source, barId, value, maxValue)`

Aggiorna il valore di una barra nel HUD del party per un giocatore specifico. Visibile a tutti i membri del suo party in tempo reale.

**Parametri:**
| Nome | Tipo | Descrizione |
|------|------|-------------|
| `source` | `number` | ID server del giocatore |
| `barId` | `string` | ID della barra (`"mana"`, `"shield"`, `"rage"`, o custom) |
| `value` | `number` | Valore attuale |
| `maxValue` | `number` | Valore massimo |

**Ritorna:** `boolean` — `true` se aggiornato, `false` se il giocatore non è in party o l'ID barra non è valido

!!! warning "Solo barre con `builtin = false`"
    La chiamata viene rifiutata sulle barre marcate `builtin = true`, gestite dalla risorsa stessa. Per prendere il controllo di `health` o `stamina` metti `builtin = false` su quella barra in `config.lua`.

```lua
-- server/main.lua — sistema mana
RegisterNetEvent('magic:castSpell')
AddEventHandler('magic:castSpell', function(manaCost)
    local src = source
    local currentMana = GetPlayerMana(src) -- funzione del tuo script
    local newMana = math.max(0, currentMana - manaCost)

    SetPlayerMana(src, newMana)

    -- aggiorna la barra nel HUD del party
    exports['avalon_party']:SetMemberBar(src, 'mana', newMana, 100)
end)
```

```lua
-- Esempio: shield che si riduce ai danni
RegisterNetEvent('combat:takeDamage')
AddEventHandler('combat:takeDamage', function(damage)
    local src = source
    local shield = GetPlayerShield(src)
    local newShield = math.max(0, shield - damage)

    exports['avalon_party']:SetMemberBar(src, 'shield', newShield, 100)
end)
```

---

#### `SetMemberStatus(source, statusId, active)`

Attiva o disattiva un'icona di stato sul member card HUD.

**Parametri:**
| Nome | Tipo | Descrizione |
|------|------|-------------|
| `source` | `number` | ID server del giocatore |
| `statusId` | `string` | ID dello status (`"combat"`, `"poisoned"`, `"buffed"`, `"dead"`, `"afk"`) |
| `active` | `boolean` | `true` per mostrarlo, `false` per nasconderlo |

**Ritorna:** `boolean`

```lua
-- server/main.lua

-- Mostra "In Combat" quando il giocatore spara
AddEventHandler('weaponDamageEvent', function(source)
    exports['avalon_party']:SetMemberStatus(source, 'combat', true)

    -- Rimuovi dopo 5 secondi
    SetTimeout(5000, function()
        exports['avalon_party']:SetMemberStatus(source, 'combat', false)
    end)
end)

-- Mostra "Avvelenato" quando il giocatore viene avvelenato
RegisterNetEvent('poison:apply')
AddEventHandler('poison:apply', function()
    exports['avalon_party']:SetMemberStatus(source, 'poisoned', true)
end)

RegisterNetEvent('poison:cure')
AddEventHandler('poison:cure', function()
    exports['avalon_party']:SetMemberStatus(source, 'poisoned', false)
end)
```

!!! info "Stato `dead`"
    Con `Config.Health.autoDeadStatus = true` (default) lo stato morte è gestito dalla risorsa. Mettilo a `false` se vuoi pilotarlo dal tuo script.

---

#### `SetPartyBucket(source, bucket)`

Sposta l'intero party di un giocatore in un routing bucket (dimensione separata). Utile per dungeon, istanze private, eventi.

**Parametri:**
| Nome | Tipo | Descrizione |
|------|------|-------------|
| `source` | `number` | Qualsiasi membro del party |
| `bucket` | `number` | ID del routing bucket di destinazione (0 = mondo principale) |

**Ritorna:** `boolean`

Il bucket resta associato al party: chi entra dopo viene spostato automaticamente, chi lascia il party torna al bucket 0. Non serve essere leader per chiamarlo.

```lua
-- server/main.lua — dungeon instanziato
RegisterNetEvent('dungeon:enter')
AddEventHandler('dungeon:enter', function(dungeonId)
    local src = source

    -- Genera un bucket univoco per questo dungeon
    local bucket = dungeonId + 1000

    local ok = exports['avalon_party']:SetPartyBucket(src, bucket)

    if ok then
        TriggerClientEvent('dungeon:teleport', src, dungeonId)
        print("Party spostato nel bucket " .. bucket)
    else
        TriggerClientEvent('ox_lib:notify', src, {
            type = 'error',
            description = 'Devi essere in un party per entrare nel dungeon!'
        })
    end
end)

-- Riporta al mondo principale all'uscita
RegisterNetEvent('dungeon:exit')
AddEventHandler('dungeon:exit', function()
    exports['avalon_party']:SetPartyBucket(source, 0)
end)
```

---

#### `GetPartyBucket(source)`

Ottiene il routing bucket attuale del party.

**Parametri:** `source` — qualsiasi membro del party
**Ritorna:** `number` (0 = mondo principale, anche se il giocatore non è in party)

```lua
local bucket = exports['avalon_party']:GetPartyBucket(source)
print("Il party è nel bucket: " .. bucket)
```

---

### Export Client-Side

#### `RequestMemberPick(mode, callbackEvent)`

Attiva la modalità selezione membro: compare il cursore sulle card del party e il giocatore clicca il membro che vuole selezionare. Utile per puntare cure, buff, scambi o abilità a un membro specifico.

**Parametri:**
| Nome | Tipo | Valori | Descrizione |
|------|------|--------|-------------|
| `mode` | `string` | `"self"` / `"target"` | Chi riceve l'evento: `"self"` chi ha selezionato · `"target"` il membro selezionato |
| `callbackEvent` | `string` | nome evento | Evento client (`RegisterNetEvent`) che riceve i dati |

**L'evento di callback riceve una tabella:**

| Campo | Tipo | Descrizione |
|-------|------|-------------|
| `id` | `number` | Server ID del membro selezionato |
| `name` | `string` | Nome del personaggio |
| `bars` | `table` | Barre lato server (mana, shield, …) — **non** vita e stamina |
| `statuses` | `table` | Stati attivi |
| `isLeader` | `boolean` | `true` se il membro selezionato è il leader |
| `offline` | `boolean` | `true` se è disconnesso ma ancora nel party |
| `picker` | `number` | Server ID di chi ha avviato la selezione |

!!! warning "Nessun callback all'annullamento"
    Se il giocatore preme **ESC** o chiami `CancelMemberPick()`, l'evento **non** viene emesso. Il tasto del mouse usato per il click si imposta con `Config.UI.pickClick`. Se il giocatore non è in un party la chiamata mostra una notifica di errore e non fa nulla.

```lua
-- client/main.lua del tuo script

-- Seleziona un alleato da curare, con rinuncia dopo 10 secondi
RegisterCommand('cure', function()
    exports['avalon_party']:RequestMemberPick('self', 'myScript:onMemberPicked')

    SetTimeout(10000, function()
        exports['avalon_party']:CancelMemberPick()  -- ignorato se ha già scelto
    end)
end)

RegisterNetEvent('myScript:onMemberPicked')
AddEventHandler('myScript:onMemberPicked', function(member)
    if member.offline then return end

    print("Hai selezionato: " .. member.name .. " (source: " .. member.id .. ")")
    TriggerServerEvent('healing:applyHeal', member.id, 50)
end)
```

```lua
-- mode 'target': l'evento arriva al membro selezionato, non a chi seleziona
RegisterCommand('giveitem', function()
    exports['avalon_party']:RequestMemberPick('target', 'myScript:itemOffered')
end)

RegisterNetEvent('myScript:itemOffered')
AddEventHandler('myScript:itemOffered', function(data)
    -- data.picker = chi ha avviato la selezione
    print(('Il giocatore %s vuole darti un oggetto'):format(data.picker))
end)
```

---

#### `CancelMemberPick()`

Cancella la modalità selezione membro attiva senza selezionare nessuno. Nessun evento viene emesso. La chiamata è ignorata se non c'è una selezione in corso.

```lua
exports['avalon_party']:CancelMemberPick()
```

---

#### `GetPartyTarget()`

Restituisce le informazioni sul ped attualmente selezionato con il ciclo **TAB**. Disponibile solo se `Config.Features.targetMark = true`.

**Ritorna:** `table` `{ ped, netId, source, isPlayer }` oppure `nil` se nessun target attivo

| Campo | Tipo | Descrizione |
|-------|------|-------------|
| `ped` | `number` | Handle dell'entità |
| `netId` | `number` | Network ID dell'entità |
| `source` | `number` | Server ID (se è un player, altrimenti `nil`) |
| `isPlayer` | `boolean` | `true` se il target è un altro giocatore |

```lua
-- client/main.lua del tuo script

-- Attacca il target attualmente selezionato
RegisterCommand('attack_target', function()
    local target = exports['avalon_party']:GetPartyTarget()

    if not target then
        lib.notify({ type = 'error', description = 'Nessun bersaglio selezionato' })
        return
    end

    if target.isPlayer then
        print("Target: giocatore " .. target.source)
        TriggerServerEvent('combat:attackPlayer', target.source)
    else
        print("Target: NPC con netId " .. target.netId)
        TriggerServerEvent('combat:attackNpc', target.netId)
    end
end)
```

```lua
-- Controlla continuamente il target e disegna un marker
CreateThread(function()
    while true do
        Wait(0)
        local target = exports['avalon_party']:GetPartyTarget()

        if target then
            local coords = GetEntityCoords(target.ped)
            DrawMarker(1, coords.x, coords.y, coords.z + 2.0,
                0, 0, 0, 0, 0, 0,
                0.3, 0.3, 0.3,
                255, 0, 0, 150,
                false, true, 2, false, nil, nil, false)
        end
    end
end)
```

---

## Esempio Completo — Script di Magia

Integrazione completa di `avalon_party` in uno script che gestisce mana, shield, status e dungeon istanziati.

```lua
-- server/main.lua dello script magia

local function updateBars(src, mana, shield)
    exports['avalon_party']:SetMemberBar(src, 'mana',   mana,   100)
    exports['avalon_party']:SetMemberBar(src, 'shield', shield, 100)
end

-- Inizializza i valori al login
AddEventHandler('esx:playerLoaded', function(source)
    updateBars(source, 100, 100)
end)

-- Cast di un incantesimo
RegisterNetEvent('magic:cast')
AddEventHandler('magic:cast', function(spellName)
    local src    = source
    local mana   = GetPlayerMana(src)
    local shield = GetPlayerShield(src)
    local cost   = SpellCosts[spellName] or 10

    if mana < cost then
        TriggerClientEvent('ox_lib:notify', src, { type='error', description='Mana insufficiente!' })
        return
    end

    mana = mana - cost
    SetPlayerMana(src, mana)
    updateBars(src, mana, shield)

    -- Mostra "Potenziato" agli alleati del party
    exports['avalon_party']:SetMemberStatus(src, 'buffed', true)
    SetTimeout(5000, function()
        exports['avalon_party']:SetMemberStatus(src, 'buffed', false)
    end)
end)

-- Dungeon: sposta l'intero party in un'istanza privata
RegisterNetEvent('dungeon:enter')
AddEventHandler('dungeon:enter', function(id)
    local src    = source
    local bucket = 1000 + id

    if not exports['avalon_party']:IsInParty(src) then
        TriggerClientEvent('ox_lib:notify', src, { type='error', description='Devi essere in party!' })
        return
    end

    if exports['avalon_party']:GetPartyBucket(src) ~= 0 then
        TriggerClientEvent('ox_lib:notify', src, { type='error', description='Il tuo party è già in un istanza!' })
        return
    end

    exports['avalon_party']:SetPartyBucket(src, bucket)

    for _, memberId in ipairs(exports['avalon_party']:GetPartyMembers(src)) do
        TriggerClientEvent('dungeon:teleport', memberId, id)
    end
end)

-- Ricompensa divisa tra i membri a fine dungeon
RegisterNetEvent('dungeon:completed')
AddEventHandler('dungeon:completed', function()
    local src     = source
    local members = exports['avalon_party']:GetPartyMembers(src)

    if not members then
        TriggerClientEvent('reward:give', src, 500)
        return
    end

    local share = math.floor(2000 / #members)
    for _, memberId in ipairs(members) do
        TriggerClientEvent('reward:give', memberId, share)
    end

    exports['avalon_party']:SetPartyBucket(src, 0)  -- ritorno al mondo pubblico
end)
```

!!! tip "File di riferimento"
    La risorsa include `example_integration.lua`: non viene mai caricato, serve solo come raccolta di snippet pronti da copiare nel tuo script.
