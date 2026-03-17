# avalon_dustman

Script para el **trabajo de basurero** en FiveM. Soporta modo individual (furgoneta) y modo grupo (camión de basura).

**Versión:** 1.0 | **Autor:** Mino

---

## Configuración

### General

| Opción | Tipo | Por defecto | Descripción |
|--------|------|-------------|-------------|
| `Config.Debug` | `boolean` | `true` | Activar logs de depuración |
| `Config.PartyMembers` | `number` | `4` | Máximo de jugadores en grupo |
| `Config.BagsPerSector` | `number` | `10` | Bolsas para completar un sector |
| `Config.StressMax` | `number` | `10000` | Estrés máximo para trabajar |
| `Config.StressValue` | `number` | `100` | Estrés añadido por cada bolsa recogida |

### Modos de Trabajo

=== "Individual"

    ```lua
    Config.Single = { vehicle = "utillitruck3", ticketRate = 5 }
    ```

=== "Grupo"

    ```lua
    Config.Group  = { vehicle = "trash", ticketRate = 4, maxTrucks = 4 }
    ```

### Sectores

```lua
Config.Sectors = {
    {
        name  = "Downtown",
        gps   = vec3(215.0, -810.0, 30.0),
        area  = { center = vec3(215.0, -810.0, 30.0), radius = 180.0 },
        props = Config.AllBins
    },
}
```

---

## Exports Disponibles

!!! info "Sin exports registrados"
    `avalon_dustman` no expone exports públicos.
