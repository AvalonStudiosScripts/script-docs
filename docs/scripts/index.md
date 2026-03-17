# Panoramica Script

Tutti gli script AvalonStudios sono progettati per **ESX** con supporto a **ox_lib**, **ox_target** e **ox_inventory**.

---

## Lista Script

| Script | Versione | Export Client | Export Server | Database |
|--------|----------|:---:|:---:|:---:|
| [avalon_coltivation](coltivation.md) | 1.0 | ✅ 5 | ❌ | ✅ MySQL |
| [avalon_dustman](dustman.md) | 1.0 | ❌ | ❌ | ❌ |
| [avalon_electrician](electrician.md) | 1.0 | ❌ | ❌ | ❌ |
| [avalon_lumberjack](lumberjack.md) | 1.0 | ❌ | ❌ | ❌ |
| [avalon_mining](mining.md) | 1.0 | ❌ | ❌ | ❌ |
| [avalon_robbery](robbery.md) | 1.0 | ❌ | ❌ | ❌ |
| [avalon_trash](trash.md) | 1.0 | ❌ | ❌ | ❌ |

---

## Dipendenze per Script

```
avalon_coltivation  → ox_lib, oxmysql, ox_target, ox_inventory
avalon_dustman      → ox_lib, ox_target, ESX
avalon_electrician  → ox_lib, ESX, framework:PlayMinigame
avalon_lumberjack   → ox_lib, ox_target, ox_inventory
avalon_mining       → ox_lib, ox_target, ox_inventory
avalon_robbery      → ox_lib, ESX, ox_target, ox_inventory
avalon_trash        → ox_lib, ox_target, ox_inventory
```
