# avalon_trash

Script para **buscar en objetos abandonados** en FiveM. Los jugadores buscan en papeleras, contenedores y buzones para encontrar botín aleatorio.

**Versión:** 1.0 | **Autor:** Mino

---

## Configuración

```lua
CONFIG.SEARCH_TIME        = 5000
CONFIG.COOLDOWN           = 60
CONFIG.TRASH_REFRESH_TIME = 5 * 60
```

### Categorías

| Categoría | Item Requerido | Bonus |
|-----------|:-----------:|-------|
| `small_bin` | ❌ | +25% probabilidad con `trash_gloves` |
| `dumpster` | `trash_gloves` | +40% probabilidad con `trash_gloves` |
| `mailbox` | `crowbar` | +60% probabilidad con `crowbar` |

### Botín

```lua
CONFIG.TRASH_LOOT = {
    small_bin = { { item = "weed",  chance = 50, amount = {1,20} } },
    dumpster  = { { item = "scrap", chance = 70, amount = {2,6}  } },
    mailbox   = { { item = "letter",chance = 80, amount = {1,2}  } },
}
```

---

## Exports Disponibles

!!! info "Sin exports registrados"
    `avalon_trash` no expone exports públicos.
