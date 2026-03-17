# avalon_trash

FiveM **垃圾搜索**脚本。玩家可以在垃圾桶、大型容器和信箱中搜索随机战利品。

**版本：** 1.0 | **作者：** Mino

---

## 配置

```lua
CONFIG.SEARCH_TIME        = 5000    -- 搜索进度条时间（毫秒）
CONFIG.COOLDOWN           = 60      -- 再次搜索同一容器前的冷却时间（秒）
CONFIG.TRASH_REFRESH_TIME = 5 * 60  -- 所有战利品重新生成前的时间（秒）
```

### 容器类别

| 类别 | 所需物品 | 加成 |
|------|:-------:|------|
| `small_bin`（小垃圾桶） | ❌ | 持有 `trash_gloves` 时 +25% 概率 |
| `dumpster`（大垃圾箱） | `trash_gloves` | 持有时 +40% 概率 |
| `mailbox`（信箱） | `crowbar` | 持有时 +60% 概率 |

### 战利品

```lua
CONFIG.TRASH_LOOT = {
    small_bin = { { item = "weed",  chance = 50, amount = {1,20} } },
    dumpster  = { { item = "scrap", chance = 70, amount = {2,6}  } },
    mailbox   = { { item = "letter",chance = 80, amount = {1,2}  } },
}
```

---

## 可用导出函数

!!! info "无注册导出函数"
    `avalon_trash` 不公开任何导出函数。
