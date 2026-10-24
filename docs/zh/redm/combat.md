# avalon_combat

RedM **近战战斗系统**。每种风格（狼人、吸血鬼、太刀、剑盾、拳击手、法师……）都使用 Avalon 动画包中的动画。命中由脚本计算：每一帧都会沿着动画跟踪手、脚和刀刃，并命中其经过的 ped。伤害始终经由服务端处理。

包含：轻/重连招、配对攻击（两个 ped 同步动画）、格挡与反击、手持武器与盾牌、冲刺、闪避、扑跃以及生物的飞行、独立战斗的 NPC，以及用于对接等级、技能、生命条和法术的导出函数。

**版本：** beta | **作者：** Avalon | **游戏：** RedM | **框架：** standalone

---

## 依赖项

| Resource | 是否必须 | 用途 |
|----------|:---:|---|
| OneSync | ✅ | 战斗 NPC 与格挡（state bag） |
| `avalon_weapons_rdr2` | ❌ | 可选：附带配置中使用的武器模型。你已有的武器同样可用（[武器与盾牌](#weapons)） |
| `avalon_shields_tint` | ❌ | 可选：附带配置中使用的盾牌模型。你已有的盾牌同样可用 |
| 动画包 `avalon_anims_*` | ✅ 至少一个 | 动画与风格（参见[风格](#styles)） |

无需任何框架：脚本直接作用于 ped 和玩家。与背包、等级或生命条的对接通过[导出函数](#exports)完成。

---

## 安装

1. 将 `avalon_combat/` 文件夹复制到 `resources/`
2. 先启动动画包以及你的武器和盾牌模型所在的资源，然后再启动本资源：

```
# ... 你的武器和盾牌所在的资源
ensure avalon_anims_werewolf
ensure avalon_anims_vampire
ensure avalon_anims_ka_02
# ... 你使用的其他动画包
ensure avalon_combat
```

!!! info "动画包的启动顺序"
    在 `avalon_combat` 之后启动（或重启）的动画包同样会被读取：其风格会在它启动后立即加入。

---

## 风格 {#styles}

风格来自**动画包**。每个动画包都带有一个 `avalon_combat.lua` 文件，其中包含自己的风格，并在其 `fxmanifest.lua` 中声明。`avalon_combat` 通过该动画包的 `CombatStyles` 导出函数自动收集这些风格。

| 风格 | 动画包 | 武器 | 防御 | 额外功能 |
|------|--------|------|------|----------|
| `werewolf` | `avalon_anims_werewolf` | 利爪 | 游戏自带（空手） | 冲刺、闪避、扑跃、配对攻击 |
| `vampire` | `avalon_anims_vampire` | 利爪 | 自定义 | 飞行、加速、配对攻击 |
| `succube` | `avalon_anims_succube` | 利爪 | 游戏自带（空手） | 飞行、加速、配对攻击 |
| `katana` `katana1` `katana3` `katana4` | `avalon_anims_ka_02` `_01` `_03` `_04` | `xur_katana` | 游戏自带（刀刃） | |
| `sword` `sword2` `sword3` | `avalon_anims_sw_01` `_02` `_03` | `xur_machete2` | 游戏自带（刀刃） | |
| `sword_shield` `sword_shield2` `sword_shield3` | `avalon_anims_sns_01` `_02` `_03` | `swn2craft` + 盾牌 | 自定义 / 自定义 / 游戏自带 | |
| `twinsword` | `avalon_anims_twinsword01` | 双手各持 `xur_katana2` | 游戏自带（刀刃） | |
| `hammer2h` | `avalon_anims_hammer2h` | `hammermoore` | 游戏自带（刀刃） | |
| `orientalmonk` | `avalon_anims_orientalmonk` | `giga` | 游戏自带（刀刃） | |
| `pugilist` | `avalon_anims_p_01` | 空手 | 游戏自带（空手） | |
| `firemage` `mage` `witch` `wizard` | `avalon_anims_firemage_01` `_mage_01` `_witch_01` `_wizard` | 双手 / 法杖 | 游戏自带（空手） | |

!!! warning "法术"
    法师风格目前以双手命中，射程更长（就像法术那样）。法杖仅作视觉效果：法术将来的发射点可通过客户端导出函数 `GetCastPoint()` 读取。

如需编写你自己的风格，参见[创建风格](#create-style)。

---

## 命令与按键

!!! warning "仅在调试模式下可用"
    所有聊天命令只有在 `config/config.lua` 中设置 `AvalonCombat.Debug = true` 时才存在：它们用于配置和测试风格、武器与 NPC。正式服务器请保持 `Debug = false`，并在你自己的脚本中通过服务端导出 `SetStyle` 让玩家进入战斗。按键始终可用。

### 战斗模式

| 命令 | 说明 |
|------|------|
| `/avalon_combat <风格>` | 以该风格进入战斗（不带参数则列出所有风格） |
| `/avalon_combat off` | 退出战斗 |

| 按键 | 操作 |
|------|------|
| **鼠标左键** | 轻连招（攻击过程中再次按下即可打出下一击） |
| **鼠标右键** | 重连招 |
| **R** | 对面前的 ped 发动配对攻击（拥有配对攻击的风格），否则为特殊连招 |
| **CTRL**（按住） | 防御 |
| **E** | 锁定镜头瞄准的 ped（头顶出现红色菱形）：攻击和配对攻击都会指向它。再按一次 E 解除锁定 |
| **Shift** | 冲刺（狼人）· 飞行（飞行风格，空中） |
| **Alt** | 闪避（狼人）· 加速（飞行风格） |
| **Shift + 空格** | 扑跃（狼人）· 起飞与降落（飞行风格） |

!!! info "谁会对攻击做出反应"
    轻击和重击只造成伤害。特殊连招、配对攻击以及带有 `react = true` 的攻击还会让受害者踉跄或倒地。

### 测试与调整命令

| 命令 | 说明 |
|------|------|
| `/avalon_attack <id>` | 执行当前风格的一次攻击（id 或 clip 名称） |
| `/avalon_pair [名称]` | 对面前的 ped 执行配对攻击 |
| `/avalon_pair_offset <名称> <x> <y> <yaw>` | 移动配对攻击中受害者的位置（仅限本客户端） |
| `/avalon_weapon <名称\|off>` | 更换右手武器 |
| `/avalon_offhand <名称\|off>` | 更换左手武器 |
| `/avalon_shield <名称\|off>` | 更换盾牌 |
| `/avalon_weapon_offset <x y z rx ry rz> [骨骼]` | 调整武器位置，并输出可复制到配置中的那一行（`_offhand_offset`、`_shield_offset` 同理） |
| `/avalon_sound <名称>` | 播放配置中的一个音效 |
| `/avalon_sound_game <set> <名称> [bank]` | 试听游戏中的任意音效 |
| `/avalon_npc <风格> [team] [模型] [scale]` | 在前方 4 米处生成一个战斗 NPC |
| `/avalon_npc_clear` | 清除你创建的 NPC |
| `/avalon_npc_info` | 30 米内战斗 NPC 的状态 |
| `/avalon_dummy [模型]` · `/avalon_dummy_clear` | 固定不动的木桩，用于击打 |
| `/avalon_hitbox` | 显示 hitbox：身体（绿色）、肢体（黄色）、刀刃（蓝色）、命中（红色） |
| `/avalon_combat_debug` | 绘制手部移动轨迹并输出每一次命中 |

---

## 伤害如何计算

1. 攻击者的客户端每一帧都会沿着动画检查肢体和刀刃。肢体速度低于 `MinSweepSpeed` 时不会命中（蓄力与收招阶段）。
2. 当肢体或刀刃接触到 ped 的 hitbox 时，客户端会通知服务端。
3. 服务端校验该次命中（距离、最大伤害、攻击是否已解锁），计算伤害与防御，并执行其他资源的 *hit handler*。
4. 根据 `AvalonCombat.DamageMode`：
    - `'native'` — 伤害直接作用于 ped 的真实生命值
    - `'external'` — 不造成伤害：服务端触发 `avalon_combat:hit` 事件，交由你的系统处理（虚拟生命条、抗性）

    无论哪种模式，受害者都会做出反应。

**公式：** 基础伤害（命中刀刃时为武器伤害，否则为风格的 `damage`）× 攻击的 `mult` × 成长倍率 × 输出倍率，四舍五入。然后再扣除防御的减伤。

---

## 配置

配置文件位于 `config/`：

| 文件 | 内容 |
|------|------|
| `config/config.lua` | 常规设置、连招、NPC、音效、伤害、防御、输入 |
| `config/guard.lua` | 可按名称使用的游戏自带防御（`'blade'`、`'unarmed'`） |
| `config/weapons.lua` | 右手武器 |
| `config/offhand.lua` | 左手武器 |
| `config/shields.lua` | 盾牌（每种颜色一项） |
| `config/styles/` | 可选的本地风格，以及逐字段说明的 `example.lua` |

!!! info "可修改的文件"
    `config/`（以及 `config/styles/`）中的文件是开放的：资源的其余部分由 Cfx Escrow 保护。所有自定义都通过这些文件和[导出](#exports)完成。

### 常规设置

| 选项 | 默认值 | 说明 |
|------|--------|------|
| `Debug` | `false` | 启用聊天命令和 `/avalon_combat_debug`（仅用于测试） |
| `TargetRange` | `4.0` | 米：更远的 ped 不会被检测 |
| `LockRange` / `LockCone` | `3.0` / `45` | 攻击会转向此米数和角度内最近的 ped |
| `AttackTowardsCamera` | `true` | 玩家的攻击（以及 R 的配对攻击）朝镜头所指的方向：每次攻击开始时角色会转向那里。`false`：朝角色面向的方向 |
| `LockOnRange` | `12.0` | 米：按 E 锁定的范围 |
| `MinSweepSpeed` | `2.0` | 米/秒：低于此速度的肢体不会命中 |
| `SkipStart` | `0.10` | 每次攻击开头被忽略的部分 |
| `Rehit` | `0.45` | 秒：同一次攻击再次命中同一目标前的间隔 |
| `MaxHitDamage` | `200` | 服务端会拒绝超过此伤害的命中 |
| `MaxHitDistance` | `6.0` | 服务端会拒绝距离超过此值的玩家命中 |
| `PairRange` | `3.5` | 米：配对攻击的最大抓取距离 |
| `HitboxScale` / `HitboxPad` | `1.0` / `0.0` | 放大刀刃的 hitbox（更容易命中） |
| `SoundVolume` | `0.35` | 音频文件的音量 |
| `DamageMode` | `'native'` | `'native'` 或 `'external'`（见上文） |

### 连招

```lua
AvalonCombat.ComboWindow = 0.45 -- 从攻击的这个阶段起，按键会将下一击加入队列
AvalonCombat.ComboCancel = 1.0  -- 队列中的下一击开始的阶段（1.0 = 动画结束时）
AvalonCombat.ComboReset = 0.8   -- 秒：停顿超过此时间，连招从第一击重新开始
```

### 输入模式

```lua
AvalonCombat.InputMode = 'combo' -- 'combo' | 'keys'
AvalonCombat.Keys = { SimulWindow = 0.08, SequenceWindow = 0.6, DirAbove = 0.5 }
```

- `'combo'`：每个按键执行其连招链中的下一击（`light`、`heavy`、`special`）
- `'keys'`：类似格斗游戏，按键执行风格的 `moves`。可以组合同时按下的按键（`'light+heavy'`）、方向（`'F+heavy'`：向前 + 重击）或序列（`{ 'light', 'light', 'heavy' }`）

每位玩家都可以通过导出函数 `SetInputMode` 使用不同的模式。

### 防御与格挡

```lua
AvalonCombat.Defence = {
    Mode = 'always',      -- 'always' | 'timed' | 'stamina' | 'points'
    ParryWindow = 0.25,   -- 秒：在受击前这段时间内举起防御 = 格挡
    StaminaCost = 15.0,   -- 'stamina'：每次格挡扣除的耐力
    Points = 100, PointsRegen = 10, PointsCost = 20, -- 'points'：防御点数
    BreakTime = 1.5,      -- 秒：防御被破后无法防御的时间
    Block = { percent = 0.75, flat = 0 }, -- 防御的基础减伤
}
```

| `Mode` | 防御何时挡住攻击 |
|--------|------------------|
| `always` | 始终，伤害按防御值减免 |
| `timed` | 仅在举起防御后的 `ParryWindow` 内，否则攻击全额命中 |
| `stamina` | 始终，每次攻击消耗耐力；耐力耗尽则防御被破 |
| `points` | 始终，每次攻击消耗防御点数（导出函数 `GetGuard` / `SetGuard`）；归零则防御被破 |

**格挡**会抵消伤害并使攻击者踉跄。**防御** = `伤害 × (1 - percent) - flat`，不会低于零。手持盾牌时，盾牌自身的防御会取代防御动作的防御值；通过导出函数 `SetDefence` 设置的防御则会叠加。

### 音效

每个音效在 `AvalonCombat.Sounds` 中都有一个名称。风格、攻击、武器和配对攻击都使用这些名称：可以是一个名称、一个随机抽取其一的列表，或按连招链区分的 `{ light = ..., heavy = ..., special = ... }`。

```lua
AvalonCombat.Sounds = {
    punch = { file = melee('punch', 11) },                 -- sounds/ 中的文件（.wav .ogg .mp3）
    my_roar = { file = 'roar.ogg', volume = 0.8, range = 40 },
    beast_roar = { set = 'LSHNT_sounds', name = 'ROAR' },  -- 游戏音效
}
```

内置的近战音效来自游戏本身，已提取到 `sounds/melee_*.wav`。

!!! tip "在恰当的时机播放音效"
    `soundAt` 让音效在动画的精确阶段播放：`0.35` = clip 的 35% 处。适用于攻击、配对攻击以及风格的移动动作（`soundAt = { leap = 0.27, dodge = 0.15 }`）。没有 `soundAt` 时，音效会在第一次快速挥动（攻击）时或立即播放。

### 身体 hitbox

`AvalonCombat.BodyHitbox` 列出了每个 ped 被命中时使用的胶囊体（`{ 起始骨骼, 结束骨骼, 半径 }`）：头部、胸部、腹部、手臂和腿。缺少这些骨骼的动物会变成半径 0.4 米的球体。`/avalon_hitbox` 可在游戏中显示它们。

---

## 武器与盾牌 {#weapons}

武器和盾牌使用你服务器自己的：`model` 可以是任何已 stream 的 prop 名称，只要其所在资源已启动。附带的配置列出了 `avalon_weapons_rdr2` 和 `avalon_shields_tint` 的模型：可以替换成你自己的，或继续添加。

`config/weapons.lua`、`config/offhand.lua` 和 `config/shields.lua` 中的每一项都是一个手持模型：

```lua
AvalonCombat.Weapons.xur_katana = {
    model = 'weapon_xur_katana',
    bone = 'SKEL_R_Hand',                         -- 附着的骨骼
    offset = vector3(0.075, 0.00, -0.03),         -- 米
    rot = vector3(-90.0, 0.0, 0.0),               -- 度
    damage = 25,                                  -- 每次刀刃命中，× 攻击的 mult
    sounds = { swing = 'blade_swipe', hit = { 'machete_slash', 'blade_flesh' } },
    blade = { from = vector3(0, 0, 0.12), to = vector3(0, 0, 1.05), radius = 0.08 }, -- 刀刃的 hitbox
}
```

| 字段 | 说明 |
|------|------|
| `model` | 模型的 archetype：任何已 stream 的 prop |
| `tint` | 可选：颜色索引（盾牌每种颜色一项） |
| `bone`、`offset`、`rot` | 握持的位置。可在游戏中用 `/avalon_weapon_offset` 调整，它会输出可复制的那一行 |
| `damage` | 每次刀刃命中的伤害 |
| `sounds` | 刀刃的 `swing` 与 `hit` |
| `blade` | 刀刃在模型空间中的胶囊体：由它代替手部进行命中 |
| `cast` | 仅限法杖：法术将来的发射点 |
| `defence` | 仅限盾牌：用 `{ percent, flat }` 取代防御动作的防御值 |

### 来自背包的武器

风格与武器是分开的：每种风格都可以手持配置中的任何武器，因此多种风格可以共用同一把武器。在你的脚本中为玩家和 NPC 选择武器（例如根据使用的物品）和风格（例如根据职业）：

```lua
-- 服务端：物品决定武器，职业决定风格
local ItemWeapons = { katana_item = 'xur_katana', machete_item = 'xur_machete2' }

RegisterNetEvent('my_inventory:useWeapon', function(item)
    local src = source
    local weapon = ItemWeapons[item]
    if not weapon then return end
    -- 不同风格使用同一把武器
    local style = MyClasses.IsMaster(src) and 'katana3' or 'katana'
    exports.avalon_combat:SetStyle(src, style, { weapon = weapon, shield = false })
end)

-- 只换武器，风格不变
exports.avalon_combat:SetHeld(src, 'weapon', 'xur_machete2')

-- NPC：用 { net = netId } 同样操作
local net = exports.avalon_combat:SpawnFighter('katana', coords, 90.0, { weapon = 'xur_machete2' })
exports.avalon_combat:SetStyle({ net = net }, 'katana4', { weapon = 'xur_machete2' })
```

---

## 创建风格 {#create-style}

风格可以写在两个位置：

=== "在动画包中"

    Avalon 动画包采用的方式。在动画包的 `fxmanifest.lua` 旁边放置 `avalon_combat.lua`：

    ```lua
    local Styles = {}

    Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
        block = 'blade',   -- 动画包以字符串形式指定游戏自带的防御
    }

    exports('CombatStyles', function() return Styles end)
    ```

    并在动画包的 `fxmanifest.lua` 中加入：

    ```lua
    avalon_combat_styles 'yes'
    shared_script 'avalon_combat.lua'
    ```

=== "在 config/styles 中"

    在 `avalon_combat` 内创建文件 `config/styles/mystyle.lua`：

    ```lua
    AvalonCombat.Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
    }
    ```

    写在这里的风格**优先于**同名的动画包风格：若要修改动画包中的某个风格，请将其复制到这里。

`config/styles/example.lua` 包含一个完整的风格，并逐字段加了注释。

### 风格的字段

| 字段 | 说明 |
|------|------|
| `dict` | 动画字典（`.ycd`） |
| `damage` | 身体一次攻击的伤害（拳、脚、利爪） |
| `idle` | 站立时循环播放的动画 |
| `weapon` / `offhand` / `shield` | 手持的物品（`Weapons`、`Offhand`、`Shields` 的键） |
| `hands` | 用于命中的肢体：`{ 起始骨骼, 结束骨骼, 射程 }` |
| `reach` / `radius` | 超出末端骨骼的射程，以及肢体的粗细（米） |
| `light` / `heavy` / `special` | 连招链：攻击列表 |
| `moves` | `'keys'` 模式的招式 |
| `sounds` | `swing`、`hit`、`knockdown`、`block`、`leap`、`dodge`、`boost`、`float` |
| `soundAt` | 移动动作音效播放的阶段 |
| `block` | 防御：带有自有 clip 的 `{ start, loop, stop, hit }`，或 `'blade'` / `'unarmed'`。没有 `block` 的风格无法格挡 |
| `stagger` / `knockdown` | 受击反应的 clip |
| `move` | 特殊移动：`kind = 'runner'`（冲刺、闪避、扑跃）或 `kind = 'flyer'`（飞行、加速） |
| `pairs` | 配对攻击 |

### 一次攻击

```lua
light = {
    { clip = 'attack@slash1', mult = 1.0 },
    { clip = 'attack@slash2', mult = 1.25, knockdown = true, soundAt = 0.35 },
},
```

| 字段 | 说明 |
|------|------|
| `clip` | 动画 |
| `mult` | 伤害倍率 |
| `knockdown` | 受害者倒地 |
| `react` | 即使不是特殊攻击，受害者也会做出反应 |
| `sound` / `soundAt` | 攻击自己的音效及其播放的阶段 |
| `keepTurn` | ped 保持动画结束时的朝向 |
| `locked` | 在成长系统解锁之前被锁定（`SetAttackEnabled`） |
| `id` | 供导出函数使用的攻击名称（默认：clip） |

### 配对攻击

```lua
pairs = {
    bite = { att = 'bite@att', vic = 'bite@vic', x = 0.5, y = 2.8, yaw = 196,
             mult = 4.0, damageAt = 0.45, sound = 'beast_roar', soundAt = 0.2 },
},
```

`att` / `vic` 分别是攻击者和受害者的 clip。`x`、`y` 和 `yaw` 表示受害者相对于攻击者的位置（米、度）。`damageAt` 是伤害生效的阶段。可在游戏中用 `/avalon_pair_offset` 调整。

!!! info "动画位移"
    Avalon 动画包的 clip 会按照自身的动作移动 ped（步伐、冲刺、跳跃）。第三方动画包的 clip 会在原地播放。

---

## 战斗 NPC

战斗 NPC 是一个网络 ped，会以某种风格独立战斗：寻找目标、靠近、连招攻击、格挡、闪避、跳跃或飞行。

- **Team：** 玩家的 team 是 `'player'`。NPC 会攻击玩家（除非它的 team 是 `'player'`）和其他 team 的 NPC，绝不会攻击普通 ped。
- **技能：** `config/config.lua` 中的 `AvalonCombat.Npc` 包含默认值（仇恨范围、攻击距离、格挡概率、闪避、配对攻击……）。每个 NPC 可通过 `opts.skill` 修改其中一部分。

```lua
-- server
local net = exports.avalon_combat:SpawnFighter('werewolf', vector3(-300.0, 790.0, 118.0), 90.0, {
    team = 'bandits',
    health = 400,
    scale = 1.5,
    skill = { BlockChance = 0.6, Weights = { heavy = 0.6 } },
})

-- 稍后
exports.avalon_combat:StopFighter(net)
```

| `opts` | 说明 |
|--------|------|
| `model` | ped 的模型（默认 `AvalonCombat.Npc.Model`） |
| `weapon` / `offhand` / `shield` | 手持的物品：配置中的键（默认：风格自带的；`false` = 空手）。不存在的键会导致生成失败 |
| `team` | Team（默认 `'npc'`） |
| `health` | 生命值 |
| `scale` | NPC 的大小（`1` = 正常，范围 `0.2` 到 `5`）。命中框、攻击范围、动画位移和战斗距离都会随之放大 |
| `skill` | 仅填写 `AvalonCombat.Npc` 中需要更改的值 |

!!! warning "需要有玩家在线"
    `SpawnFighter` 会在离该坐标最近的玩家的客户端上创建 ped：服务器没有玩家时返回 `nil`。

---

## 成长（等级与技能） {#progression}

avalon_combat **没有**等级或技能系统：那是你服务器自己的系统。本脚本只提供工具，让你为每个玩家或战斗 NPC 决定可以使用哪些攻击、强度如何。你的系统调用导出，avalon_combat 负责执行。

### 可以使用哪些攻击

- 每个攻击默认**可用**，除非它在风格中的条目写了 `locked = true`：那样它对所有人一开始都是锁定的。
- `SetAttackEnabled(target, 风格, attackId, true | false)` 为该战斗者和该风格解锁或锁定一个攻击，优先于配置中的 `locked`。
- `attackId` 是攻击的 `id`，没有则为其动画片段名。配对攻击则是它在 `pairs` 中的键。`GetAttacks(风格)` 列出一个风格的所有攻击及其 id、连段和 `locked`。
- 连招模式的连段（`light`、`heavy`、`special`）和 `'keys'` 模式的 `moves` 是不同的条目，id 也不同：如果服务器两种模式都用，两边都要解锁。
- 被锁定的攻击会被跳过：连招中按键会跳到该连段的下一个可用攻击（没有可用的则按键无效）；`'keys'` 模式下不会选中该招式；R 不会发动配对攻击。NPC 只使用自己可用的攻击。
- 服务器会丢弃被锁定攻击的命中：修改过的客户端也无法使用。

### 强度如何

| 导出 | 效果 |
|------|------|
| `SetAttackMultiplier(target, 风格, attackId, m)` | 单个攻击的伤害倍率 |
| `SetOutgoingMultiplier(target, m)` | 其全部伤害的倍率 |
| `SetDefence(target, { percent, flat })` | 自身防御，叠加在格挡或盾牌的防御上 |
| `SetMaxGuard(target, max)` | 最大格挡点数（`'points'` 模式） |
| `SetInputMode(target, 'combo' \| 'keys')` | 战斗者的输入模式 |
| `SetStyle(target, 风格, 武器)` · `SetHeld(target, slot, 键)` | 玩家和 NPC 的风格与武器（例如来自职业或背包） |

每个 `Set...` 都有对应的 `Get...` 用于读取当前值。

!!! warning "不会保存任何数据"
    成长数据只保存在内存中：玩家退出（或 NPC 停止）后会恢复为配置中的值。保存它是你的系统的工作，并在每次进入时重新发送，例如在角色加载完成时。

```lua
-- server: 你的等级系统
local Unlocks = { -- 每个风格中每个攻击的最低等级
    katana = { ['combo@a03@rm'] = 5, ['combo@a04@rm'] = 10, ['combo@d03@rm'] = 15 },
}

local function applyProgress(src, level)
    for style, attacks in pairs(Unlocks) do
        for attackId, minLevel in pairs(attacks) do
            exports.avalon_combat:SetAttackEnabled(src, style, attackId, level >= minLevel)
        end
    end
    exports.avalon_combat:SetOutgoingMultiplier(src, 1.0 + level * 0.02) -- 每级 +2% 伤害
end

AddEventHandler('mylevels:loaded', applyProgress)  -- 角色已加载
AddEventHandler('mylevels:levelUp', applyProgress) -- 升级
```

---

## 可用导出 {#exports}

`target` 是玩家的服务端 ID，或对于战斗 NPC 为 `{ net = netId }`。

### 服务端导出

| 导出函数 | 说明 |
|----------|------|
| `SetStyle(target, 风格 \| nil, 武器)` | 让战斗者以该风格进入战斗。`武器`：`{ weapon, offhand, shield }`，每项为配置中的键，`false` 表示空手，省略则使用风格自带的。`nil` 让玩家退出战斗（NPC 用 `StopFighter` 移除） |
| `GetStyle(target)` | 战斗者当前的风格 |
| `SetHeld(target, slot, 键 \| nil)` | 手持的武器（`'weapon'`、`'offhand'`）或盾牌（`'shield'`），风格保持不变 |
| `GetHeld(target)` | `{ slot = 键 }` |
| `SpawnFighter(风格, coords, heading, opts)` | 创建战斗 NPC，返回 net id |
| `StopFighter(net)` | 移除战斗 NPC |
| `AddHitHandler(fn)` / `RemoveHitHandler(id)` | 每次命中时调用的函数（见下文） |
| `GetDamageMode()` | `'native'` 或 `'external'` |
| `React(target, 反应)` | 让目标执行 `'stagger'`、`'knockdown'` 或 `'death'` |
| `SetAttackEnabled(target, 风格, attackId, on)` / `IsAttackEnabled(...)` | 为该战斗者解锁或锁定一个攻击 |
| `SetAttackMultiplier(target, 风格, attackId, m)` / `GetAttackMultiplier(...)` | 某个攻击的倍率 |
| `SetOutgoingMultiplier(target, m)` / `GetOutgoingMultiplier(target)` | 其全部伤害的倍率 |
| `SetDefence(target, { percent, flat })` / `GetDefence(target)` | 自身的防御，与防御动作的防御值叠加 |
| `GetGuard` / `SetGuard` / `GetMaxGuard` / `SetMaxGuard` | 防御点数（`'points'` 模式） |
| `SetInputMode(target, 'combo' \| 'keys' \| nil)` / `GetInputMode(target)` | 战斗者的输入模式 |
| `GetStyles()` | 所有已加载的风格 |
| `GetAttacks(风格)` | `{ { id, chain, clip, mult, locked, input } }` |
| `GetWeapon(键)` / `GetShield(键)` | 配置中的条目 |

#### 命中（`hit`）

hit handler 和 `avalon_combat:hit` 事件接收的表如下：

| 字段 | 说明 |
|------|------|
| `mode` | `'native'` 或 `'external'` |
| `attacker` | `{ kind = 'player' \| 'fighter', src, net, style, team }` |
| `victim` | `{ kind = 'player' \| 'fighter' \| 'ped', src, net, style }` |
| `attack` | `{ id, chain, clip, mult, knockdown, react }`（`chain`：`light`、`heavy`、`special`、`moves`、`pair`） |
| `weapon` | 命中刀刃时为 `{ slot, key, data }`，否则为 `nil` |
| `base` | 武器或风格的伤害 |
| `attackMult`, `progressMult`, `outgoingMult` | 三个倍率 |
| `damage` | 防御之前的伤害 |
| `guard` | `{ blocked, parried, broken, percent, flat, cost }` |
| `final` | 防御之后的伤害（`'native'` 模式下实际应用的伤害） |
| `direction` | 从攻击者指向受害者的 `{ x, y }` |

handler 在防御计算之后执行。handler 返回修改后的命中表，返回 `nil` 表示保持不变，返回 `false` 表示取消。资源停止时，其 handler 会被自动移除。

```lua
-- server: 抗性与等级
exports.avalon_combat:AddHitHandler(function(hit)
    if hit.victim.kind == 'player' and HasSilverArmor(hit.victim.src) and hit.attacker.style == 'werewolf' then
        hit.final = hit.final * 0.5 -- 在 handler 之后由服务端四舍五入
        return hit
    end
end)
```

```lua
-- server: DamageMode = 'external'，生命值由你的系统管理
AddEventHandler('avalon_combat:hit', function(hit)
    if hit.victim.kind ~= 'player' then return end
    local hp = MyHealth.Remove(hit.victim.src, hit.final)
    if hp <= 0 then exports.avalon_combat:React(hit.victim.src, 'death') end
end)
```

**服务端事件**（`AddEventHandler`，接收命中表）：`avalon_combat:hit`（`'external'` 模式）、`avalon_combat:blocked`、`avalon_combat:parried`、`avalon_combat:guardBroken`。

---

### 客户端导出

| 导出函数 | 返回值 |
|----------|--------|
| `GetStyle()` | 当前风格或 `nil` |
| `IsBlocking()` | `boolean` |
| `IsAttacking()` | `boolean` |
| `GetGuard()` | `{ v, max }` 防御点数（`'points'` 模式） |
| `GetStamina()` | 游戏的耐力，即 `'stamina'` 模式所读取的值 |
| `SetCombatEnabled(on)` | `false` 会把操作交还给游戏（菜单、过场动画），直到再次设为 `true` |
| `GetCastPoint()` | 法杖法术发射点的 `vector3`，或 `nil` |
| `GetInputMode()` | `'combo'` 或 `'keys'` |
| `MakeFighter(ped, 风格, opts)` / `StopFighter(ped)` | 将 ped 变为战斗者，或使其停止 |

**本地事件** `avalon_combat:onHurt` `(ped, info, attacker)`：由本客户端管理的 ped 每次受击时触发，可用于 HUD。

```lua
-- client: 生命条与伤害数字
AddEventHandler('avalon_combat:onHurt', function(ped, info, attacker)
    if ped ~= PlayerPedId() then return end
    if info.parried then
        ShowText('格挡！')
    elseif info.blocked then
        ShowText(('-%d（已防御）'):format(info.damage))
    else
        ShowText(('-%d'):format(info.damage))
    end
end)

-- client: 菜单打开时不进行战斗
exports.avalon_combat:SetCombatEnabled(false)
```
