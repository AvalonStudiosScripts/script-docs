# avalon_coltivation

Script para el **cultivo de plantas** en FiveM. Permite a los jugadores plantar, regar, fertilizar y cosechar varios cultivos en terrenos válidos.

**Versión:** 1.0 | **Autor:** Mino | **Base de datos:** ✅ MySQL

---

## Dependencias

| Resource | Requerido |
|----------|:---------:|
| ox_lib | ✅ |
| oxmysql | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Instalación

```
ensure avalon_coltivation
```

---

## Configuración

### Opciones Generales

| Opción | Tipo | Por defecto | Descripción |
|--------|------|-------------|-------------|
| `CONFIG.MIN_PLANT_DISTANCE` | `number` | `1.5` | Distancia mínima entre plantas |
| `CONFIG.MIN_STAGE_TIME` | `number` | `300000` | Tiempo mínimo (ms) para avanzar de etapa |
| `CONFIG.StressMax` | `number` | `1000000` | Estrés máximo para poder plantar |
| `CONFIG.StressValue` | `number` | `100` | Estrés añadido cada vez que se planta |

### Plantas

| Planta | Semilla | Tiempo | Cosecha | Agua |
|--------|---------|--------|---------|:----:|
| `grain` | `seme_grano` | 30 min | 5x `grano` | ✅ |
| `canapa` | `seme_canapa` | 45 min | 3x `canapa` | ✅ |
| `coca` | `seme_coca` | 60 min | 2x `coca` | ✅ |
| `fungus` | `seme_fungo` | 20 min | 4x `funghi` | ❌ |
| `papavero` | `seme_papavero` | 50 min | 2x `papavero` | ✅ |

### Herramientas

| Herramienta | Item | Multiplicador | Uso único | Etapas |
|-------------|------|:-------------:|:---------:|--------|
| Regadera | `watering_can` | -25% tiempo | ❌ | seed, small |
| Fertilizante | `fertilizer` | -40% tiempo | ✅ | small, grown |
| Lámpara | `grow_lamp` | -15% tiempo | ✅ | seed, small, grown |

---

## Exports Disponibles

!!! info "Tipo: Client-Side"
    Estos exports están registrados en el **cliente** y deben llamarse desde scripts de cliente.

### `seme_grano`
Inicia la colocación de una semilla de trigo.
```lua
exports['avalon_coltivation']:seme_grano()
```

### `seme_canapa`
Inicia la colocación de una semilla de cáñamo.
```lua
exports['avalon_coltivation']:seme_canapa()
```

### `seme_coca`
Inicia la colocación de una semilla de coca.
```lua
exports['avalon_coltivation']:seme_coca()
```

### `seme_fungo`
Inicia la colocación de una semilla de seta.
```lua
exports['avalon_coltivation']:seme_fungo()
```

### `seme_papavero`
Inicia la colocación de una semilla de amapola.
```lua
exports['avalon_coltivation']:seme_papavero()
```

---

## Ejemplo de Uso

```lua
-- client/main.lua de tu script
RegisterCommand('plantar_trigo', function()
    exports['avalon_coltivation']:seme_grano()
end)
```
