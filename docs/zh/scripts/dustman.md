# avalon_dustman

FiveM **清洁工工作**脚本。支持单人模式（小货车）和团队模式（垃圾车）。

**版本：** 1.0 | **作者：** Mino

---

## 配置

| 选项 | 默认值 | 描述 |
|------|--------|------|
| `Config.Debug` | `true` | 启用调试日志 |
| `Config.PartyMembers` | `4` | 团队最大人数 |
| `Config.BagsPerSector` | `10` | 完成一个区域需要收集的垃圾袋数量 |
| `Config.StressMax` | `10000` | 工作最大压力值 |
| `Config.StressValue` | `100` | 每次收集垃圾桶增加的压力值 |

### 工作模式

=== "单人"
    ```lua
    Config.Single = { vehicle = "utillitruck3", ticketRate = 5 }
    ```
=== "团队"
    ```lua
    Config.Group  = { vehicle = "trash", ticketRate = 4, maxTrucks = 4 }
    ```

---

## 可用导出函数

!!! info "无注册导出函数"
    `avalon_dustman` 不公开任何导出函数。
