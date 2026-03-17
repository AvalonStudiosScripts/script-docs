# avalon_coltivation

Script para o **cultivo de plantas** no FiveM. Permite aos jogadores plantar, regar, fertilizar e colher várias culturas em terrenos válidos.

**Versão:** 1.0 | **Autor:** Mino | **Base de dados:** ✅ MySQL

---

## Dependências

| Resource | Obrigatório |
|----------|:-----------:|
| ox_lib | ✅ |
| oxmysql | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## Configuração

### Opções Gerais

| Opção | Tipo | Padrão | Descrição |
|-------|------|--------|-----------|
| `CONFIG.MIN_PLANT_DISTANCE` | `number` | `1.5` | Distância mínima entre plantas |
| `CONFIG.MIN_STAGE_TIME` | `number` | `300000` | Tempo mínimo (ms) para avançar etapa |
| `CONFIG.StressMax` | `number` | `1000000` | Stress máximo para poder plantar |
| `CONFIG.StressValue` | `number` | `100` | Stress adicionado a cada planta colocada |

### Plantas

| Planta | Semente | Tempo | Colheita | Água |
|--------|---------|-------|----------|:----:|
| `grain` | `seme_grano` | 30 min | 5x `grano` | ✅ |
| `canapa` | `seme_canapa` | 45 min | 3x `canapa` | ✅ |
| `coca` | `seme_coca` | 60 min | 2x `coca` | ✅ |
| `fungus` | `seme_fungo` | 20 min | 4x `funghi` | ❌ |
| `papavero` | `seme_papavero` | 50 min | 2x `papavero` | ✅ |

### Ferramentas

| Ferramenta | Item | Multiplicador | Uso único | Etapas |
|------------|------|:-------------:|:---------:|--------|
| Regador | `watering_can` | -25% tempo | ❌ | seed, small |
| Fertilizante | `fertilizer` | -40% tempo | ✅ | small, grown |
| Lâmpada | `grow_lamp` | -15% tempo | ✅ | seed, small, grown |

---

## Exports Disponíveis

!!! info "Tipo: Client-Side"
    Estes exports estão registados no **cliente** e devem ser chamados de scripts cliente.

### `seme_grano`
Inicia a colocação de uma semente de trigo.
```lua
exports['avalon_coltivation']:seme_grano()
```

### `seme_canapa`
Inicia a colocação de uma semente de cânhamo.
```lua
exports['avalon_coltivation']:seme_canapa()
```

### `seme_coca`
Inicia a colocação de uma semente de coca.
```lua
exports['avalon_coltivation']:seme_coca()
```

### `seme_fungo`
Inicia a colocação de uma semente de cogumelo.
```lua
exports['avalon_coltivation']:seme_fungo()
```

### `seme_papavero`
Inicia a colocação de uma semente de papoila.
```lua
exports['avalon_coltivation']:seme_papavero()
```

---

## Exemplo de Utilização

```lua
RegisterCommand('plantar_trigo', function()
    exports['avalon_coltivation']:seme_grano()
end)
```
