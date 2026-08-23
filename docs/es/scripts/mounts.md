# avalon_mounts

Script para **criaturas montables** en FiveM. Los jugadores pueden invocar, montar y gestionar criaturas fantásticas (dragones, grifos, pegasos, caballos, alpacas) mediante comandos o ítems.

**Versión:** 1.0 | **Autor:** Mino

---

## Configuración

### Opciones Generales

| Opción | Tipo | Por defecto | Descripción |
|--------|------|-------------|-------------|
| `CONFIG.command` | `boolean` | `true` | Activar comandos para invocar criaturas |
| `CONFIG.commandName` | `string` | `"mount"` | Nombre del comando invocar (`/mount`) |
| `CONFIG.delCommand` | `string` | `"delMount"` | Nombre del comando despedir (`/delMount`) |
| `CONFIG.items` | `boolean` | `true` | Usar ítems del inventario para invocar |
| `CONFIG.invincible` | `boolean` | `false` | Hacer la criatura invencible |
| `CONFIG.maxrangedespawn` | `number` | `200.0` | Distancia antes del auto-despawn |
| `CONFIG.timeForSpawnAfterDeaths` | `number` | `10000` | Espera (ms) para invocar después de morir |

### Teclas

| Opción | Tecla | Descripción |
|--------|:-----:|-------------|
| `CONFIG.keyMount` | `51` (E) | Montar / Desmontar |
| `CONFIG.keyDelete` | `74` (H) | Despedir criatura |
| `CONFIG.keyFlymode` | `73` (X) | Activar/desactivar vuelo |

### Sistema de Vuelo

```lua
CONFIG.flyWithTime = false   -- false = vuelo ilimitado
CONFIG.flyTimeMax  = 60000   -- ms de vuelo disponibles
CONFIG.flyCooldown = 60000   -- ms de recarga
```

---

## Criaturas Disponibles

| Criatura | Variantes | Puede volar |
|----------|:---------:|:-----------:|
| `necrodragon` | 5 | ✅ |
| `griffon` | 3 | ✅ |
| `pegaso` | 3 | ✅ |
| `horse` | 3 | ❌ |
| `alpaca` | 3 | ❌ |

---

## Exports Disponibles

### `API` — Client-Side

```lua
local MountsAPI = exports['avalon_mounts']:API()

-- Invocar un necrodragon (variante 1)
MountsAPI.spawn("necrodragon", 1)

-- Verificar si hay una criatura invocada
if MountsAPI.isSpawned() then
    print("El jugador tiene una montura activa")
end

-- Despedir
MountsAPI.despawn()
```

#### Funciones

| Función | Descripción |
|---------|-------------|
| `spawn(name, color)` | Invocar una criatura |
| `despawn()` | Despedir la criatura activa |
| `isSpawned()` | ¿Hay criatura invocada? |
| `isOnMount()` | ¿El jugador está montado? |
| `getVehicle()` | Obtiene la entidad vehículo |
| `getData(model)` | Datos de una criatura específica |
| `getAllData()` | Datos de todas las criaturas |

### Exports de Ítems Dinámicos

```lua
exports['avalon_mounts']:necrodragon_1()
exports['avalon_mounts']:griffon_1()
exports['avalon_mounts']:pegaso_1()
exports['avalon_mounts']:horse_1()
exports['avalon_mounts']:alpaca_1()
```
