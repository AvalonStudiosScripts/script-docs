# avalon_party

Sistema de **party estilo MMORPG** para FiveM. Grupos con HUD en tiempo real, barras de vida personalizables, ready check, waypoint compartido, espectador automático, blips y marcadores sobre los compañeros, marcación de objetivos e instancias privadas (routing buckets).

**Versión:** 1.0.0 | **Autor:** Avalon | **Framework:** ESX / QBCore / QBox / Standalone

---

## Dependencias

| Resource | Requerido |
|----------|:---------:|
| ox_lib | ✅ |
| ox_target | ✅ |

---

## Instalación

```
ensure ox_lib
ensure ox_target
ensure avalon_party
```

El framework se detecta solo: `es_extended` → ESX, `qbx_core` → QBox, `qb-core` → QBCore, si no standalone.

---

## Idiomas

Los textos están en `locales/<código>.lua`. Incluidos: **inglés, italiano, portugués, español, ruso, chino simplificado, tailandés**.

```lua
-- config.lua (dejar esta línea arriba del archivo)
Config.locale = 'es'   -- en | it | pt | es | ru | zh | th
```

Una clave que falte en una traducción cae automáticamente al inglés. Para añadir un idioma: copia `locales/en.lua`, renómbralo (ej. `de.lua`), traduce los valores, cambia `Locales['en']` por `Locales['de']` y pon `Config.locale = 'de'`.

---

## Comandos

| Comando | Argumento | Descripción |
|---------|-----------|-------------|
| `/party` | — | Abrir el menú del party |
| `/partyinvite` | `[id jugador]` | Invitar a un jugador |
| `/partyaccept` | — | Aceptar invitación |
| `/partydecline` | — | Rechazar invitación |
| `/partyleave` | — | Salir del party |
| `/partydisband` | — | Disolver el party (líder) |
| `/partykick` | `[id jugador]` | Expulsar un miembro (líder) |
| `/partytransfer` | `[id jugador]` | Transferir liderazgo (líder) |
| `/partyready` | — | Iniciar ready check (líder) |
| `/partywp` | — | Compartir waypoint (líder) |

No hace falta tener party: la primera invitación crea el grupo. Apuntando a otro jugador con **ox_target** aparece la opción **"Invitar al Party"** (3 m).

---

## Teclas

| Tecla | Acción |
|-------|--------|
| **F5** | Abrir el menú del party |
| **F1** / **F2** | Aceptar / rechazar invitación |
| **F3** / **F4** | Listo / no listo (ready check) |
| **TAB** | Ciclar objetivos cercanos (se activa desde el menú) |
| **CTRL** (mantener) | Mira de marcación, solo líder — clic derecho sobre un ped abre los símbolos |
| **← →** | Cambiar jugador observado en modo espectador |

Los valores por defecto están en `Config.Keys`; cada jugador puede reasignarlas desde los ajustes de teclas de FiveM.

---

## Menú del party (F5)

Pantalla NUI con: lista de miembros (kick y transferencia para el líder), salir/disolver, ready check, compartir waypoint, toggle del objetivo TAB y de la marcación, visibilidad de cada barra y de los estados, visibilidad de blips y marcadores 3D, y escala del HUD de `0.5` a `3.0`.

!!! tip "Los ajustes se guardan"
    Las opciones son **por jugador** y se guardan en el `localStorage` de la NUI: sin tráfico de servidor ni base de datos.

---

## Configuración

| Opción | Default | Descripción |
|--------|---------|-------------|
| `Config.locale` | `'en'` | Idioma de los textos |
| `Config.maxPartySize` | `6` | Máximo de miembros |
| `Config.inviteExpiry` | `30000` | ms antes de que caduque una invitación |
| `Config.offlineGracePeriod` | `120000` | ms que un desconectado conserva su plaza |
| `Config.readyCheckTimeout` | `15000` | Duración del ready check en ms |

