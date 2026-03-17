# avalon_robbery

Script para **robos en tiendas** en FiveM. Los jugadores apuntan un arma al cajero para iniciar el robo, recaudan dinero progresivamente y abren la caja fuerte con un minijuego.

**Versión:** 1.0 | **Autor:** Mino

---

## Configuración

### Tiendas

```lua
CONFIG.SHOPS = {
    ["shop00"] = {
        ped         = { model = "s_m_m_ammucountry", coords = vector4(-1221.71, -908.18, 12.326, 0.0) },
        lockbox     = { coords = vector4(-1221.310, -915.907, 11.096, 123.85), weight = 50000 },
        items       = { {name = "money", quantity = 1000} },
        policeNotify= "¡Actividad sospechosa en la tienda!",
        jobToNotify = "police",
        tools       = "stetoscopio",
        minigame    = { game = "Locked", options = nil },
        timeForRobbery  = 30000,
        startMoney      = 500,
        startMoneyStep  = 50,
        startMoneyDelay = 1000,
        shopResetTime   = 1800000
    }
}
```

| Clave | Descripción |
|-------|-------------|
| `timeForRobbery` | Duración del robo en ms |
| `startMoney` | Dinero total de la caja |
| `startMoneyStep` | Dinero por step de animación |
| `shopResetTime` | Ms antes de que la tienda vuelva a ser robable |

---

## Exports Disponibles

!!! info "Sin exports registrados"
    `avalon_robbery` no expone exports públicos.
