# avalon_lumberjack

Script para el **trabajo de leñador** en FiveM. Los jugadores talan árboles con un minijuego de puntuación, con soporte para distintos tipos de hacha, sistema de durabilidad y bonificaciones de habilidad.

**Versión:** 1.0 | **Autor:** Avalon

---

## Configuración

### Idioma
```lua
Lang = "Español"
```

### Minijuego
```lua
Config.scoreGame  = true
Config.scoreToWin = 50
```

### Hachas

```lua
Config.Axe = {
    ["money"]      = { propModel = "prop_tool_fireaxe", BonusLoot = 2, BonusScore = 1 },
    ["tool_consaw"]= { propModel = "prop_tool_consaw",  BonusLoot = 3, BonusScore = 2 },
}
```

### Durabilidad

```lua
Config.durabilitySystemEnabled = true
Config.MaxDurability            = 200
Config.Durability               = 15
```

### Coordenadas de Árboles

```lua
Config.LumberCoords = {
    vector3(-527.5469, 537.0093, 111.5338),
}
```

### Botín

```lua
Config.Loot = {
    { name = "raw_material_wood", label = "Madera", chance = 20, scoreGame = {min=1, max=5} },
    { name = "rubber",            label = "Goma",   chance = 5,  scoreGame = {min=1, max=5} },
    { name = "fibers",            label = "Fibras", chance = 8,  scoreGame = {min=1, max=5} },
}
```

---

## Exports Disponibles

!!! info "Sin exports registrados"
    `avalon_lumberjack` no expone exports públicos.
