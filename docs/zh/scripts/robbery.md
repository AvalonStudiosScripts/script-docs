# avalon_robbery

FiveM **商店抢劫**脚本。玩家将武器对准收银员 NPC 以开始抢劫，逐步收集现金，并通过小游戏打开保险箱。

**版本：** 1.0 | **作者：** Mino

---

## 配置

```lua
CONFIG.SHOPS = {
    ["shop00"] = {
        ped         = { model = "s_m_m_ammucountry", coords = vector4(-1221.71, -908.18, 12.326, 0.0) },
        lockbox     = { coords = vector4(-1221.310, -915.907, 11.096, 123.85), weight = 50000 },
        items       = { {name = "money", quantity = 1000} },
        policeNotify= "商店发现可疑活动！",
        jobToNotify = "police",
        tools       = "stetoscopio",
        minigame    = { game = "Locked", options = nil },
        timeForRobbery  = 30000,   -- 30 秒
        startMoney      = 500,
        startMoneyStep  = 50,
        startMoneyDelay = 1000,
        shopResetTime   = 1800000  -- 30 分钟
    }
}
```

| 键 | 描述 |
|----|------|
| `timeForRobbery` | 抢劫持续时间（毫秒） |
| `startMoney` | 收银机中的总现金 |
| `startMoneyStep` | 每个动画步骤给予的现金 |
| `shopResetTime` | 商店重置前的毫秒数 |

---

## 可用导出函数

!!! info "无注册导出函数"
    `avalon_robbery` 不公开任何导出函数。
