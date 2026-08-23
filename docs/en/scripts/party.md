# avalon_party

**MMORPG-style party system** for FiveM. Create groups with a real-time HUD, customizable vital bars, ready check, shared waypoint, automatic spectator mode, map blips and head markers, target marking with symbols, and private instances (routing buckets).

**Version:** 1.0.0 | **Author:** Avalon | **Framework:** ESX / QBCore / QBox / Standalone

---

## Dependencies

| Resource | Required |
|----------|:--------:|
| ox_lib | ✅ |
| ox_target | ✅ |

---

## Installation

1. Copy the `avalon_party/` folder into `resources/`
2. Add the resource to `server.cfg` **after** `ox_lib` and `ox_target`

```
ensure ox_lib
ensure ox_target
ensure avalon_party
```

!!! info "Framework"
    Detection is automatic: `es_extended` → ESX, `qbx_core` → QBox, `qb-core` → QBCore, otherwise standalone. Character names are read from the detected framework.

---

## Languages

Every string lives in `locales/<code>.lua`, one file per language. Shipped: **English, Italian, Portuguese, Spanish, Russian, Simplified Chinese, Thai**.

```lua
-- config.lua (keep this line at the top of the file)
Config.locale = 'en'   -- en | it | pt | es | ru | zh | th
```

- A missing key in a translation falls back to English automatically
- Translations cover both the game notifications and the text inside the HUD
- To add one: copy `locales/en.lua`, rename it (e.g. `de.lua`), translate the values, change `Locales['en']` to `Locales['de']` and set `Config.locale = 'de'`

!!! tip "Custom labels"
    Every `label` in `Config.Bars`, `Config.Statuses` and `Config.TargetSymbols` can be a `Locale.*` key (translated) or your own hardcoded string.

---

## Commands

| Command | Argument | Description |
|---------|----------|-------------|
| `/party` | — | Open the party management menu |
| `/partyinvite` | `[player id]` | Invite a player to the party |
| `/partyaccept` | — | Accept a pending invite |
| `/partydecline` | — | Decline a pending invite |
| `/partyleave` | — | Leave the party |
| `/partydisband` | — | Disband the party (leader only) |
| `/partykick` | `[player id]` | Kick a member (leader only) |
| `/partytransfer` | `[player id]` | Transfer leadership (leader only) |
| `/partyready` | — | Start a ready check (leader only) |
| `/partywp` | — | Share current waypoint (leader only) |

!!! info "Anyone can invite"
    You do not need a party first: the initial invite creates the group. Permissions (leader, member, existing party) are validated server-side anyway.

---

## Keybinds

| Key | Action |
|-----|--------|
| **F5** | Open party management menu |
| **F1** | Accept incoming invite |
| **F2** | Decline incoming invite |
| **F3** | Confirm "Ready" (ready check) |
| **F4** | Confirm "Not Ready" (ready check) |
| **TAB** | Cycle nearby targets (must be enabled in the menu) |
| **CTRL** (hold) | Marking crosshair, leader only — right click on a ped opens the symbol picker |
| **← →** | Switch spectated player while in spectator mode |

!!! info "Changing the keys"
    Defaults live in `config.lua` under `Config.Keys`. They are registered with `lib.addKeybind`, so every player can remap them from the FiveM key settings (`Settings → Key Bindings → FiveM`).

---

## ox_target interaction

Aiming at another player shows the **"Invite to Party"** option (3 m range). Fastest way to invite someone without knowing their server ID.

---

## Party menu (F5)

The menu is a NUI screen and offers:

- **Member list** with kick and leadership transfer (leader only)
- **Leave party** / **Disband party** (leader only)
- **Start ready check** and **Share waypoint** (leader only)
- **TAB targeting toggle** and **symbol marking toggle**
- **HUD visibility**: every bar and the status block can be toggled individually
- **World element visibility**: map blips and head markers
- **HUD scale** from `0.5` to `3.0`

!!! tip "Settings persist"
    Menu choices are **per player** and stored in the NUI `localStorage`: no server traffic and no database table. They survive a game restart.

---

## Configuration

### General

| Option | Default | Description |
|--------|---------|-------------|
| `Config.locale` | `'en'` | Language (`en` `it` `pt` `es` `ru` `zh` `th`) |
| `Config.maxPartySize` | `6` | Maximum members per party |
| `Config.inviteExpiry` | `30000` | ms before an invite expires |
| `Config.offlineGracePeriod` | `120000` | ms a disconnected member keeps his slot |
| `Config.readyCheckTimeout` | `15000` | Ready check duration in ms |

!!! info "Reconnecting"
    Within `offlineGracePeriod` a returning player is put back into his party. A leader is replaced right away, but gets the lead back on return.

