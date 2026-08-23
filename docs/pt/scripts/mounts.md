# avalon_mounts

Script para **criaturas montáveis** no FiveM. Os jogadores podem invocar, montar e gerir criaturas fantásticas (dragões, grifos, pégasos, cavalos, alpacas) através de comandos ou itens.

**Versão:** 1.0 | **Autor:** Mino

---

## Configuração

### Opções Gerais

| Opção | Tipo | Padrão | Descrição |
|-------|------|--------|-----------|
| `CONFIG.command` | `boolean` | `true` | Ativar comandos para invocar criaturas |
| `CONFIG.commandName` | `string` | `"mount"` | Nome do comando invocar (`/mount`) |
| `CONFIG.delCommand` | `string` | `"delMount"` | Nome do comando dispensar (`/delMount`) |
| `CONFIG.items` | `boolean` | `true` | Usar itens do inventário para invocar |
| `CONFIG.invincible` | `boolean` | `false` | Tornar a criatura invencível |
| `CONFIG.maxrangedespawn` | `number` | `200.0` | Distância antes do auto-despawn |
| `CONFIG.timeForSpawnAfterDeaths` | `number` | `10000` | Espera (ms) para invocar após morrer |

### Teclas

| Opção | Tecla | Descrição |
|-------|:-----:|-----------|
| `CONFIG.keyMount` | `51` (E) | Montar / Desmontar |
| `CONFIG.keyDelete` | `74` (H) | Dispensar criatura |
| `CONFIG.keyFlymode` | `73` (X) | Ativar/desativar voo |

### Sistema de Voo

```lua
CONFIG.flyWithTime = false
CONFIG.flyTimeMax  = 60000
CONFIG.flyCooldown = 60000
```

---

## Criaturas Disponíveis

| Criatura | Variantes | Pode voar |
|----------|:---------:|:---------:|
| `necrodragon` | 5 | ✅ |
| `griffon` | 3 | ✅ |
| `pegaso` | 3 | ✅ |
| `horse` | 3 | ❌ |
| `alpaca` | 3 | ❌ |

---

## Exports Disponíveis

### `API` — Client-Side

```lua
local MountsAPI = exports['avalon_mounts']:API()

MountsAPI.spawn("necrodragon", 1)

if MountsAPI.isSpawned() then
    print("Jogador tem uma montaria ativa")
end

MountsAPI.despawn()
```

| Função | Descrição |
|--------|-----------|
| `spawn(name, color)` | Invocar uma criatura |
| `despawn()` | Dispensar a criatura ativa |
| `isSpawned()` | Há criatura invocada? |
| `isOnMount()` | O jogador está montado? |
| `getVehicle()` | Obtém a entidade veículo |
| `getData(model)` | Dados de uma criatura específica |

### Exports de Itens Dinâmicos

```lua
exports['avalon_mounts']:necrodragon_1()
exports['avalon_mounts']:griffon_1()
exports['avalon_mounts']:pegaso_1()
exports['avalon_mounts']:horse_1()
exports['avalon_mounts']:alpaca_1()
```
