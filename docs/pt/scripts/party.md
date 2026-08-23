# avalon_party

Sistema de **party estilo MMORPG** para FiveM. Grupos com HUD em tempo real, barras vitais personalizáveis, ready check, waypoint compartilhado, espectador automático, blips e marcadores sobre os companheiros, marcação de alvos e instâncias privadas (routing buckets).

**Versão:** 1.0.0 | **Autor:** Avalon | **Framework:** ESX / QBCore / QBox / Standalone

---

## Dependências

| Resource | Obrigatório |
|----------|:-----------:|
| ox_lib | ✅ |
| ox_target | ✅ |

---

## Instalação

```
ensure ox_lib
ensure ox_target
ensure avalon_party
```

O framework é detectado sozinho: `es_extended` → ESX, `qbx_core` → QBox, `qb-core` → QBCore, senão standalone.

---

## Idiomas

Os textos ficam em `locales/<código>.lua`. Incluídos: **inglês, italiano, português, espanhol, russo, chinês simplificado, tailandês**.

```lua
-- config.lua (mantenha esta linha no topo do arquivo)
Config.locale = 'pt'   -- en | it | pt | es | ru | zh | th
```

Uma chave que falte numa tradução cai automaticamente para o inglês. Para adicionar um idioma: copie `locales/en.lua`, renomeie (ex. `de.lua`), traduza os valores, troque `Locales['en']` por `Locales['de']` e defina `Config.locale = 'de'`.

---

## Comandos

| Comando | Argumento | Descrição |
|---------|-----------|-----------|
| `/party` | — | Abrir o menu do party |
| `/partyinvite` | `[id jogador]` | Convidar um jogador |
| `/partyaccept` | — | Aceitar convite |
| `/partydecline` | — | Recusar convite |
| `/partyleave` | — | Sair do party |
| `/partydisband` | — | Desfazer o party (líder) |
| `/partykick` | `[id jogador]` | Expulsar um membro (líder) |
| `/partytransfer` | `[id jogador]` | Transferir liderança (líder) |
| `/partyready` | — | Iniciar ready check (líder) |
| `/partywp` | — | Compartilhar waypoint (líder) |

Não é preciso já ter um party: o primeiro convite cria o grupo. Mirando outro jogador com **ox_target** aparece a opção **"Convidar para o Party"** (3 m).

---

## Teclas

| Tecla | Ação |
|-------|------|
| **F5** | Abrir o menu do party |
| **F1** / **F2** | Aceitar / recusar convite |
| **F3** / **F4** | Pronto / não pronto (ready check) |
| **TAB** | Ciclar alvos próximos (ativado no menu) |
| **CTRL** (segurar) | Mira de marcação, só o líder — clique direito num ped abre os símbolos |
| **← →** | Trocar de jogador observado no modo espectador |

Os padrões estão em `Config.Keys`; cada jogador pode remapear nas configurações de teclas do FiveM.

---

## Menu do party (F5)

Tela NUI com: lista de membros (kick e transferência para o líder), sair/desfazer, ready check, compartilhar waypoint, toggle do alvo TAB e da marcação, visibilidade de cada barra e dos status, visibilidade de blips e marcadores 3D, e escala do HUD de `0.5` a `3.0`.

!!! tip "As opções ficam salvas"
    As escolhas são **por jogador** e ficam no `localStorage` da NUI: sem tráfego de servidor nem banco de dados.

---

## Configuração

| Opção | Padrão | Descrição |
|-------|--------|-----------|
| `Config.locale` | `'en'` | Idioma dos textos |
| `Config.maxPartySize` | `6` | Máximo de membros |
| `Config.inviteExpiry` | `30000` | ms até o convite expirar |
| `Config.offlineGracePeriod` | `120000` | ms que um desconectado mantém a vaga |
| `Config.readyCheckTimeout` | `15000` | Duração do ready check em ms |

