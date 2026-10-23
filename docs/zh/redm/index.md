# RedM 脚本概览

AvalonStudios 的 RedM 脚本均为 **standalone**：不依赖任何框架，通过导出函数和事件与服务器的其他系统对接。

---

## 脚本列表

| 脚本 | 版本 | 客户端导出 | 服务端导出 | 数据库 |
|------|------|:---:|:---:|:---:|
| [avalon_combat](combat.md) | beta | ✅ 10 | ✅ 28 | ❌ |

---

## 各脚本的依赖项

```
avalon_combat  → OneSync, 动画包 avalon_anims_*
```

!!! info "动画包"
    战斗风格来自 Avalon 动画包（`avalon_anims_*`）：每个动画包都带有自己的风格，`avalon_combat` 会自动加载它们。参见[风格](combat.md#styles)。
