# avalon_trash

Script para **procurar em objetos abandonados** no FiveM. Os jogadores podem procurar em caixotes, contentores e caixas de correio para encontrar loot aleatório.

**Versão:** 1.0 | **Autor:** Mino

---

## Configuração

```lua
CONFIG.SEARCH_TIME        = 5000
CONFIG.COOLDOWN           = 60
CONFIG.TRASH_REFRESH_TIME = 5 * 60
```

### Categorias

| Categoria | Item Necessário | Bónus |
|-----------|:--------------:|-------|
| `small_bin` | ❌ | +25% probabilidade com `trash_gloves` |
| `dumpster` | `trash_gloves` | +40% probabilidade |
| `mailbox` | `crowbar` | +60% probabilidade |

### Loot

```lua
CONFIG.TRASH_LOOT = {
    small_bin = { { item = "weed",  chance = 50, amount = {1,20} } },
    dumpster  = { { item = "scrap", chance = 70, amount = {2,6}  } },
    mailbox   = { { item = "letter",chance = 80, amount = {1,2}  } },
}
```

---

## Exports Disponíveis

!!! info "Sem exports registados"
    `avalon_trash` não expõe exports públicos.