### Features

```lua
Config.Features = {
    distance   = true,  -- show distance (meters) on member cards
    readyCheck = true,  -- ready check system
    waypoint   = true,  -- shared waypoint from leader
    spectator  = true,  -- auto-spectator on death
    targetMark = true,  -- target marking with symbols
}
```

Turning a feature off also removes its commands, keybinds and menu entries.

### HUD

```lua
Config.UI = {
    position       = 'top-left',  -- 'top-left' | 'top-right' | 'bottom-left' | 'bottom-right'
    pickClick      = 'right',     -- mouse button to pick a member: 'right' | 'left'
    offsetX        = 20,
    offsetY        = 20,
    hudWidth       = 260,         -- card width in px, before scaling
    scale          = 1.0,         -- 1.0 = automatic (see below)
    showStatuses   = true,        -- show the status icon block
    updateInterval = 500,         -- ms between vitals refreshes
    criticalPct    = 20,          -- below this percentage a bar pulses red
}
```

!!! tip "Automatic scaling"
    Leaving `scale = 1.0` lets the HUD size itself: `1.5` from 2560 px, `1.6` from 3440 px (ultrawide), `2.0` from 3840 px (4K). Any other value is used as-is. Players can still override it from the F5 menu.

### Health and stamina

Health and stamina never go through the server: every client reads them from the GTA natives for all streamed members. Out of streaming range (~200 m) the last known values are kept and flagged as stale.

```lua
Config.Health = {
    displayMode     = 'usable',  -- 'usable' | 'raw' | 'percent'
    syncMaxHealth   = true,      -- share max HP between clients (state bag)
    applyMaxToClone = true,      -- raise the HP ceiling of cloned peds on other clients
    autoDeadStatus  = true,      -- turn the 'dead' status on automatically at zero health
    deadStatusId    = 'dead',    -- status id used for death
}
```

| `displayMode` | What it shows |
|---------------|---------------|
| `usable` | `87/100` male, `62/75` female — health minus the death threshold |
| `raw` | `187/200` and `162/175` — native values, half bar on death |
| `percent` | `87/100` for everybody — same bar length for male and female |

!!! warning "Custom maximums"
    GTA does not sync max health. With `syncMaxHealth = false` women show up at 75% on other HUDs and health upgrades stay invisible. With `applyMaxToClone = false` a 400 HP member is cut down to 200 on everybody else's screen.

### World elements

```lua
Config.World = {
    showBlips       = true,
    blipSprite      = 1,      -- 1 = standard player dot
    blipColorMember = 5,      -- yellow
    blipColorLeader = 28,     -- gold
    blipScale       = 0.75,

    showHeadMarker    = true,
    headMarkerDist    = 50.0,   -- max distance, GTA units
    headMarkerOffsetZ = 1.5,

    headMarker = {
        text  = '🛡',
        scale = 0.30,
        r = 255, g = 220, b = 60, a = 230,
    },
}
```

These are the **defaults**: each player can then turn blips and markers off from the F5 menu.

### Target marking

With `Config.Features.targetMark = true` the leader can put a symbol on a ped, visible to the whole party: hold **CTRL**, aim at the ped and right click to pick the symbol. **TAB** instead cycles nearby peds and is available to every member once enabled from the menu.

```lua
Config.TargetMark = {
    range         = 25.0,    -- TAB detection range
    oxTargetRange = 25.0,    -- ox_target range for "Mark Target"

    tabMarker    = { r = 255, g = 130, b = 0, a = 200 },
    tabRingType  = 1,        -- 1 = flat circle, 28 = thin, 36 = checkpoint
    tabShowRing  = true,
    tabArrowType = 27,       -- 27 = arrow down, 0 = arrow up
    tabShowArrow = true,

    ctrlIcon      = 'fa-solid fa-arrows-to-dot',  -- crosshair icon (any Font Awesome class)
    symbolScale   = 0.70,    -- symbol size above the ped
    symbolOffsetZ = 1.5,     -- height above the ped
    symbolMaxDist = 50.0,    -- past this the symbol is not drawn
    arrowScale    = 0.65,
    ringScale     = { x = 1.0, y = 1.0, z = 0.15 },
}
```

#### Available symbols

| ID | Label | Icon |
|----|-------|:----:|
| `warning` | Warning | ⚠️ |
| `target` | Target | 🎯 |
| `attack` | Attack | ❌ |
| `defend` | Defend | 🛡️ |
| `heal` | Heal | 💊 |
| `focus` | Focus | ⭐ |

Add or remove them freely in `Config.TargetSymbols`.

### Vital Bars

