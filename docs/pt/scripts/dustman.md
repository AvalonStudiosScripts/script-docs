# avalon_dustman

Script para o **trabalho de lixeiro** no FiveM. Suporta modo individual e modo grupo.

**Versão:** 1.0 | **Autor:** Mino

---

## Configuração

| Opção | Padrão | Descrição |
|-------|--------|-----------|
| `Config.Debug` | `true` | Ativar logs de depuração |
| `Config.PartyMembers` | `4` | Máximo de jogadores em grupo |
| `Config.BagsPerSector` | `10` | Sacos para completar um setor |
| `Config.StressMax` | `10000` | Stress máximo para trabalhar |
| `Config.StressValue` | `100` | Stress adicionado por cada saco recolhido |

### Modos

=== "Individual"
    ```lua
    Config.Single = { vehicle = "utillitruck3", ticketRate = 5 }
    ```
=== "Grupo"
    ```lua
    Config.Group  = { vehicle = "trash", ticketRate = 4, maxTrucks = 4 }
    ```

---

## Exports Disponíveis

!!! info "Sem exports registados"
    `avalon_dustman` não expõe exports públicos.