```lua
Config.Features = {
    distance   = true,  -- distancia en metros en las tarjetas
    readyCheck = true,  -- ready check
    waypoint   = true,  -- waypoint compartido
    spectator  = true,  -- espectador automático al morir
    targetMark = true,  -- marcación de objetivos con símbolos
}

Config.UI = {
    position       = 'top-left',  -- top-left | top-right | bottom-left | bottom-right
    pickClick      = 'right',     -- botón del ratón para elegir miembro
    hudWidth       = 260,
    scale          = 1.0,         -- 1.0 = automático según resolución
    showStatuses   = true,
    updateInterval = 500,         -- ms entre refrescos de los vitales
    criticalPct    = 20,          -- por debajo la barra parpadea en rojo
}
```

Con `scale = 1.0` el HUD se adapta solo: `1.5` desde 2560 px, `1.6` desde 3440 px, `2.0` desde 3840 px.

### Vida y estamina

Nunca pasan por el servidor: cada cliente las lee de los nativos de GTA para todos los miembros a la vista. Fuera del rango de streaming (~200 m) se conserva el último valor conocido y se marca como desactualizado.

```lua
Config.Health = {
    displayMode     = 'usable',  -- 'usable' | 'raw' | 'percent'
    syncMaxHealth   = true,      -- comparte el máximo de HP entre clientes (state bag)
    applyMaxToClone = true,      -- sube el techo de HP de los peds clonados
    autoDeadStatus  = true,      -- activa solo el estado 'dead' a vida cero
    deadStatusId    = 'dead',
}
```

| `displayMode` | Muestra |
|---------------|---------|
| `usable` | `87/100` hombre, `62/75` mujer — vida menos el umbral de muerte |
| `raw` | `187/200` y `162/175` — valores nativos |
| `percent` | `87/100` para todos |

### Elementos en el mundo y marcación

`Config.World` controla los blips en el mapa (`showBlips`, `blipSprite`, `blipColorMember`, `blipColorLeader`, `blipScale`) y el marcador sobre la cabeza (`showHeadMarker`, `headMarkerDist`, `headMarkerOffsetZ`, `headMarker`). Son los valores por defecto: cada jugador puede apagarlos desde el menú.

`Config.TargetMark` ajusta el alcance del TAB, los colores y el tamaño de los símbolos y el icono de la mira (`ctrlIcon`, cualquier clase de Font Awesome). Los símbolos disponibles se definen en `Config.TargetSymbols`: `warning` ⚠️, `target` 🎯, `attack` ❌, `defend` 🛡️, `heal` 💊, `focus` ⭐.

### Barras

Las barras con `builtin = true` se actualizan solas desde los nativos de GTA. Las de `builtin = false` permanecen **ocultas hasta que tu script envía un valor** con `SetMemberBar`.

| ID | Etiqueta | Builtin | Color |
|----|----------|:-------:|-------|
| `health` | HP | ✅ | Rojo |
| `stamina` | Estamina | ✅ | Verde |
| `mana` | Maná | ❌ | Azul |
| `shield` | Escudo | ❌ | Gris |
| `rage` | Furia | ❌ | Naranja |

```lua
-- config.lua, dentro de Config.Bars
{
    id = 'energy', label = 'Energía', color = '#f39c12', bgColor = '#3d2200',
    icon = '⚡', defaultMax = 100, builtin = false, visible = true,
},
```

!!! tip "Controlar la vida con tu sistema"
    Pon `builtin = false` en la barra `health` (o `stamina`): el recurso deja de escribirla y pasa a valer lo que envíes con `SetMemberBar`.

### Estados

`combat` ⚔️ · `poisoned` ☠️ · `buffed` ✨ · `dead` 💀 · `afk` 💤 — se encienden con `SetMemberStatus`, salvo `dead` que es automático mientras `Config.Health.autoDeadStatus = true`.

```lua
-- config.lua, dentro de Config.Statuses
{ id = 'stunned', label = 'Aturdido', icon = '💫', color = '#9b59b6', duration = 0 },
```