Bars with `builtin = true` update themselves from the GTA natives (zero server traffic). Bars with `builtin = false` stay **hidden until your script sends a value** through the `SetMemberBar` export.

| ID | Label | Builtin | Color | Visible by default |
|----|-------|:-------:|-------|:---:|
| `health` | HP | ✅ | Red | ✅ |
| `stamina` | Stamina | ✅ | Green | ✅ |
| `mana` | Mana | ❌ | Blue | ✅ |
| `shield` | Shield | ❌ | Grey | ✅ |
| `rage` | Rage | ❌ | Orange | ✅ |

#### Adding a bar

```lua
-- config.lua, inside Config.Bars
{
    id         = 'energy',
    label      = 'Energy',      -- or a Locale.* key
    color      = '#f39c12',
    bgColor    = '#3d2200',
    icon       = '⚡',           -- emoji or a path under ui/dist/img/ (e.g. 'img/energy.png')
    defaultMax = 100,
    builtin    = false,         -- IMPORTANT: false for bars driven by external scripts
    visible    = true,          -- initial state of the menu toggle
},
```

!!! tip "Driving health or stamina yourself"
    Set `builtin = false` on the `health` (or `stamina`) bar: the resource stops writing to it and the value becomes whatever you send with `SetMemberBar`. Handy with wound systems or custom HP scales.

### Statuses

| ID | Label | Icon |
|----|-------|:----:|
| `combat` | In Combat | ⚔️ |
| `poisoned` | Poisoned | ☠️ |
| `buffed` | Buffed | ✨ |
| `dead` | Dead | 💀 |
| `afk` | AFK | 💤 |

Every status is toggled with `SetMemberStatus`, except `dead` which is automatic as long as `Config.Health.autoDeadStatus = true`.

#### Adding a status

```lua
-- config.lua, inside Config.Statuses
{ id = 'stunned', label = 'Stunned', icon = '💫', color = '#9b59b6', duration = 0 },
```

`duration = 0` means the status stays on until you turn it off.

---

## Available Exports

### Server-Side

#### `IsInParty(source)` → `boolean`
Check if a player is currently in a party.

```lua
local inParty = exports['avalon_party']:IsInParty(source)

if inParty then
    TriggerClientEvent('zone:denied', source, "Can't enter solo zones while in a party")
end
```

---

#### `GetPartyMembers(source)` → `table | nil`
Returns a list of server IDs for all members of the player's party (leader included), or `nil` if he is not in one.

```lua
local members = exports['avalon_party']:GetPartyMembers(source)

if members then
    for _, memberId in ipairs(members) do
        -- give an item to every party member
        exports.ox_inventory:AddItem(memberId, 'health_potion', 1)
    end
end
```

---

#### `GetPartyLeader(source)` → `number | nil`
Returns the server ID of the party leader.

```lua
local leader = exports['avalon_party']:GetPartyLeader(source)

if leader == source then
    print("This player is the party leader")
end
```

---

#### `SetMemberBar(source, barId, value, maxValue)` → `boolean`
Update a bar in the HUD for a specific player, visible to all party members in real time. Returns `false` if the player is not in a party or the bar id is invalid.

!!! warning "Only bars with `builtin = false`"
    The call is rejected on bars flagged `builtin = true`, which the resource drives itself. To take over `health` or `stamina`, set `builtin = false` on that bar in `config.lua`.

```lua
-- Update mana bar after casting a spell
RegisterNetEvent('magic:castSpell')
AddEventHandler('magic:castSpell', function(manaCost)
    local src = source
    local newMana = math.max(0, GetPlayerMana(src) - manaCost)
    SetPlayerMana(src, newMana)

    exports['avalon_party']:SetMemberBar(src, 'mana', newMana, 100)
end)

-- Update shield on damage
RegisterNetEvent('combat:takeDamage')
AddEventHandler('combat:takeDamage', function(damage)
    local src = source
    local newShield = math.max(0, GetPlayerShield(src) - damage)
    exports['avalon_party']:SetMemberBar(src, 'shield', newShield, 100)
end)
```

---

#### `SetMemberStatus(source, statusId, active)` → `boolean`
Show or hide a status icon on a member's HUD card.

```lua
-- Show "In Combat" when player fires
AddEventHandler('weaponDamageEvent', function(source)
    exports['avalon_party']:SetMemberStatus(source, 'combat', true)
    SetTimeout(5000, function()
        exports['avalon_party']:SetMemberStatus(source, 'combat', false)
    end)
end)

-- Show "Poisoned"
RegisterNetEvent('poison:apply')
AddEventHandler('poison:apply', function()
    exports['avalon_party']:SetMemberStatus(source, 'poisoned', true)
end)

RegisterNetEvent('poison:cure')
AddEventHandler('poison:cure', function()
    exports['avalon_party']:SetMemberStatus(source, 'poisoned', false)
end)
```