```lua
Config.Features = {
    distance   = true,  -- distância em metros nos cards
    readyCheck = true,  -- ready check
    waypoint   = true,  -- waypoint compartilhado
    spectator  = true,  -- espectador automático ao morrer
    targetMark = true,  -- marcação de alvos com símbolos
}

Config.UI = {
    position       = 'top-left',  -- top-left | top-right | bottom-left | bottom-right
    pickClick      = 'right',     -- botão do mouse para escolher um membro
    hudWidth       = 260,
    scale          = 1.0,         -- 1.0 = automático conforme a resolução
    showStatuses   = true,
    updateInterval = 500,         -- ms entre atualizações dos vitais
    criticalPct    = 20,          -- abaixo disso a barra pisca em vermelho
}
```

Com `scale = 1.0` o HUD se ajusta sozinho: `1.5` a partir de 2560 px, `1.6` a partir de 3440 px, `2.0` a partir de 3840 px.

### Vida e estamina

Nunca passam pelo servidor: cada cliente lê os nativos do GTA para todos os membros visíveis. Fora do alcance de streaming (~200 m) fica o último valor conhecido, marcado como desatualizado.

```lua
Config.Health = {
    displayMode     = 'usable',  -- 'usable' | 'raw' | 'percent'
    syncMaxHealth   = true,      -- compartilha o HP máximo entre clientes (state bag)
    applyMaxToClone = true,      -- eleva o teto de HP dos peds clonados
    autoDeadStatus  = true,      -- liga sozinho o status 'dead' com vida zero
    deadStatusId    = 'dead',
}
```

| `displayMode` | Mostra |
|---------------|--------|
| `usable` | `87/100` homem, `62/75` mulher — vida menos o limiar de morte |
| `raw` | `187/200` e `162/175` — valores nativos |
| `percent` | `87/100` para todos |

### Elementos no mundo e marcação

`Config.World` controla os blips no mapa (`showBlips`, `blipSprite`, `blipColorMember`, `blipColorLeader`, `blipScale`) e o marcador acima da cabeça (`showHeadMarker`, `headMarkerDist`, `headMarkerOffsetZ`, `headMarker`). São os padrões: cada jogador pode desligá-los pelo menu.

`Config.TargetMark` ajusta o alcance do TAB, as cores e o tamanho dos símbolos e o ícone da mira (`ctrlIcon`, qualquer classe Font Awesome). Os símbolos ficam em `Config.TargetSymbols`: `warning` ⚠️, `target` 🎯, `attack` ❌, `defend` 🛡️, `heal` 💊, `focus` ⭐.

### Barras

Barras com `builtin = true` se atualizam sozinhas pelos nativos do GTA. As de `builtin = false` ficam **escondidas até o seu script mandar um valor** com `SetMemberBar`.

| ID | Rótulo | Builtin | Cor |
|----|--------|:-------:|-----|
| `health` | HP | ✅ | Vermelho |
| `stamina` | Estamina | ✅ | Verde |
| `mana` | Mana | ❌ | Azul |
| `shield` | Escudo | ❌ | Cinza |
| `rage` | Fúria | ❌ | Laranja |

```lua
-- config.lua, dentro de Config.Bars
{
    id = 'energy', label = 'Energia', color = '#f39c12', bgColor = '#3d2200',
    icon = '⚡', defaultMax = 100, builtin = false, visible = true,
},
```

!!! tip "Controlar a vida com o seu sistema"
    Coloque `builtin = false` na barra `health` (ou `stamina`): o resource para de escrevê-la e o valor passa a ser o que você manda com `SetMemberBar`.

### Status

`combat` ⚔️ · `poisoned` ☠️ · `buffed` ✨ · `dead` 💀 · `afk` 💤 — ligados com `SetMemberStatus`, exceto `dead` que é automático enquanto `Config.Health.autoDeadStatus = true`.

```lua
-- config.lua, dentro de Config.Statuses
{ id = 'stunned', label = 'Atordoado', icon = '💫', color = '#9b59b6', duration = 0 },
```

