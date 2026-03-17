# avalon_robbery

Script para **roubos em lojas** no FiveM. Os jogadores apontam uma arma ao caixa para iniciar o roubo e abrem o cofre com um minijogo.

**Versão:** 1.0 | **Autor:** Mino

---

## Configuração

```lua
CONFIG.SHOPS = {
    ["shop00"] = {
        ped         = { model = "s_m_m_ammucountry", coords = vector4(-1221.71, -908.18, 12.326, 0.0) },
        lockbox     = { coords = vector4(-1221.310, -915.907, 11.096, 123.85), weight = 50000 },
        items       = { {name = "money", quantity = 1000} },
        policeNotify= "Atividade suspeita na loja!",
        jobToNotify = "police",
        tools       = "stetoscopio",
        minigame    = { game = "Locked", options = nil },
        timeForRobbery  = 30000,
        startMoney      = 500,
        startMoneyStep  = 50,
        startMoneyDelay = 1000,
        shopResetTime   = 1800000
    }
}
```

---

## Exports Disponíveis

!!! info "Sem exports registados"
    `avalon_robbery` não expõe exports públicos.
