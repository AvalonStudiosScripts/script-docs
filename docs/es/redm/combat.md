# avalon_combat

Sistema de **combate cuerpo a cuerpo** para RedM. Cada estilo (licántropo, vampiro, katana, espada y escudo, pugilista, magos...) usa las animaciones de los paquetes Avalon. Los golpes los calcula el script: en cada frame sigue manos, pies y hojas a lo largo de la animación y golpea a los peds que atraviesan. El daño pasa siempre por el servidor.

Incluye: combos ligeros y pesados, ataques emparejados (dos peds animados a la vez), parada y contraataque, armas y escudos empuñados, carrera, esquiva, salto y vuelo para las criaturas, NPC que combaten solos y exports para conectar niveles, habilidades, barras de vida y magias.

**Versión:** beta | **Autor:** Avalon | **Juego:** RedM | **Framework:** standalone

---

## Dependencias

| Resource | Obligatoria | Para qué sirve |
|----------|:---:|---|
| OneSync | ✅ | NPC combatientes y parada (state bag) |
| `avalon_weapons_rdr2` | ✅ | Los modelos de las armas empuñadas |
| `avalon_shields_tint` | ✅ | Los modelos de los escudos |
| Paquetes de animación `avalon_anims_*` | ✅ al menos uno | Las animaciones y los estilos (ver [Estilos](#estilos)) |

No se requiere ningún framework: el script trabaja sobre los peds y los jugadores. La conexión con inventarios, niveles o barras de vida se hace con los [exports](#exportaciones-disponibles).

---

## Instalación

1. Copia la carpeta `avalon_combat/` en `resources/`
2. Inicia las armas, los escudos y los paquetes de animación, luego el recurso:

```
ensure avalon_weapons_rdr2
ensure avalon_shields_tint
ensure avalon_anims_werewolf
ensure avalon_anims_vampire
ensure avalon_anims_ka_02
# ... los demás paquetes que uses
ensure avalon_combat
```

!!! info "Orden de los paquetes"
    Un paquete iniciado después de `avalon_combat` (o reiniciado) se lee igualmente: sus estilos se añaden en cuanto arranca.

---

## Estilos

Los estilos provienen de los **paquetes de animación**. Cada paquete aporta un archivo `avalon_combat.lua` con sus propios estilos y lo declara en su `fxmanifest.lua`. `avalon_combat` los recoge por sí solo mediante el export `CombatStyles` del paquete.

| Estilo | Paquete | Arma | Guardia | Extra |
|--------|---------|------|---------|-------|
| `werewolf` | `avalon_anims_werewolf` | garras | del juego (manos desnudas) | carrera, esquiva, salto, ataques emparejados |
| `vampire` | `avalon_anims_vampire` | garras | propia | vuelo, boost, ataques emparejados |
| `succube` | `avalon_anims_succube` | garras | del juego (manos desnudas) | vuelo, boost, ataques emparejados |
| `katana` `katana1` `katana3` `katana4` | `avalon_anims_ka_02` `_01` `_03` `_04` | `xur_katana` | del juego (hoja) | |
| `sword` `sword2` `sword3` | `avalon_anims_sw_01` `_02` `_03` | `xur_machete2` | del juego (hoja) | |
| `sword_shield` `sword_shield2` `sword_shield3` | `avalon_anims_sns_01` `_02` `_03` | `swn2craft` + escudo | propia / propia / del juego | |
| `twinsword` | `avalon_anims_twinsword01` | `xur_katana2` en ambas manos | del juego (hoja) | |
| `hammer2h` | `avalon_anims_hammer2h` | `hammermoore` | del juego (hoja) | |
| `orientalmonk` | `avalon_anims_orientalmonk` | `giga` | del juego (hoja) | |
| `pugilist` | `avalon_anims_p_01` | manos desnudas | del juego (manos desnudas) | |
| `firemage` `mage` `witch` `wizard` | `avalon_anims_firemage_01` `_mage_01` `_witch_01` `_wizard` | manos / bastón | del juego (manos desnudas) | |

!!! warning "Magias"
    Los estilos de mago por ahora golpean con las manos, con un alcance mayor (como lo haría el hechizo). Los bastones son solo visuales: el punto desde el que saldrán las magias se lee con el export client `GetCastPoint()`.

Para escribir tu propio estilo mira [Crear un estilo](#crear-un-estilo).

---

## Comandos y teclas

!!! warning "Solo con el debug activo"
    Todos los comandos de chat existen solo con `AvalonCombat.Debug = true` en `config/config.lua`: sirven para configurar y probar estilos, armas y NPC. En un servidor en producción deja `Debug = false` y pon a los jugadores en combate desde tu script con el export de servidor `SetStyle`. Las teclas funcionan siempre.

### Modo combate

| Comando | Descripción |
|---------|-------------|
| `/avalon_combat <estilo>` | Entra en combate con ese estilo (sin argumento lista los estilos) |
| `/avalon_combat off` | Sale del combate |

| Tecla | Acción |
|-------|--------|
| **Clic izquierdo** | Combo ligero (pulsa de nuevo durante el ataque para el siguiente golpe) |
| **Clic derecho** | Combo pesado |
| **R** | Ataque emparejado sobre el ped de enfrente (los estilos que lo tienen) o combo especial |
| **CTRL** (mantenido) | Guardia |
| **E** | Fija al ped apuntado con la cámara (rombo rojo sobre la cabeza): los ataques y los ataques emparejados van hacia él. E de nuevo lo suelta |
| **Shift** | Carrera (licántropo) · vuelo (estilos voladores, en el aire) |
| **Alt** | Esquiva (licántropo) · boost (estilos voladores) |
| **Shift + Espacio** | Salto (licántropo) · despegue y aterrizaje (estilos voladores) |

!!! info "Quién reacciona a los golpes"
    Los golpes ligeros y pesados solo hacen daño. El combo especial, los ataques emparejados y los ataques con `react = true` también hacen tambalear o caer a la víctima.

### Comandos de prueba y ajuste

| Comando | Descripción |
|---------|-------------|
| `/avalon_attack <id>` | Ejecuta un ataque del estilo actual (id o nombre del clip) |
| `/avalon_pair [nombre]` | Ejecuta un ataque emparejado sobre el ped de enfrente |
| `/avalon_pair_offset <nombre> <x> <y> <yaw>` | Mueve a la víctima de un ataque emparejado (solo en este cliente) |
| `/avalon_weapon <nombre\|off>` | Cambia el arma de la mano derecha |
| `/avalon_offhand <nombre\|off>` | Cambia el arma de la mano izquierda |
| `/avalon_shield <nombre\|off>` | Cambia el escudo |
| `/avalon_weapon_offset <x y z rx ry rz> [hueso]` | Ajusta la posición del arma e imprime la línea para copiar en el config (también `_offhand_offset`, `_shield_offset`) |
| `/avalon_sound <nombre>` | Reproduce un sonido del config |
| `/avalon_sound_game <set> <nombre> [bank]` | Prueba cualquier sonido del juego |
| `/avalon_npc <estilo> [team] [modelo] [escala]` | Crea un NPC combatiente 4 m delante |
| `/avalon_npc_clear` | Borra los NPC creados por ti |
| `/avalon_npc_info` | Estado de los NPC combatientes a menos de 30 m |
| `/avalon_dummy [modelo]` · `/avalon_dummy_clear` | Maniquíes quietos para golpear |
| `/avalon_hitbox` | Muestra las hitbox: cuerpos (verde), miembros (amarillo), hojas (azul claro), golpes (rojo) |
| `/avalon_combat_debug` | Dibuja los movimientos de las manos e imprime cada golpe |

---

## Cómo funciona el daño

1. El cliente de quien ataca comprueba en cada frame miembros y hojas a lo largo de la animación. Un miembro más lento que `MinSweepSpeed` no golpea (carga y retorno del golpe).
2. Cuando un miembro o una hoja toca la hitbox de un ped, el cliente se lo comunica al servidor.
3. El servidor verifica el golpe (distancia, daño máximo, ataque desbloqueado), calcula daño y guardia y ejecuta los *hit handlers* de los demás recursos.
4. Según `AvalonCombat.DamageMode`:
    - `'native'` — el daño va a la vida real del ped
    - `'external'` — sin daño: el servidor emite el evento `avalon_combat:hit` para tu sistema (barras de vida simuladas, resistencias)

    La víctima ejecuta la reacción igualmente.

**Fórmula:** daño base (el del arma si golpea la hoja, si no el `damage` del estilo) × `mult` del ataque × multiplicador de progresión × multiplicador de salida, redondeado. Después se resta la defensa de la guardia.

---

## Configuración

Los archivos de configuración están en `config/`:

| Archivo | Contenido |
|---------|-----------|
| `config/config.lua` | Ajustes generales, combos, NPC, sonidos, daño, defensa, input |
| `config/guard.lua` | Las guardias del juego utilizables por nombre (`'blade'`, `'unarmed'`) |
| `config/weapons.lua` | Armas de la mano derecha |
| `config/offhand.lua` | Armas de la mano izquierda |
| `config/shields.lua` | Escudos (una entrada por color) |
| `config/styles/` | Estilos locales opcionales y `example.lua` con todos los campos explicados |

!!! info "Archivos editables"
    Los archivos de `config/` (y `config/styles/`) están abiertos: el resto del recurso está protegido con Cfx Escrow. Todo se personaliza desde ahí y con los [exports](#exportaciones-disponibles).

### Ajustes generales

| Opción | Por defecto | Descripción |
|--------|-------------|-------------|
| `Debug` | `false` | Activa los comandos de chat y `/avalon_combat_debug` (solo para pruebas) |
| `TargetRange` | `4.0` | m: los peds más lejanos no se comprueban |
| `LockRange` / `LockCone` | `3.0` / `45` | Un ataque se orienta hacia el ped más cercano dentro de estos metros y grados |
| `LockOnRange` | `12.0` | m: radio de la fijación con E |
| `MinSweepSpeed` | `2.0` | m/s: un miembro más lento no golpea |
| `SkipStart` | `0.10` | Parte inicial de cada ataque ignorada |
| `Rehit` | `0.45` | s antes de que el mismo ataque pueda volver a golpear al mismo objetivo |
| `MaxHitDamage` | `200` | El servidor rechaza golpes por encima de este daño |
| `MaxHitDistance` | `6.0` | El servidor rechaza golpes a jugadores más lejos que esto |
| `PairRange` | `3.5` | m: distancia máxima de agarre de un ataque emparejado |
| `HitboxScale` / `HitboxPad` | `1.0` / `0.0` | Agranda las hitbox de las hojas (para golpear con más facilidad) |
| `SoundVolume` | `0.35` | Volumen de los archivos de audio |
| `DamageMode` | `'native'` | `'native'` o `'external'` (ver arriba) |

### Combo

```lua
AvalonCombat.ComboWindow = 0.45 -- desde esta fase del ataque, una pulsación pone en cola el siguiente golpe
AvalonCombat.ComboCancel = 1.0  -- fase en la que sale el golpe en cola (1.0 = al final de la animación)
AvalonCombat.ComboReset = 0.8   -- s: pasada esta pausa el combo vuelve al primer golpe
```

### Modo de input

```lua
AvalonCombat.InputMode = 'combo' -- 'combo' | 'keys'
AvalonCombat.Keys = { SimulWindow = 0.08, SequenceWindow = 0.6, DirAbove = 0.5 }
```

- `'combo'`: cada tecla ejecuta el siguiente golpe de su cadena (`light`, `heavy`, `special`)
- `'keys'`: como en un juego de lucha, las teclas ejecutan los `moves` del estilo. Se combinan teclas pulsadas a la vez (`'light+heavy'`), una dirección (`'F+heavy'`: adelante + pesado) o secuencias (`{ 'light', 'light', 'heavy' }`)

Cada jugador puede tener uno distinto con el export `SetInputMode`.

### Guardia y parada

```lua
AvalonCombat.Defence = {
    Mode = 'always',      -- 'always' | 'timed' | 'stamina' | 'points'
    ParryWindow = 0.25,   -- s: guardia levantada dentro de este tiempo desde el golpe = parada
    StaminaCost = 15.0,   -- 'stamina': stamina restada por golpe parado
    Points = 100, PointsRegen = 10, PointsCost = 20, -- 'points': puntos de guardia
    BreakTime = 1.5,      -- s sin guardia después de que se rompa
    Block = { percent = 0.75, flat = 0 }, -- defensa base de la guardia
}
```

| `Mode` | Cuándo la guardia detiene un golpe |
|--------|------------------------------------|
| `always` | Siempre, daño reducido por la defensa |
| `timed` | Solo dentro de `ParryWindow` desde que se levanta, si no el golpe entra completo |
| `stamina` | Siempre, cada golpe cuesta stamina; al acabarse la stamina la guardia se rompe |
| `points` | Siempre, cada golpe cuesta puntos de guardia (exports `GetGuard` / `SetGuard`); a cero se rompe |

Una **parada** anula el daño y hace tambalear a quien ataca. **Defensa** = `daño × (1 - percent) - flat`, nunca por debajo de cero. Un escudo empuñado sustituye la defensa de la guardia por la suya; la dada con el export `SetDefence` se suma.

### Sonidos

Cada sonido tiene un nombre en `AvalonCombat.Sounds`. Estilos, ataques, armas y ataques emparejados usan esos nombres: un nombre, una lista de la que elegir uno al azar, o `{ light = ..., heavy = ..., special = ... }` por cadena.

```lua
AvalonCombat.Sounds = {
    punch = { file = melee('punch', 11) },                 -- archivo en sounds/ (.wav .ogg .mp3)
    my_roar = { file = 'roar.ogg', volume = 0.8, range = 40 },
    beast_roar = { set = 'LSHNT_sounds', name = 'ROAR' },  -- sonido del juego
}
```

Los sonidos cuerpo a cuerpo incluidos son los del juego, extraídos en `sounds/melee_*.wav`.

!!! tip "Sonido en el momento justo"
    `soundAt` hace que el sonido empiece en una fase precisa de la animación: `0.35` = al 35% del clip. Vale para los ataques, para los ataques emparejados y para los movimientos del estilo (`soundAt = { leap = 0.27, dodge = 0.15 }`). Sin `soundAt` el sonido empieza en el primer movimiento rápido (ataques) o de inmediato.

### Hitbox del cuerpo

`AvalonCombat.BodyHitbox` enumera las cápsulas sobre las que se golpea a cada ped (`{ hueso desde, hueso hasta, radio }`): cabeza, tórax, abdomen, brazos y piernas. Un animal sin esos huesos se convierte en una esfera de 0.4 m. `/avalon_hitbox` las muestra en el juego.

---

## Armas y escudos

Cada entrada de `config/weapons.lua`, `config/offhand.lua` y `config/shields.lua` es un modelo empuñado:

```lua
AvalonCombat.Weapons.xur_katana = {
    model = 'weapon_xur_katana',
    bone = 'SKEL_R_Hand',                         -- hueso al que está unida
    offset = vector3(0.075, 0.00, -0.03),         -- m
    rot = vector3(-90.0, 0.0, 0.0),               -- grados
    damage = 25,                                  -- por golpe de hoja, × mult del ataque
    sounds = { swing = 'blade_swipe', hit = { 'machete_slash', 'blade_flesh' } },
    blade = { from = vector3(0, 0, 0.12), to = vector3(0, 0, 1.05), radius = 0.08 }, -- hitbox de la hoja
}
```

| Campo | Descripción |
|-------|-------------|
| `model` | Arquetipo del modelo |
| `tint` | Opcional: índice del color (los escudos tienen una entrada por color) |
| `bone`, `offset`, `rot` | Dónde se sostiene en la mano. Se ajusta en el juego con `/avalon_weapon_offset`, que imprime la línea para copiar |
| `damage` | Daño por golpe de hoja |
| `sounds` | `swing` y `hit` de la hoja |
| `blade` | Cápsula de la hoja en el espacio del modelo: golpea en lugar de la mano |
| `cast` | Solo bastones: el punto desde el que saldrán las magias |
| `defence` | Solo escudos: `{ percent, flat }` en lugar de la de la guardia |

---

## Crear un estilo

Un estilo se puede escribir en dos lugares:

=== "En el paquete de animación"

    La forma usada por los paquetes Avalon. Junto al `fxmanifest.lua` del paquete pon `avalon_combat.lua`:

    ```lua
    local Styles = {}

    Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
        block = 'blade',   -- un paquete nombra la guardia del juego como cadena
    }

    exports('CombatStyles', function() return Styles end)
    ```

    y en el `fxmanifest.lua` del paquete:

    ```lua
    avalon_combat_styles 'yes'
    shared_script 'avalon_combat.lua'
    ```

=== "En config/styles"

    Un archivo `config/styles/mystyle.lua` dentro de `avalon_combat`:

    ```lua
    AvalonCombat.Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
    }
    ```

    Un estilo escrito aquí **gana** sobre el de un paquete con el mismo nombre: para modificar un estilo de un paquete, cópialo aquí.

`config/styles/example.lua` contiene un estilo completo, comentado campo por campo.

### Campos de un estilo

| Campo | Descripción |
|-------|-------------|
| `dict` | Diccionario de las animaciones (`.ycd`) |
| `damage` | Daño de un golpe del cuerpo (puñetazos, patadas, garras) |
| `idle` | Animación en bucle en reposo |
| `weapon` / `offhand` / `shield` | Qué empuña (claves de `Weapons`, `Offhand`, `Shields`) |
| `hands` | Los miembros que golpean: `{ hueso desde, hueso hasta, alcance }` |
| `reach` / `radius` | Alcance más allá del último hueso y grosor de los miembros (m) |
| `light` / `heavy` / `special` | Las cadenas del combo: listas de ataques |
| `moves` | Los movimientos del modo `'keys'` |
| `sounds` | `swing`, `hit`, `knockdown`, `block`, `leap`, `dodge`, `boost`, `float` |
| `soundAt` | Fase en la que salen los sonidos de los movimientos |
| `block` | Guardia: `{ start, loop, stop, hit }` con clips propios, o `'blade'` / `'unarmed'`. Sin `block` el estilo no para |
| `stagger` / `knockdown` | Clips de reacción a los golpes |
| `move` | Movimiento especial: `kind = 'runner'` (carrera, esquiva, salto) o `kind = 'flyer'` (vuelo, boost) |
| `pairs` | Ataques emparejados |

### Un ataque

```lua
light = {
    { clip = 'attack@slash1', mult = 1.0 },
    { clip = 'attack@slash2', mult = 1.25, knockdown = true, soundAt = 0.35 },
},
```

| Campo | Descripción |
|-------|-------------|
| `clip` | La animación |
| `mult` | Multiplicador del daño |
| `knockdown` | La víctima cae |
| `react` | La víctima reacciona aunque no sea un ataque especial |
| `sound` / `soundAt` | Sonido propio del ataque y fase en la que sale |
| `keepTurn` | El ped se queda orientado como lo deja la animación |
| `locked` | Bloqueado hasta que la progresión lo desbloquea (`SetAttackEnabled`) |
| `id` | Nombre del ataque para los exports (por defecto: el clip) |

### Ataque emparejado

```lua
pairs = {
    bite = { att = 'bite@att', vic = 'bite@vic', x = 0.5, y = 2.8, yaw = 196,
             mult = 4.0, damageAt = 0.45, sound = 'beast_roar', soundAt = 0.2 },
},
```

`att` / `vic` son los clips de quien ataca y de la víctima. `x`, `y` y `yaw` indican dónde está la víctima respecto a quien ataca (m, grados). `damageAt` es la fase en la que llega el daño. Se ajusta en el juego con `/avalon_pair_offset`.

!!! info "Movimiento de las animaciones"
    Los clips de los paquetes Avalon desplazan al ped según su propio movimiento (pasos, sprints, saltos). Los clips de paquetes de terceros se reproducen en el sitio.

---

## NPC combatientes

Un NPC combatiente es un ped de red que combate solo con un estilo: busca un objetivo, se acerca, ataca en combo, para, esquiva, salta o vuela.

- **Team:** los jugadores son `'player'`. Un NPC ataca a los jugadores (a menos que su team sea `'player'`) y a los NPC de otros teams, nunca a los peds normales.
- **Habilidades:** `AvalonCombat.Npc` en `config/config.lua` contiene los valores por defecto (radio de agresión, distancia de ataque, probabilidad de parada, esquiva, ataque emparejado...). Cada NPC puede cambiar algunos con `opts.skill`.

```lua
-- server
local net = exports.avalon_combat:SpawnFighter('werewolf', vector3(-300.0, 790.0, 118.0), 90.0, {
    team = 'bandits',
    health = 400,
    scale = 1.5,
    skill = { BlockChance = 0.6, Weights = { heavy = 0.6 } },
})

-- más tarde
exports.avalon_combat:StopFighter(net)
```

| `opts` | Descripción |
|--------|-------------|
| `model` | Modelo del ped (por defecto `AvalonCombat.Npc.Model`) |
| `weapon` / `offhand` / `shield` | Qué empuña (por defecto: los del estilo; `shield = false` = ninguno) |
| `team` | Team (por defecto `'npc'`) |
| `health` | Vida |
| `scale` | Tamaño del NPC (`1` = normal, de `0.2` a `5`). Con él crecen las hitbox, el alcance de los golpes, el movimiento de las animaciones y las distancias de combate |
| `skill` | Solo los valores de `AvalonCombat.Npc` que cambian |

!!! warning "Se necesita un jugador"
    `SpawnFighter` crea el ped en el cliente del jugador más cercano a las coordenadas: con el servidor vacío devuelve `nil`.

---

## Progresión (niveles y habilidades)

avalon_combat **no tiene** un sistema de niveles o habilidades: eso es de tu servidor. El script solo da las herramientas para decidir, para cada jugador o NPC combatiente, qué ataques puede usar y lo fuerte que es. Tu sistema llama a los exports, avalon_combat los aplica.

### Qué ataques puede usar

- Cada ataque está **disponible**, salvo que su entrada en el estilo tenga `locked = true`: entonces empieza bloqueado para todos.
- `SetAttackEnabled(target, estilo, attackId, true | false)` desbloquea o bloquea un ataque para ese combatiente y ese estilo. Prevalece sobre el `locked` del config.
- `attackId` es el `id` del ataque, si no el nombre de su clip. Para un ataque emparejado es su clave en `pairs`. `GetAttacks(estilo)` lista todos los ataques de un estilo con id, cadena y `locked`.
- Las cadenas del combo (`light`, `heavy`, `special`) y los `moves` del modo `'keys'` son entradas distintas, con id distintos: si tu servidor usa ambos modos, desbloquea ambos.
- Un ataque bloqueado se salta: en el combo el botón pasa al siguiente disponible de la cadena (ninguno disponible: el botón no hace nada); en el modo `'keys'` el movimiento no se elige; R no ejecuta el ataque emparejado. Los NPC solo usan sus ataques disponibles.
- El servidor descarta los golpes de un ataque bloqueado: un cliente modificado tampoco puede usarlo.

### Lo fuerte que es

| Export | Efecto |
|--------|--------|
| `SetAttackMultiplier(target, estilo, attackId, m)` | Multiplica el daño de un ataque |
| `SetOutgoingMultiplier(target, m)` | Multiplica todo su daño |
| `SetDefence(target, { percent, flat })` | Defensa propia, sumada a la de la guardia o el escudo |
| `SetMaxGuard(target, max)` | Puntos de guardia máximos (modo `'points'`) |
| `SetInputMode(target, 'combo' \| 'keys')` | Modo de entrada del combatiente |
| `SetStyle(src, estilo)` · `SetHeld(src, slot, clave)` | Estilo y armas del jugador (por ejemplo desde su clase o inventario) |

Cada `Set...` tiene su `Get...` para leer el valor actual.

!!! warning "No se guarda nada"
    La progresión vive en memoria: cuando el jugador sale (o un NPC se detiene) vuelve a los valores del config. Guardarla es tarea de tu sistema, que la vuelve a enviar en cada entrada, por ejemplo cuando se carga el personaje.

```lua
-- server: tu sistema de niveles
local Unlocks = { -- nivel mínimo por ataque, por estilo
    katana = { ['combo@a03@rm'] = 5, ['combo@a04@rm'] = 10, ['combo@d03@rm'] = 15 },
}

local function applyProgress(src, level)
    for style, attacks in pairs(Unlocks) do
        for attackId, minLevel in pairs(attacks) do
            exports.avalon_combat:SetAttackEnabled(src, style, attackId, level >= minLevel)
        end
    end
    exports.avalon_combat:SetOutgoingMultiplier(src, 1.0 + level * 0.02) -- +2% de daño por nivel
end

AddEventHandler('mylevels:loaded', applyProgress)  -- personaje cargado
AddEventHandler('mylevels:levelUp', applyProgress) -- nuevo nivel
```

---

## Exportaciones Disponibles

Un `target` es el ID de servidor de un jugador o `{ net = netId }` para un NPC combatiente.

### Exports Server-Side

| Export | Descripción |
|--------|-------------|
| `SetStyle(src, estilo \| nil)` | Pone al jugador en combate con ese estilo, o se lo quita |
| `GetStyle(src)` | Estilo actual del jugador |
| `SetHeld(src, slot, clave \| nil)` | Arma (`'weapon'`, `'offhand'`) o escudo (`'shield'`) en la mano |
| `GetHeld(src)` | `{ slot = clave }` |
| `SpawnFighter(estilo, coords, heading, opts)` | Crea un NPC combatiente, devuelve el net id |
| `StopFighter(net)` | Quita un NPC combatiente |
| `AddHitHandler(fn)` / `RemoveHitHandler(id)` | Función llamada en cada golpe (ver abajo) |
| `GetDamageMode()` | `'native'` o `'external'` |
| `React(target, qué)` | Hace ejecutar `'stagger'`, `'knockdown'` o `'death'` al objetivo |
| `SetAttackEnabled(target, estilo, attackId, on)` / `IsAttackEnabled(...)` | Desbloquea o bloquea un ataque para ese combatiente |
| `SetAttackMultiplier(target, estilo, attackId, m)` / `GetAttackMultiplier(...)` | Multiplicador de un ataque |
| `SetOutgoingMultiplier(target, m)` / `GetOutgoingMultiplier(target)` | Multiplicador sobre todo su daño |
| `SetDefence(target, { percent, flat })` / `GetDefence(target)` | Defensa propia, sumada a la de la guardia |
| `GetGuard` / `SetGuard` / `GetMaxGuard` / `SetMaxGuard` | Puntos de guardia (modo `'points'`) |
| `SetInputMode(target, 'combo' \| 'keys' \| nil)` / `GetInputMode(target)` | Modo de input del combatiente |
| `GetStyles()` | Todos los estilos cargados |
| `GetAttacks(estilo)` | `{ { id, chain, clip, mult, locked, input } }` |
| `GetWeapon(clave)` / `GetShield(clave)` | Entrada del config |

#### El golpe (`hit`)

Los hit handlers y el evento `avalon_combat:hit` reciben esta tabla:

| Campo | Descripción |
|-------|-------------|
| `mode` | `'native'` o `'external'` |
| `attacker` | `{ kind = 'player' \| 'fighter', src, net, style, team }` |
| `victim` | `{ kind = 'player' \| 'fighter' \| 'ped', src, net, style }` |
| `attack` | `{ id, chain, clip, mult, knockdown, react }` (`chain`: `light`, `heavy`, `special`, `moves`, `pair`) |
| `weapon` | `{ slot, key, data }` si ha golpeado una hoja, si no `nil` |
| `base` | Daño del arma o del estilo |
| `attackMult`, `progressMult`, `outgoingMult` | Los tres multiplicadores |
| `damage` | Daño antes de la guardia |
| `guard` | `{ blocked, parried, broken, percent, flat, cost }` |
| `final` | Daño después de la guardia (el aplicado en `'native'`) |
| `direction` | `{ x, y }` de quien ataca hacia la víctima |

Los handlers se ejecutan después del cálculo de la guardia. Un handler devuelve el golpe modificado, `nil` para dejarlo como está o `false` para anularlo. Los handlers de un recurso que se detiene se eliminan solos.

```lua
-- server: resistencias y niveles
exports.avalon_combat:AddHitHandler(function(hit)
    if hit.victim.kind == 'player' and HasSilverArmor(hit.victim.src) and hit.attacker.style == 'werewolf' then
        hit.final = hit.final * 0.5 -- redondeado por el servidor después de los handlers
        return hit
    end
end)
```

```lua
-- server: DamageMode = 'external', vida gestionada por tu sistema
AddEventHandler('avalon_combat:hit', function(hit)
    if hit.victim.kind ~= 'player' then return end
    local hp = MyHealth.Remove(hit.victim.src, hit.final)
    if hp <= 0 then exports.avalon_combat:React(hit.victim.src, 'death') end
end)
```

**Eventos server** (`AddEventHandler`, reciben el golpe): `avalon_combat:hit` (modo `'external'`), `avalon_combat:blocked`, `avalon_combat:parried`, `avalon_combat:guardBroken`.

---

### Exports Client-Side

| Export | Devuelve |
|--------|----------|
| `GetStyle()` | Estilo actual o `nil` |
| `IsBlocking()` | `boolean` |
| `IsAttacking()` | `boolean` |
| `GetGuard()` | `{ v, max }` puntos de guardia (modo `'points'`) |
| `GetStamina()` | Stamina del juego, tal como la lee el modo `'stamina'` |
| `SetCombatEnabled(on)` | `false` deja los controles al juego (menú, cutscene) hasta que vuelve a `true` |
| `GetCastPoint()` | `vector3` desde el que salen las magias del bastón, o `nil` |
| `GetInputMode()` | `'combo'` o `'keys'` |
| `MakeFighter(ped, estilo, opts)` / `StopFighter(ped)` | Convierte un ped en combatiente, o lo detiene |

**Evento local** `avalon_combat:onHurt` `(ped, info, attacker)`: en cada golpe recibido por un ped gestionado por este cliente, para un HUD.

```lua
-- client: barra de vida y números de daño
AddEventHandler('avalon_combat:onHurt', function(ped, info, attacker)
    if ped ~= PlayerPedId() then return end
    if info.parried then
        ShowText('¡PARADA!')
    elseif info.blocked then
        ShowText(('-%d (parado)'):format(info.damage))
    else
        ShowText(('-%d'):format(info.damage))
    end
end)

-- client: nada de combate mientras el menú está abierto
exports.avalon_combat:SetCombatEnabled(false)
```