---

## Exports Disponíveis

### Servidor

#### `IsInParty(source)` → `boolean`
```lua
if exports['avalon_party']:IsInParty(source) then
    -- lógica para jogadores em party
end
```

#### `GetPartyMembers(source)` → `table | nil`
```lua
local members = exports['avalon_party']:GetPartyMembers(source)
if members then
    for _, id in ipairs(members) do
        exports.ox_inventory:AddItem(id, 'pocao', 1)
    end
end
```

#### `GetPartyLeader(source)` → `number | nil`
```lua
local leader = exports['avalon_party']:GetPartyLeader(source)
```

#### `SetMemberBar(source, barId, value, maxValue)` → `boolean`
Atualiza uma barra no HUD visível para todo o party. Rejeitado em barras com `builtin = true`.

```lua
RegisterNetEvent('magia:lancar')
AddEventHandler('magia:lancar', function(custo)
    local src = source
    local novoMana = math.max(0, GetMana(src) - custo)
    exports['avalon_party']:SetMemberBar(src, 'mana', novoMana, 100)
end)
```

#### `SetMemberStatus(source, statusId, active)` → `boolean`
Mostrar/ocultar um ícone de status no HUD.

```lua
exports['avalon_party']:SetMemberStatus(source, 'poisoned', true)
exports['avalon_party']:SetMemberStatus(source, 'poisoned', false)
```

#### `SetPartyBucket(source, bucket)` → `boolean`
Move todo o party para um routing bucket (instância privada). O bucket fica ligado ao party: quem entra depois é movido junto e quem sai volta ao bucket 0. Não precisa ser líder.

```lua
RegisterNetEvent('masmorra:entrar')
AddEventHandler('masmorra:entrar', function(id)
    local src = source
    local ok = exports['avalon_party']:SetPartyBucket(src, 1000 + id)
    if not ok then
        TriggerClientEvent('ox_lib:notify', src, { type='error', description='Você precisa de um party!' })
    end
end)
```

#### `GetPartyBucket(source)` → `number`
```lua
local bucket = exports['avalon_party']:GetPartyBucket(source)  -- 0 = mundo principal
```

---

### Cliente

#### `RequestMemberPick(mode, callbackEvent)`
Ativa a seleção de membro: o cursor aparece sobre os cards do party e o jogador clica em um.

- `mode` `"self"` → o evento chega a quem selecionou · `"target"` → chega ao membro selecionado
- O callback recebe `{ id, name, bars, statuses, isLeader, offline, picker }`

!!! warning "Sem callback ao cancelar"
    Com **ESC** ou `CancelMemberPick()` o evento **não** é disparado. O botão do mouse é definido em `Config.UI.pickClick`.

```lua
RegisterCommand('curar_aliado', function()
    exports['avalon_party']:RequestMemberPick('self', 'cura:alvo')

    SetTimeout(10000, function()
        exports['avalon_party']:CancelMemberPick()
    end)
end)

RegisterNetEvent('cura:alvo')
AddEventHandler('cura:alvo', function(member)
    if member.offline then return end
    TriggerServerEvent('cura:aplicar', member.id, 50)
end)
```

#### `CancelMemberPick()`
Cancela a seleção ativa sem escolher ninguém. Nenhum evento é disparado.

```lua
exports['avalon_party']:CancelMemberPick()
```

#### `GetPartyTarget()` → `{ ped, netId, source, isPlayer } | nil`
Devolve o ped selecionado pelo ciclo **TAB**. `source` é `nil` quando o ped é um NPC.

```lua
local target = exports['avalon_party']:GetPartyTarget()
if target and target.isPlayer then
    TriggerServerEvent('combate:atacar', target.source)
end
```

!!! tip "Arquivo de referência"
    O resource inclui `example_integration.lua`: nunca é carregado, é só uma coleção de snippets prontos para copiar.
