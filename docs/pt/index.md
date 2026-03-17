# AvalonStudios Script Docs

Bem-vindo à documentação oficial dos scripts FiveM da **AvalonStudios**.

Aqui encontras a referência completa de todos os scripts: configuração, exports disponíveis e exemplos de utilização.

---

## Scripts Disponíveis

| Script | Descrição | Exports |
|--------|-----------|:-------:|
| [avalon_coltivation](scripts/coltivation.md) | Sistema de cultivo de plantas | ✅ |
| [avalon_dustman](scripts/dustman.md) | Trabalho de lixeiro (individual/grupo) | ❌ |
| [avalon_electrician](scripts/electrician.md) | Trabalho de eletricista com minijogos | ❌ |
| [avalon_lumberjack](scripts/lumberjack.md) | Trabalho de lenhador com minijogo | ❌ |
| [avalon_mining](scripts/mining.md) | Trabalho de mineiro com minijogo | ❌ |
| [avalon_robbery](scripts/robbery.md) | Sistema de roubo a lojas | ❌ |
| [avalon_trash](scripts/trash.md) | Sistema de busca no lixo | ❌ |

---

## Dependências Comuns

Todos os scripts requerem os seguintes recursos ativos:

- **[ox_lib](https://github.com/overextended/ox_lib)** — Biblioteca base
- **[ox_target](https://github.com/overextended/ox_target)** — Sistema de interações
- **[ox_inventory](https://github.com/overextended/ox_inventory)** — Gestão de inventário
- **[es_extended](https://github.com/esx-framework/esx_core)** — Framework ESX

!!! tip "Dica"
    Garante que todas as dependências sejam iniciadas **antes** dos scripts Avalon no `server.cfg`.

---

## Como Usar os Exports

Os exports permitem que outros scripts chamem funções de um script Avalon sem dependências diretas no código.

=== "Client"

    ```lua
    exports['nome-resource']:NomeFuncao(param1, param2)
    ```

=== "Server"

    ```lua
    exports['nome-resource']:NomeFuncao(param1, param2)
    ```

!!! warning "Importante"
    Os exports de **cliente** só podem ser chamados de scripts do lado cliente.
    Os exports de **servidor** só podem ser chamados de scripts do lado servidor.

---

*Documentação por AvalonStudios — [GitHub](https://github.com/AvalonStudiosScripts)*
