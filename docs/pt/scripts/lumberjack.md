# avalon_lumberjack

Script para o **trabalho de lenhador** no FiveM. Os jogadores cortam árvores com um minijogo de pontuação.

**Versão:** 1.0 | **Autor:** Avalon

---

## Configuração

```lua
Lang = "Português"

Config.scoreGame  = true
Config.scoreToWin = 50

Config.Axe = {
    ["money"]      = { propModel = "prop_tool_fireaxe", BonusLoot = 2, BonusScore = 1 },
    ["tool_consaw"]= { propModel = "prop_tool_consaw",  BonusLoot = 3, BonusScore = 2 },
}

Config.durabilitySystemEnabled = true
Config.MaxDurability            = 200
Config.Durability               = 15

Config.Loot = {
    { name = "raw_material_wood", label = "Madeira", chance = 20, scoreGame = {min=1, max=5} },
    { name = "rubber",            label = "Borracha", chance = 5,  scoreGame = {min=1, max=5} },
    { name = "fibers",            label = "Fibras",  chance = 8,  scoreGame = {min=1, max=5} },
}
```

---

## Exports Disponíveis

!!! info "Sem exports registados"
    `avalon_lumberjack` não expõe exports públicos.
