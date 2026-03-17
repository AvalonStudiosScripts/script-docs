# avalon_electrician

FiveM **电工工作**脚本。玩家通过三种难度级别的小游戏修复电气箱。

**版本：** 1.0 | **作者：** Mino

---

## 难度级别

| 级别 | 名称 | 地点数量 | 薪酬 | 失败扣除 |
|------|------|:--------:|------|:--------:|
| `1` | 简单 | 33 | 100–150 票 | 10 票 |
| `2` | 中等 | 25 | 200–300 票 | 25 票 |
| `3` | 困难 | 15 | 400–600 票 | 50 票 |

## 工具

```lua
CONFIG.toolItem    = 'toolbox'
CONFIG.toolItemPro = { name = 'toolbox_pro', multiply = 1.3 }  -- 减少 30% 时间
```

## 压力值

```lua
CONFIG.StressMax   = 10000
CONFIG.StressValue = 100
```

---

## 可用导出函数

!!! info "无注册导出函数"
    `avalon_electrician` 不公开任何导出函数。