!!! info "The `dead` status"
    With `Config.Health.autoDeadStatus = true` (default) death is handled by the resource. Set it to `false` to drive it from your own script.

---

#### `SetPartyBucket(source, bucket)` → `boolean`
Move an entire party into a routing bucket (private instance). Useful for dungeons and events. The bucket sticks to the party: members who join later are moved in automatically, members who leave go back to bucket 0. Leadership is not required.

```lua
RegisterNetEvent('dungeon:enter')
AddEventHandler('dungeon:enter', function(dungeonId)
    local src = source
    local bucket = 1000 + dungeonId

    local ok = exports['avalon_party']:SetPartyBucket(src, bucket)
    if ok then
        TriggerClientEvent('dungeon:teleport', src, dungeonId)
    else
        TriggerClientEvent('ox_lib:notify', src, {
            type = 'error', description = 'You need a party to enter this dungeon!'
        })
    end
end)

-- Return to main world on exit
RegisterNetEvent('dungeon:exit')
AddEventHandler('dungeon:exit', function()
    exports['avalon_party']:SetPartyBucket(source, 0)
end)
```

---

#### `GetPartyBucket(source)` → `number`
Get the current routing bucket of the party (0 = main world, also when the player is not in a party).

```lua
local bucket = exports['avalon_party']:GetPartyBucket(source)
print("Party is in bucket: " .. bucket)
```

---

### Client-Side

#### `RequestMemberPick(mode, callbackEvent)`
Activates member selection mode: the cursor appears over the party cards and the player clicks the member he wants. Useful for targeting heals, buffs, trades or abilities at a specific member.

- `mode`: `"self"` → the event fires on the player who picked · `"target"` → the event fires on the member who was picked
- `callbackEvent`: client event name (`RegisterNetEvent`) that receives the data

**The callback receives a table:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | `number` | Server ID of the picked member |
| `name` | `string` | Character name |
| `bars` | `table` | Server-side bars (mana, shield, …) — **not** health and stamina |
| `statuses` | `table` | Active statuses |
| `isLeader` | `boolean` | `true` if the picked member is the leader |
| `offline` | `boolean` | `true` if he is disconnected but still in the party |
| `picker` | `number` | Server ID of the player who started the pick |

!!! warning "No callback on cancel"
    If the player presses **ESC** or you call `CancelMemberPick()`, the event is **not** fired. The mouse button used to click is set by `Config.UI.pickClick`. If the player is not in a party the call shows an error notification and does nothing.

```lua
-- Pick a member to heal, give up after 10 seconds
RegisterCommand('heal_ally', function()
    exports['avalon_party']:RequestMemberPick('self', 'healing:onTargetPicked')

    SetTimeout(10000, function()
        exports['avalon_party']:CancelMemberPick()  -- no-op if he already picked
    end)
end)

RegisterNetEvent('healing:onTargetPicked')
AddEventHandler('healing:onTargetPicked', function(member)
    if member.offline then return end
    TriggerServerEvent('healing:applyHeal', member.id, 50)
end)
```

```lua
-- mode 'target': the event fires on the picked member, not on the picker
RegisterCommand('giveitem', function()
    exports['avalon_party']:RequestMemberPick('target', 'myScript:itemOffered')
end)

RegisterNetEvent('myScript:itemOffered')
AddEventHandler('myScript:itemOffered', function(data)
    -- data.picker = the source of the player who started the pick
    print(('%s wants to give you an item'):format(data.picker))
end)
```

---

#### `CancelMemberPick()`
Cancel the active member selection without picking anybody. No event is fired. The call is ignored if no pick is running.

```lua
exports['avalon_party']:CancelMemberPick()
```

---

#### `GetPartyTarget()` → `{ ped, netId, source, isPlayer } | nil`
Returns info about the ped currently selected through the **TAB** cycle. Available only with `Config.Features.targetMark = true`. `source` is `nil` when the ped is an NPC.

```lua
RegisterCommand('attack_target', function()
    local target = exports['avalon_party']:GetPartyTarget()
    if not target then
        lib.notify({ type='error', description='No target selected' })
        return
    end

    if target.isPlayer then
        TriggerServerEvent('combat:attackPlayer', target.source)
    else
        TriggerServerEvent('combat:attackNpc', target.netId)
    end
end)
```

!!! tip "Reference file"
    The resource ships with `example_integration.lua`. It is never loaded — it is a collection of ready-made snippets to copy into your own script.
