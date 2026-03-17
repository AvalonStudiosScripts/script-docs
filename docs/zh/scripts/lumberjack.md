# avalon_lumberjack

FiveM **伐木工工作**脚本。玩家使用计分小游戏砍伐树木，支持多种斧头、耐久度系统和技能加成。

**版本：** 1.0 | **作者：** Avalon

---

## 配置

```lua
Lang = "中文"

Config.scoreGame  = true   -- 启用计分系统
Config.scoreToWin = 50     -- 获胜所需分数

Config.Axe = {
    ["money"]      = { propModel = "prop_tool_fireaxe", BonusLoot = 2, BonusScore = 1 },
    ["tool_consaw"]= { propModel = "prop_tool_consaw",  BonusLoot = 3, BonusScore = 2 },
}

Config.durabilitySystemEnabled = true
Config.MaxDurability            = 200
Config.Durability               = 15  -- 每次砍树减少的耐久度

Config.Loot = {
    { name = "raw_material_wood", label = "木材",   chance = 20, scoreGame = {min=1, max=5} },
    { name = "rubber",            label = "橡胶",   chance = 5,  scoreGame = {min=1, max=5} },
    { name = "fibers",            label = "纤维",   chance = 8,  scoreGame = {min=1, max=5} },
}
```

---

## 可用导出函数

!!! info "无注册导出函数"
    `avalon_lumberjack` 不公开任何导出函数。
