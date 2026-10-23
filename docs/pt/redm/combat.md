# avalon_combat

Sistema de **combate corpo a corpo** para RedM. Cada estilo (lobisomem, vampiro, katana, espada e escudo, pugilista, magos...) usa as animações dos pacotes Avalon. Os golpes são calculados pelo script: a cada frame segue mãos, pés e lâminas ao longo da animação e atinge os peds que atravessam. O dano passa sempre pelo servidor.

Inclui: combos leves e pesados, ataques emparelhados (dois peds animados em conjunto), defesa e contra-ataque, armas e escudos empunhados, corrida, esquiva, salto e voo para as criaturas, NPCs que lutam sozinhos e exports para ligar níveis, habilidades, barras de vida e magias.

**Versão:** beta | **Autor:** Avalon | **Jogo:** RedM | **Framework:** standalone

---

## Dependências

| Resource | Obrigatória | Para que serve |
|----------|:---:|---|
| OneSync | ✅ | NPCs lutadores e defesa (state bag) |
| `avalon_weapons_rdr2` | ✅ | Os modelos das armas empunhadas |
| `avalon_shields_tint` | ✅ | Os modelos dos escudos |
| Pacotes de animação `avalon_anims_*` | ✅ pelo menos um | As animações e os estilos (vê [Estilos](#estilos)) |

Nenhuma framework necessária: o script trabalha sobre peds e jogadores. A ligação com inventários, níveis ou barras de vida faz-se com os [exports](#exports-disponiveis).

---

## Instalação

1. Copia a pasta `avalon_combat/` para `resources/`
2. Inicia as armas, os escudos e os pacotes de animação, depois o resource:

```
ensure avalon_weapons_rdr2
ensure avalon_shields_tint
ensure avalon_anims_werewolf
ensure avalon_anims_vampire
ensure avalon_anims_ka_02
# ... os outros pacotes que usas
ensure avalon_combat
```

!!! info "Ordem dos pacotes"
    Um pacote iniciado depois do `avalon_combat` (ou reiniciado) é lido na mesma: os seus estilos são adicionados assim que arranca.

---

## Estilos

Os estilos vêm dos **pacotes de animação**. Cada pacote traz um ficheiro `avalon_combat.lua` com os seus estilos e declara-o no seu `fxmanifest.lua`. O `avalon_combat` recolhe-os sozinho através do export `CombatStyles` do pacote.

| Estilo | Pacote | Arma | Guarda | Extra |
|--------|--------|------|--------|-------|
| `werewolf` | `avalon_anims_werewolf` | garras | do jogo (mãos nuas) | corrida, esquiva, salto, ataques emparelhados |
| `vampire` | `avalon_anims_vampire` | garras | própria | voo, boost, ataques emparelhados |
| `succube` | `avalon_anims_succube` | garras | do jogo (mãos nuas) | voo, boost, ataques emparelhados |
| `katana` `katana1` `katana3` `katana4` | `avalon_anims_ka_02` `_01` `_03` `_04` | `xur_katana` | do jogo (lâmina) | |
| `sword` `sword2` `sword3` | `avalon_anims_sw_01` `_02` `_03` | `xur_machete2` | do jogo (lâmina) | |
| `sword_shield` `sword_shield2` `sword_shield3` | `avalon_anims_sns_01` `_02` `_03` | `swn2craft` + escudo | própria / própria / do jogo | |
| `twinsword` | `avalon_anims_twinsword01` | `xur_katana2` em ambas as mãos | do jogo (lâmina) | |
| `hammer2h` | `avalon_anims_hammer2h` | `hammermoore` | do jogo (lâmina) | |
| `orientalmonk` | `avalon_anims_orientalmonk` | `giga` | do jogo (lâmina) | |
| `pugilist` | `avalon_anims_p_01` | mãos nuas | do jogo (mãos nuas) | |
| `firemage` `mage` `witch` `wizard` | `avalon_anims_firemage_01` `_mage_01` `_witch_01` `_wizard` | mãos / cajado | do jogo (mãos nuas) | |

!!! warning "Magias"
    Os estilos de mago por agora atingem com as mãos, com um alcance maior (como faria o feitiço). Os cajados são apenas visuais: o ponto de onde partirão as magias lê-se com o export client `GetCastPoint()`.

Para escreveres um estilo teu vê [Criar um Estilo](#criar-um-estilo).

---

## Comandos e teclas

### Modo de combate

| Comando | Descrição |
|---------|-----------|
| `/avalon_combat <estilo>` | Entra em combate com esse estilo (sem argumento lista os estilos) |
| `/avalon_combat off` | Sai do combate |

| Tecla | Ação |
|-------|------|
| **Clique esquerdo** | Combo leve (carrega de novo durante o ataque para o golpe seguinte) |
| **Clique direito** | Combo pesado |
| **R** | Ataque emparelhado no ped à frente (os estilos que o têm) ou combo especial |
| **CTRL** (mantido) | Guarda |
| **E** | Fixa o ped apontado pela câmara (losango vermelho sobre a cabeça): ataques e ataques emparelhados vão para ele. E de novo liberta-o |
| **Shift** | Corrida (lobisomem) · voo (estilos voadores, no ar) |
| **Alt** | Esquiva (lobisomem) · boost (estilos voadores) |
| **Shift + Espaço** | Salto (lobisomem) · descolagem e aterragem (estilos voadores) |

!!! info "Quem reage aos golpes"
    Os golpes leves e pesados causam apenas dano. O combo especial, os ataques emparelhados e os ataques com `react = true` fazem também a vítima cambalear ou cair.

### Comandos de teste e ajuste

| Comando | Descrição |
|---------|-----------|
| `/avalon_attack <id>` | Executa um ataque do estilo atual (id ou nome da clip) |
| `/avalon_pair [nome]` | Executa um ataque emparelhado no ped à frente |
| `/avalon_pair_offset <nome> <x> <y> <yaw>` | Move a vítima de um ataque emparelhado (apenas neste client) |
| `/avalon_weapon <nome\|off>` | Muda a arma na mão direita |
| `/avalon_offhand <nome\|off>` | Muda a arma na mão esquerda |
| `/avalon_shield <nome\|off>` | Muda o escudo |
| `/avalon_weapon_offset <x y z rx ry rz> [osso]` | Ajusta a posição da arma e imprime a linha para copiar para o config (também `_offhand_offset`, `_shield_offset`) |
| `/avalon_sound <nome>` | Toca um som do config |
| `/avalon_sound_game <set> <nome> [bank]` | Experimenta um som qualquer do jogo |
| `/avalon_npc <estilo> [team] [modelo]` | Cria um NPC lutador 4 m à frente |
| `/avalon_npc_clear` | Apaga os NPCs criados por ti |
| `/avalon_npc_info` | Estado dos NPCs lutadores num raio de 30 m |
| `/avalon_dummy [modelo]` · `/avalon_dummy_clear` | Manequins parados para atingir |
| `/avalon_hitbox` | Mostra as hitboxes: corpos (verde), membros (amarelo), lâminas (azul-claro), golpes (vermelho) |
| `/avalon_combat_debug` | Desenha os movimentos das mãos e imprime cada golpe |

---

## Como funciona o dano

1. O client de quem ataca verifica a cada frame membros e lâminas ao longo da animação. Um membro mais lento do que `MinSweepSpeed` não atinge (carregamento e retorno do golpe).
2. Quando um membro ou uma lâmina toca a hitbox de um ped, o client comunica-o ao servidor.
3. O servidor verifica o golpe (distância, dano máximo, ataque desbloqueado), calcula dano e guarda e executa os *hit handlers* dos outros resources.
4. Consoante `AvalonCombat.DamageMode`:
    - `'native'` — o dano vai para a vida verdadeira do ped
    - `'external'` — sem dano: o servidor emite o evento `avalon_combat:hit` para o teu sistema (barras de vida falsas, resistências)

    A vítima executa na mesma a reação.

**Fórmula:** dano base (o da arma se atinge a lâmina, caso contrário o `damage` do estilo) × `mult` do ataque × multiplicador de progressão × multiplicador de saída, arredondado. Depois subtrai-se a defesa da guarda.

---

## Configuração

Os ficheiros de configuração estão em `config/`:

| Ficheiro | Conteúdo |
|----------|----------|
| `config/config.lua` | Definições gerais, combos, NPCs, sons, dano, defesa, input |
| `config/guard.lua` | As guardas do jogo utilizáveis por nome (`'blade'`, `'unarmed'`) |
| `config/weapons.lua` | Armas da mão direita |
| `config/offhand.lua` | Armas da mão esquerda |
| `config/shields.lua` | Escudos (uma entrada por cor) |
| `config/styles/` | Estilos locais opcionais e `example.lua` com todos os campos explicados |

### Definições gerais

| Opção | Default | Descrição |
|-------|---------|-----------|
| `TargetRange` | `4.0` | m: os peds mais distantes não são verificados |
| `LockRange` / `LockCone` | `3.0` / `45` | Um ataque vira-se para o ped mais próximo dentro destes metros e graus |
| `LockOnRange` | `12.0` | m: raio da fixação com E |
| `MinSweepSpeed` | `2.0` | m/s: um membro mais lento não atinge |
| `SkipStart` | `0.10` | Parte inicial de cada ataque ignorada |
| `Rehit` | `0.45` | s antes de o mesmo ataque poder voltar a atingir o mesmo alvo |
| `MaxHitDamage` | `200` | O servidor recusa golpes acima deste dano |
| `MaxHitDistance` | `6.0` | O servidor recusa golpes em jogadores mais distantes do que isto |
| `PairRange` | `3.5` | m: distância máxima de agarre de um ataque emparelhado |
| `HitboxScale` / `HitboxPad` | `1.0` / `0.0` | Aumenta as hitboxes das lâminas (para atingir mais facilmente) |
| `SoundVolume` | `0.35` | Volume dos ficheiros de áudio |
| `DamageMode` | `'native'` | `'native'` ou `'external'` (vê acima) |

### Combo

```lua
AvalonCombat.ComboWindow = 0.45 -- a partir desta fase do ataque, uma pressão põe em fila o golpe seguinte
AvalonCombat.ComboCancel = 1.0  -- fase em que parte o golpe em fila (1.0 = no fim da animação)
AvalonCombat.ComboReset = 0.8   -- s: depois desta pausa o combo recomeça do primeiro golpe
```

### Modo de input

```lua
AvalonCombat.InputMode = 'combo' -- 'combo' | 'keys'
AvalonCombat.Keys = { SimulWindow = 0.08, SequenceWindow = 0.6, DirAbove = 0.5 }
```

- `'combo'`: cada tecla executa o golpe seguinte da sua cadeia (`light`, `heavy`, `special`)
- `'keys'`: como num jogo de luta, as teclas executam as `moves` do estilo. Combinam-se teclas premidas em conjunto (`'light+heavy'`), uma direção (`'F+heavy'`: frente + pesado) ou sequências (`{ 'light', 'light', 'heavy' }`)

Cada jogador pode ter um diferente com o export `SetInputMode`.

### Guarda e defesa

```lua
AvalonCombat.Defence = {
    Mode = 'always',      -- 'always' | 'timed' | 'stamina' | 'points'
    ParryWindow = 0.25,   -- s: guarda levantada dentro deste tempo do golpe = defesa perfeita
    StaminaCost = 15.0,   -- 'stamina': stamina retirada por golpe defendido
    Points = 100, PointsRegen = 10, PointsCost = 20, -- 'points': pontos de guarda
    BreakTime = 1.5,      -- s sem guarda depois de se quebrar
    Block = { percent = 0.75, flat = 0 }, -- defesa base da guarda
}
```

| `Mode` | Quando a guarda trava um golpe |
|--------|--------------------------------|
| `always` | Sempre, dano reduzido pela defesa |
| `timed` | Só dentro de `ParryWindow` desde que é levantada, caso contrário o golpe entra inteiro |
| `stamina` | Sempre, cada golpe custa stamina; acabada a stamina a guarda quebra-se |
| `points` | Sempre, cada golpe custa pontos de guarda (exports `GetGuard` / `SetGuard`); a zero quebra-se |

Uma **defesa perfeita** (parry) anula o dano e faz cambalear quem ataca. **Defesa** = `dano × (1 - percent) - flat`, nunca abaixo de zero. Um escudo empunhado substitui a defesa da guarda pela sua; a dada com o export `SetDefence` soma-se.

### Sons

Cada som tem um nome em `AvalonCombat.Sounds`. Estilos, ataques, armas e ataques emparelhados usam esses nomes: um nome, uma lista da qual escolher um ao acaso, ou `{ light = ..., heavy = ..., special = ... }` por cadeia.

```lua
AvalonCombat.Sounds = {
    punch = { file = melee('punch', 11) },                 -- ficheiro em sounds/ (.wav .ogg .mp3)
    my_roar = { file = 'roar.ogg', volume = 0.8, range = 40 },
    beast_roar = { set = 'LSHNT_sounds', name = 'ROAR' },  -- som do jogo
}
```

Os sons corpo a corpo incluídos são os do jogo, extraídos para `sounds/melee_*.wav`.

!!! tip "Som no momento certo"
    `soundAt` faz o som começar numa fase precisa da animação: `0.35` = aos 35% da clip. Vale para os ataques, para os ataques emparelhados e para os movimentos do estilo (`soundAt = { leap = 0.27, dodge = 0.15 }`). Sem `soundAt` o som começa no primeiro movimento rápido (ataques) ou imediatamente.

### Hitbox do corpo

`AvalonCombat.BodyHitbox` lista as cápsulas sobre as quais se atinge cada ped (`{ osso de, osso até, raio }`): cabeça, tórax, abdómen, braços e pernas. Um animal sem esses ossos torna-se uma esfera de 0.4 m. `/avalon_hitbox` mostra-as no jogo.

---

## Armas e escudos

Cada entrada de `config/weapons.lua`, `config/offhand.lua` e `config/shields.lua` é um modelo empunhado:

```lua
AvalonCombat.Weapons.xur_katana = {
    model = 'weapon_xur_katana',
    bone = 'SKEL_R_Hand',                         -- osso ao qual está presa
    offset = vector3(0.075, 0.00, -0.03),         -- m
    rot = vector3(-90.0, 0.0, 0.0),               -- graus
    damage = 25,                                  -- por golpe de lâmina, × mult do ataque
    sounds = { swing = 'blade_swipe', hit = { 'machete_slash', 'blade_flesh' } },
    blade = { from = vector3(0, 0, 0.12), to = vector3(0, 0, 1.05), radius = 0.08 }, -- hitbox da lâmina
}
```

| Campo | Descrição |
|-------|-----------|
| `model` | Arquétipo do modelo |
| `tint` | Opcional: índice da cor (os escudos têm uma entrada por cor) |
| `bone`, `offset`, `rot` | Onde fica na mão. Ajusta-se no jogo com `/avalon_weapon_offset`, que imprime a linha para copiar |
| `damage` | Dano por golpe de lâmina |
| `sounds` | `swing` e `hit` da lâmina |
| `blade` | Cápsula da lâmina no espaço do modelo: atinge no lugar da mão |
| `cast` | Apenas cajados: o ponto de onde partirão as magias |
| `defence` | Apenas escudos: `{ percent, flat }` no lugar da da guarda |

---

## Criar um Estilo

Um estilo pode ser escrito em dois sítios:

=== "No pacote de animação"

    O modo usado pelos pacotes Avalon. Ao lado do `fxmanifest.lua` do pacote coloca `avalon_combat.lua`:

    ```lua
    local Styles = {}

    Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
        block = 'blade',   -- um pacote nomeia a guarda do jogo como string
    }

    exports('CombatStyles', function() return Styles end)
    ```

    e no `fxmanifest.lua` do pacote:

    ```lua
    avalon_combat_styles 'yes'
    shared_script 'avalon_combat.lua'
    ```

    Uma build do Avalon Explorer escreve estas duas linhas sozinha se o ficheiro existir.

=== "Em config/styles"

    Um ficheiro `config/styles/mystyle.lua` dentro do `avalon_combat`:

    ```lua
    AvalonCombat.Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
    }
    ```

    Um estilo escrito aqui **prevalece** sobre o de um pacote com o mesmo nome: para modificar um estilo de um pacote, copia-o para aqui.

`config/styles/example.lua` contém um estilo completo, comentado campo a campo.

### Campos de um estilo

| Campo | Descrição |
|-------|-----------|
| `dict` | Dicionário das animações (`.ycd`) |
| `damage` | Dano de um golpe do corpo (socos, pontapés, garras) |
| `idle` | Animação em loop parado |
| `weapon` / `offhand` / `shield` | O que empunha (chaves de `Weapons`, `Offhand`, `Shields`) |
| `hands` | Os membros que atingem: `{ osso de, osso até, alcance }` |
| `reach` / `radius` | Alcance para além do último osso e espessura dos membros (m) |
| `light` / `heavy` / `special` | As cadeias do combo: listas de ataques |
| `moves` | Os movimentos do modo `'keys'` |
| `sounds` | `swing`, `hit`, `knockdown`, `block`, `leap`, `dodge`, `boost`, `float` |
| `soundAt` | Fase em que partem os sons dos movimentos |
| `block` | Guarda: `{ start, loop, stop, hit }` com clips próprias, ou `'blade'` / `'unarmed'`. Sem `block` o estilo não defende |
| `stagger` / `knockdown` | Clips de reação aos golpes |
| `move` | Movimento especial: `kind = 'runner'` (corrida, esquiva, salto) ou `kind = 'flyer'` (voo, boost) |
| `pairs` | Ataques emparelhados |

### Um ataque

```lua
light = {
    { clip = 'attack@slash1', mult = 1.0 },
    { clip = 'attack@slash2', mult = 1.25, knockdown = true, soundAt = 0.35 },
},
```

| Campo | Descrição |
|-------|-----------|
| `clip` | A animação |
| `mult` | Multiplicador do dano |
| `knockdown` | A vítima cai |
| `react` | A vítima reage mesmo que não seja um ataque especial |
| `sound` / `soundAt` | Som próprio do ataque e fase em que começa |
| `keepTurn` | O ped fica virado como a animação o deixa |
| `locked` | Bloqueado até a progressão o desbloquear (`SetAttackEnabled`) |
| `id` | Nome do ataque para os exports (default: a clip) |

### Ataque emparelhado

```lua
pairs = {
    bite = { att = 'bite@att', vic = 'bite@vic', x = 0.5, y = 2.8, yaw = 196,
             mult = 4.0, damageAt = 0.45, sound = 'beast_roar', soundAt = 0.2 },
},
```

`att` / `vic` são as clips de quem ataca e da vítima. `x`, `y` e `yaw` indicam onde fica a vítima em relação a quem ataca (m, graus). `damageAt` é a fase em que chega o dano. Ajusta-se no jogo com `/avalon_pair_offset`.

!!! info "Movimento das animações"
    As clips que deslocam o ped seguem o seu *mover*, lido de `data/clip_movers.lua`. As clips de um pacote novo precisam dos seus movers, gerados com o Avalon Explorer; sem eles, a clip fica no sítio.

---

## NPCs lutadores

Um NPC lutador é um ped de rede que luta sozinho com um estilo: procura um alvo, aproxima-se, ataca em combo, defende-se, esquiva-se, salta ou voa.

- **Team:** os jogadores são `'player'`. Um NPC ataca os jogadores (a menos que o seu team seja `'player'`) e os NPCs de outros teams, nunca os peds normais.
- **Habilidades:** `AvalonCombat.Npc` em `config/config.lua` contém os valores por defeito (raio de agressão, distância de ataque, probabilidade de defesa, esquiva, ataque emparelhado...). Cada NPC pode alterar alguns com `opts.skill`.

```lua
-- server
local net = exports.avalon_combat:SpawnFighter('werewolf', vector3(-300.0, 790.0, 118.0), 90.0, {
    team = 'bandits',
    health = 400,
    skill = { BlockChance = 0.6, Weights = { heavy = 0.6 } },
})

-- mais tarde
exports.avalon_combat:StopFighter(net)
```

| `opts` | Descrição |
|--------|-----------|
| `model` | Modelo do ped (default `AvalonCombat.Npc.Model`) |
| `weapon` / `offhand` / `shield` | O que empunha (default: os do estilo; `shield = false` = nenhum) |
| `team` | Team (default `'npc'`) |
| `health` | Vida |
| `skill` | Apenas os valores de `AvalonCombat.Npc` que mudam |

!!! warning "É preciso um jogador"
    `SpawnFighter` cria o ped no client do jogador mais próximo das coordenadas: com o servidor vazio devolve `nil`.

---

## Exports Disponíveis

Um `target` é o ID de servidor de um jogador ou `{ net = netId }` para um NPC lutador.

### Exports Server-Side

| Export | Descrição |
|--------|-----------|
| `SetStyle(src, estilo \| nil)` | Põe o jogador em combate com esse estilo, ou retira-o |
| `GetStyle(src)` | Estilo atual do jogador |
| `SetHeld(src, slot, chave \| nil)` | Arma (`'weapon'`, `'offhand'`) ou escudo (`'shield'`) na mão |
| `GetHeld(src)` | `{ slot = chave }` |
| `SpawnFighter(estilo, coords, heading, opts)` | Cria um NPC lutador, devolve o net id |
| `StopFighter(net)` | Remove um NPC lutador |
| `AddHitHandler(fn)` / `RemoveHitHandler(id)` | Função chamada a cada golpe (vê abaixo) |
| `GetDamageMode()` | `'native'` ou `'external'` |
| `React(target, o_quê)` | Faz o alvo executar `'stagger'`, `'knockdown'` ou `'death'` |
| `SetAttackEnabled(target, estilo, attackId, on)` / `IsAttackEnabled(...)` | Desbloqueia ou bloqueia um ataque para esse lutador |
| `SetAttackMultiplier(target, estilo, attackId, m)` / `GetAttackMultiplier(...)` | Multiplicador de um ataque |
| `SetOutgoingMultiplier(target, m)` / `GetOutgoingMultiplier(target)` | Multiplicador sobre todo o seu dano |
| `SetDefence(target, { percent, flat })` / `GetDefence(target)` | Defesa própria, somada à da guarda |
| `GetGuard` / `SetGuard` / `GetMaxGuard` / `SetMaxGuard` | Pontos de guarda (modo `'points'`) |
| `SetInputMode(target, 'combo' \| 'keys' \| nil)` / `GetInputMode(target)` | Modo de input do lutador |
| `GetStyles()` | Todos os estilos carregados |
| `GetAttacks(estilo)` | `{ { id, chain, clip, mult, locked, input } }` |
| `GetWeapon(chave)` / `GetShield(chave)` | Entrada do config |

#### O golpe (`hit`)

Os hit handlers e o evento `avalon_combat:hit` recebem esta tabela:

| Campo | Descrição |
|-------|-----------|
| `mode` | `'native'` ou `'external'` |
| `attacker` | `{ kind = 'player' \| 'fighter', src, net, style, team }` |
| `victim` | `{ kind = 'player' \| 'fighter' \| 'ped', src, net, style }` |
| `attack` | `{ id, chain, clip, mult, knockdown, react }` (`chain`: `light`, `heavy`, `special`, `moves`, `pair`) |
| `weapon` | `{ slot, key, data }` se atingiu uma lâmina, caso contrário `nil` |
| `base` | Dano da arma ou do estilo |
| `attackMult`, `progressMult`, `outgoingMult` | Os três multiplicadores |
| `damage` | Dano antes da guarda |
| `guard` | `{ blocked, parried, broken, percent, flat, cost }` |
| `final` | Dano depois da guarda (o aplicado em `'native'`) |
| `direction` | `{ x, y }` de quem ataca para a vítima |

Os handlers correm depois do cálculo da guarda. Um handler devolve o golpe modificado, `nil` para o deixar como está ou `false` para o anular. Os handlers de um resource que pára são removidos automaticamente.

```lua
-- server: resistências e níveis
exports.avalon_combat:AddHitHandler(function(hit)
    if hit.victim.kind == 'player' and HasSilverArmor(hit.victim.src) and hit.attacker.style == 'werewolf' then
        hit.final = hit.final * 0.5 -- arredondado pelo servidor depois dos handlers
        return hit
    end
end)

-- desbloqueia um ataque ao nível 10
AddEventHandler('levels:up', function(src, level)
    if level == 10 then
        exports.avalon_combat:SetAttackEnabled(src, 'katana', 'attack@combo3', true)
    end
end)
```

```lua
-- server: DamageMode = 'external', vida gerida pelo teu sistema
AddEventHandler('avalon_combat:hit', function(hit)
    if hit.victim.kind ~= 'player' then return end
    local hp = MyHealth.Remove(hit.victim.src, hit.final)
    if hp <= 0 then exports.avalon_combat:React(hit.victim.src, 'death') end
end)
```

**Eventos server** (`AddEventHandler`, recebem o golpe): `avalon_combat:hit` (modo `'external'`), `avalon_combat:blocked`, `avalon_combat:parried`, `avalon_combat:guardBroken`.

---

### Exports Client-Side

| Export | Devolve |
|--------|---------|
| `GetStyle()` | Estilo atual ou `nil` |
| `IsBlocking()` | `boolean` |
| `IsAttacking()` | `boolean` |
| `GetGuard()` | `{ v, max }` pontos de guarda (modo `'points'`) |
| `GetStamina()` | Stamina do jogo, tal como a lê o modo `'stamina'` |
| `SetCombatEnabled(on)` | `false` deixa os comandos ao jogo (menu, cutscene) até voltar a `true` |
| `GetCastPoint()` | `vector3` de onde partem as magias do cajado, ou `nil` |
| `GetInputMode()` | `'combo'` ou `'keys'` |
| `MakeFighter(ped, estilo, opts)` / `StopFighter(ped)` | Transforma um ped em lutador, ou pára-o |

**Evento local** `avalon_combat:onHurt` `(ped, info, attacker)`: a cada golpe recebido por um ped gerido por este client, para um HUD.

```lua
-- client: barra de vida e números de dano
AddEventHandler('avalon_combat:onHurt', function(ped, info, attacker)
    if ped ~= PlayerPedId() then return end
    if info.parried then
        ShowText('DEFESA PERFEITA!')
    elseif info.blocked then
        ShowText(('-%d (defendido)'):format(info.damage))
    else
        ShowText(('-%d'):format(info.damage))
    end
end)

-- client: sem combate enquanto o menu está aberto
exports.avalon_combat:SetCombatEnabled(false)
```
