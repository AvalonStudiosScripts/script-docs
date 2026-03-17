# avalon_mining

FiveM **矿工工作**脚本。与 `avalon_lumberjack` 结构相同，但适用于矿山：镐子代替斧头，矿石代替木材。

**版本：** 1.0 | **作者：** Avalon

---

## 配置

```lua
Lang = "中文"

Config.pickAxe = {
    ["tool_pickaxe"] = { propModel = "prop_tool_pickaxe", BonusLoot = 2 },
    ["tool_jackham"] = { propModel = "prop_tool_jackham",  BonusLoot = 3 },
}

Config.Loot = {
    { name = "rock",   label = "岩石",  chance = 20, scoreGame = {min=1, max=5} },
    { name = "iron",   label = "铁矿",  chance = 5,  scoreGame = {min=1, max=5} },
    { name = "copper", label = "铜矿",  chance = 8,  scoreGame = {min=1, max=5} },
}
```

---

## 可用导出函数

!!! info "无注册导出函数"
    `avalon_mining` 不公开任何导出函数。