---

## Exports Disponibles

### Servidor

#### `IsInParty(source)` → `boolean`
```lua
if exports['avalon_party']:IsInParty(source) then
    -- lógica para jugadores en party
end
```

#### `GetPartyMembers(source)` → `table | nil`
```lua
local members = exports['avalon_party']:GetPartyMembers(source)
if members then
    for _, id in ipairs(members) do
        exports.ox_inventory:AddItem(id, 'pocion', 1)
    end
end
```

#### `GetPartyLeader(source)` → `number | nil`
```lua
local leader = exports['avalon_party']:GetPartyLeader(source)
```

#### `SetMemberBar(source, barId, value, maxValue)` → `boolean`
Actualiza una barra en el HUD visible para todo el party. Rechazado en barras con `builtin = true`.

```lua
-- Actualizar maná tras lanzar un hechizo
RegisterNetEvent('magia:lanzar')
AddEventHandler('magia:lanzar', function(coste)
    local src = source
    local nuevoMana = math.max(0, GetMana(src) - coste)
    exports['avalon_party']:SetMemberBar(src, 'mana', nuevoMana, 100)
end)
```

#### `SetMemberStatus(source, statusId, active)` → `boolean`
Mostrar/ocultar un icono de estado en el HUD.

```lua
exports['avalon_party']:SetMemberStatus(source, 'poisoned', true)
exports['avalon_party']:SetMemberStatus(source, 'poisoned', false)
```

#### `SetPartyBucket(source, bucket)` → `boolean`
Mueve todo el party a un routing bucket (instancia privada). El bucket queda asociado al party: quien entre después se mueve solo y quien salga vuelve al bucket 0. No hace falta ser líder.

```lua
RegisterNetEvent('mazmorra:entrar')
AddEventHandler('mazmorra:entrar', function(id)
    local src = source
    local ok = exports['avalon_party']:SetPartyBucket(src, 1000 + id)
    if not ok then
        TriggerClientEvent('ox_lib:notify', src, { type='error', description='¡Necesitas un party!' })
    end
end)
```

#### `GetPartyBucket(source)` → `number`
```lua
local bucket = exports['avalon_party']:GetPartyBucket(source)  -- 0 = mundo principal
```

---

### Cliente

#### `RequestMemberPick(mode, callbackEvent)`
Activa la selección de miembro: aparece el cursor sobre las tarjetas del party y el jugador hace clic en uno.

- `mode` `"self"` → el evento llega a quien selecciona · `"target"` → llega al miembro seleccionado
- El callback recibe `{ id, name, bars, statuses, isLeader, offline, picker }`

!!! warning "Sin callback al cancelar"
    Con **ESC** o `CancelMemberPick()` el evento **no** se dispara. El botón del ratón se define en `Config.UI.pickClick`.

```lua
RegisterCommand('curar_aliado', function()
    exports['avalon_party']:RequestMemberPick('self', 'curacion:objetivo')

    SetTimeout(10000, function()
        exports['avalon_party']:CancelMemberPick()
    end)
end)

RegisterNetEvent('curacion:objetivo')
AddEventHandler('curacion:objetivo', function(member)
    if member.offline then return end
    TriggerServerEvent('curacion:aplicar', member.id, 50)
end)
```

#### `CancelMemberPick()`
Cancela la selección activa sin elegir a nadie. No dispara ningún evento.

```lua
exports['avalon_party']:CancelMemberPick()
```

#### `GetPartyTarget()` → `{ ped, netId, source, isPlayer } | nil`
Devuelve el ped seleccionado con el ciclo **TAB**. `source` es `nil` si el ped es un NPC.

```lua
local target = exports['avalon_party']:GetPartyTarget()
if target and target.isPlayer then
    TriggerServerEvent('combate:atacar', target.source)
end
```

!!! tip "Archivo de referencia"
    El recurso incluye `example_integration.lua`: nunca se carga, es solo una colección de snippets listos para copiar.
