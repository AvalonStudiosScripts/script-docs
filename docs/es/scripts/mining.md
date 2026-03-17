# avalon_mining

Script para el **trabajo de minero** en FiveM. Misma estructura que `avalon_lumberjack` pero adaptada a la mina: picos en lugar de hachas, minerales en lugar de madera.

**Versión:** 1.0 | **Autor:** Avalon

---

## Configuración

```lua
Lang = "Español"

Config.pickAxe = {
    ["tool_pickaxe"] = { propModel = "prop_tool_pickaxe", BonusLoot = 2 },
    ["tool_jackham"] = { propModel = "prop_tool_jackham",  BonusLoot = 3 },
}

Config.Loot = {
    { name = "rock",   label = "Roca",  chance = 20, scoreGame = {min=1, max=5} },
    { name = "iron",   label = "Hierro",chance = 5,  scoreGame = {min=1, max=5} },
    { name = "copper", label = "Cobre", chance = 8,  scoreGame = {min=1, max=5} },
}
```

---

## Exports Disponibles

!!! info "Sin exports registrados"
    `avalon_mining` no expone exports públicos.
