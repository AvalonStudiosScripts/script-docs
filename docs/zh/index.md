# AvalonStudios 脚本文档

欢迎来到 **AvalonStudios** FiveM 脚本的官方文档。

在这里你可以找到所有脚本的完整参考：配置、可用导出函数和使用示例。

---

## 可用脚本

| 脚本 | 描述 | 导出函数 |
|------|------|:--------:|
| [avalon_coltivation](scripts/coltivation.md) | 植物种植系统 | ✅ |
| [avalon_dustman](scripts/dustman.md) | 清洁工工作（单人/团队） | ❌ |
| [avalon_electrician](scripts/electrician.md) | 电工工作（含小游戏） | ❌ |
| [avalon_lumberjack](scripts/lumberjack.md) | 伐木工工作（含小游戏） | ❌ |
| [avalon_mining](scripts/mining.md) | 矿工工作（含小游戏） | ❌ |
| [avalon_robbery](scripts/robbery.md) | 商店抢劫系统 | ❌ |
| [avalon_trash](scripts/trash.md) | 垃圾搜索系统 | ❌ |

---

## 公共依赖项

所有脚本都需要以下资源处于运行状态：

- **[ox_lib](https://github.com/overextended/ox_lib)** — 基础库
- **[ox_target](https://github.com/overextended/ox_target)** — 交互系统
- **[ox_inventory](https://github.com/overextended/ox_inventory)** — 背包管理
- **[es_extended](https://github.com/esx-framework/esx_core)** — ESX 框架

!!! tip "提示"
    确保所有依赖项在 `server.cfg` 中 **先于** Avalon 脚本启动。

---

## 如何使用导出函数

导出函数允许其他脚本调用 Avalon 脚本的函数，无需直接代码依赖。

=== "客户端 (Client)"

    ```lua
    -- 从客户端调用导出函数
    exports['资源名称']:函数名(参数1, 参数2)
    ```

=== "服务端 (Server)"

    ```lua
    -- 从服务端调用导出函数
    exports['资源名称']:函数名(参数1, 参数2)
    ```

!!! warning "重要"
    **客户端**导出函数只能从客户端脚本调用。
    **服务端**导出函数只能从服务端脚本调用。

---

*由 AvalonStudios 制作 — [GitHub](https://github.com/AvalonStudiosScripts)*
