# avalon_party

FiveM **MMORPG 风格组队系统**。创建带有实时 HUD 的队伍，支持自定义状态条、准备检查、共享路点、自动观战模式、地图光标与头顶标记、目标标记系统以及私有副本（routing bucket）。

**版本：** 1.0.0 | **作者：** Avalon | **框架：** ESX / QBCore / QBox / Standalone

---

## 依赖项

| Resource | 是否必须 |
|----------|:-------:|
| ox_lib | ✅ |
| ox_target | ✅ |

---

## 安装

```
ensure ox_lib
ensure ox_target
ensure avalon_party
```

框架自动识别：`es_extended` → ESX，`qbx_core` → QBox，`qb-core` → QBCore，否则为 standalone。

---

## 语言

所有文本位于 `locales/<代码>.lua`。内置：**英语、意大利语、葡萄牙语、西班牙语、俄语、简体中文、泰语**。

```lua
-- config.lua（请保持此行在文件顶部）
Config.locale = 'zh'   -- en | it | pt | es | ru | zh | th
```

翻译中缺失的键会自动回退到英语。新增语言：复制 `locales/en.lua`，重命名（例如 `de.lua`），翻译内容，把 `Locales['en']` 改为 `Locales['de']`，并设置 `Config.locale = 'de'`。

---

## 命令

| 命令 | 参数 | 说明 |
|------|------|------|
| `/party` | — | 打开队伍菜单 |
| `/partyinvite` | `[玩家ID]` | 邀请玩家入队 |
| `/partyaccept` | — | 接受邀请 |
| `/partydecline` | — | 拒绝邀请 |
| `/partyleave` | — | 离开队伍 |
| `/partydisband` | — | 解散队伍（队长） |
| `/partykick` | `[玩家ID]` | 踢出成员（队长） |
| `/partytransfer` | `[玩家ID]` | 转让队长（队长） |
| `/partyready` | — | 发起准备检查（队长） |
| `/partywp` | — | 共享路点（队长） |

无需已有队伍：第一次邀请即创建队伍。用 **ox_target** 瞄准其他玩家会出现 **"邀请入队"** 选项（3 米内）。

---

## 按键

| 按键 | 操作 |
|------|------|
| **F5** | 打开队伍菜单 |
| **F1** / **F2** | 接受 / 拒绝邀请 |
| **F3** / **F4** | 准备 / 未准备（准备检查） |
| **TAB** | 循环切换附近目标（需在菜单中开启） |
| **CTRL**（按住） | 标记准星，仅队长——右键点击 ped 打开符号选择 |
| **← →** | 观战模式下切换观战对象 |

默认值在 `Config.Keys` 中；每位玩家都可以在 FiveM 按键设置里重新绑定。

---

## 队伍菜单（F5）

NUI 界面，包含：成员列表（队长可踢人和转让队长）、离开/解散队伍、发起准备检查、共享路点、TAB 目标与符号标记开关、每条状态条与状态图标的显示开关、地图光标与 3D 头顶标记的显示开关，以及 `0.5` 到 `3.0` 的 HUD 缩放。

!!! tip "设置会保留"
    菜单中的选择是**按玩家**保存的，存放在 NUI 的 `localStorage` 中：不产生服务器流量，也不需要数据库。

---

## 配置

| 选项 | 默认值 | 说明 |
|------|--------|------|
| `Config.locale` | `'en'` | 文本语言 |
| `Config.maxPartySize` | `6` | 队伍人数上限 |
| `Config.inviteExpiry` | `30000` | 邀请过期时间（毫秒） |
| `Config.offlineGracePeriod` | `120000` | 掉线成员保留位置的时间（毫秒） |
| `Config.readyCheckTimeout` | `15000` | 准备检查持续时间（毫秒） |

```lua
Config.Features = {
    distance   = true,  -- 成员卡片上显示距离（米）
    readyCheck = true,  -- 准备检查
    waypoint   = true,  -- 共享路点
    spectator  = true,  -- 死亡后自动观战
    targetMark = true,  -- 用符号标记目标
}

Config.UI = {
    position       = 'top-left',  -- top-left | top-right | bottom-left | bottom-right
    pickClick      = 'right',     -- 选择成员使用的鼠标键
    hudWidth       = 260,
    scale          = 1.0,         -- 1.0 = 按分辨率自动缩放
    showStatuses   = true,
    updateInterval = 500,         -- 状态刷新间隔（毫秒）
    criticalPct    = 20,          -- 低于该百分比时状态条闪红
}
```

保持 `scale = 1.0` 时 HUD 会自动适配：2560 px 起为 `1.5`，3440 px 起为 `1.6`，3840 px 起为 `2.0`。

### 生命值与耐力

两者都不经过服务器：每个客户端直接从 GTA 原生函数读取所有已加载成员的数值。超出流送范围（约 200 米）时保留最后已知值，并标记为过期。

```lua
Config.Health = {
    displayMode     = 'usable',  -- 'usable' | 'raw' | 'percent'
    syncMaxHealth   = true,      -- 通过 state bag 在客户端之间共享最大生命值
    applyMaxToClone = true,      -- 提高克隆 ped 的生命值上限
    autoDeadStatus  = true,      -- 生命值归零时自动开启 'dead' 状态
    deadStatusId    = 'dead',
}
```

| `displayMode` | 显示效果 |
|---------------|----------|
| `usable` | 男性 `87/100`，女性 `62/75` —— 生命值减去死亡阈值 |
| `raw` | `187/200` 和 `162/175` —— 原生数值 |
| `percent` | 所有人都是 `87/100` |

### 世界元素与目标标记

`Config.World` 控制地图光标（`showBlips`、`blipSprite`、`blipColorMember`、`blipColorLeader`、`blipScale`）和头顶标记（`showHeadMarker`、`headMarkerDist`、`headMarkerOffsetZ`、`headMarker`）。这些只是默认值：每位玩家都可以在菜单中关闭。

`Config.TargetMark` 用于调整 TAB 检测范围、符号颜色与大小以及准星图标（`ctrlIcon`，任意 Font Awesome 类名）。可用符号定义在 `Config.TargetSymbols`：`warning` ⚠️、`target` 🎯、`attack` ❌、`defend` 🛡️、`heal` 💊、`focus` ⭐。

### 状态条

`builtin = true` 的条由 GTA 原生函数自动更新。`builtin = false` 的条在你的脚本用 `SetMemberBar` 发送数值之前**保持隐藏**。

| ID | 标签 | Builtin | 颜色 |
|----|------|:-------:|------|
| `health` | HP | ✅ | 红色 |
| `stamina` | 耐力 | ✅ | 绿色 |
| `mana` | 魔法 | ❌ | 蓝色 |
| `shield` | 护盾 | ❌ | 灰色 |
| `rage` | 怒气 | ❌ | 橙色 |

```lua
-- config.lua，写在 Config.Bars 里
{
    id = 'energy', label = '能量', color = '#f39c12', bgColor = '#3d2200',
    icon = '⚡', defaultMax = 100, builtin = false, visible = true,
},
```

!!! tip "用自己的系统接管生命值"
    把 `health`（或 `stamina`）条设为 `builtin = false`：资源不再写入该条，数值改为由你通过 `SetMemberBar` 提供。

### 状态图标

`combat` ⚔️ · `poisoned` ☠️ · `buffed` ✨ · `dead` 💀 · `afk` 💤 —— 用 `SetMemberStatus` 开关，只有 `dead` 在 `Config.Health.autoDeadStatus = true` 时由资源自动处理。

```lua
-- config.lua，写在 Config.Statuses 里
{ id = 'stunned', label = '眩晕', icon = '💫', color = '#9b59b6', duration = 0 },
```

---

## 可用导出函数

### 服务端

#### `IsInParty(source)` → `boolean`
```lua
if exports['avalon_party']:IsInParty(source) then
    -- 玩家在队伍中
end
```

#### `GetPartyMembers(source)` → `table | nil`
获取队伍所有成员的服务器 ID 列表（含队长），不在队伍时返回 `nil`。
```lua
local members = exports['avalon_party']:GetPartyMembers(source)
if members then
    for _, id in ipairs(members) do
        exports.ox_inventory:AddItem(id, 'health_potion', 1)
    end
end
```

#### `GetPartyLeader(source)` → `number | nil`
```lua
local leader = exports['avalon_party']:GetPartyLeader(source)
```

#### `SetMemberBar(source, barId, value, maxValue)` → `boolean`
更新 HUD 中的状态条，队伍全员实时可见。对 `builtin = true` 的条会被拒绝。

```lua
RegisterNetEvent('magic:cast')
AddEventHandler('magic:cast', function(cost)
    local src = source
    local newMana = math.max(0, GetPlayerMana(src) - cost)
    exports['avalon_party']:SetMemberBar(src, 'mana', newMana, 100)
end)
```

#### `SetMemberStatus(source, statusId, active)` → `boolean`
显示或隐藏 HUD 上的状态图标。

```lua
exports['avalon_party']:SetMemberStatus(source, 'poisoned', true)
exports['avalon_party']:SetMemberStatus(source, 'poisoned', false)
```

#### `SetPartyBucket(source, bucket)` → `boolean`
把整支队伍移入 routing bucket（私有副本）。bucket 与队伍绑定：之后加入的成员会自动跟随，离队的成员回到 bucket 0。无需队长权限。

```lua
RegisterNetEvent('dungeon:enter')
AddEventHandler('dungeon:enter', function(id)
    local src = source
    local ok = exports['avalon_party']:SetPartyBucket(src, 1000 + id)
    if not ok then
        TriggerClientEvent('ox_lib:notify', src, { type='error', description='需要先组队！' })
    end
end)
```

#### `GetPartyBucket(source)` → `number`
```lua
local bucket = exports['avalon_party']:GetPartyBucket(source)  -- 0 = 主世界
```

---

### 客户端

#### `RequestMemberPick(mode, callbackEvent)`
开启成员选择模式：鼠标指针出现在队伍卡片上，玩家点击其中一名成员。

- `mode` `"self"` → 事件发给选择者 · `"target"` → 事件发给被选中的成员
- 回调收到 `{ id, name, bars, statuses, isLeader, offline, picker }`

!!! warning "取消时不触发回调"
    按 **ESC** 或调用 `CancelMemberPick()` 时**不会**触发事件。点击所用的鼠标键由 `Config.UI.pickClick` 决定。

```lua
RegisterCommand('heal_ally', function()
    exports['avalon_party']:RequestMemberPick('self', 'healing:targetPicked')

    SetTimeout(10000, function()
        exports['avalon_party']:CancelMemberPick()
    end)
end)

RegisterNetEvent('healing:targetPicked')
AddEventHandler('healing:targetPicked', function(member)
    if member.offline then return end
    TriggerServerEvent('healing:apply', member.id, 50)
end)
```

#### `CancelMemberPick()`
取消进行中的成员选择，不触发任何事件。

```lua
exports['avalon_party']:CancelMemberPick()
```

#### `GetPartyTarget()` → `{ ped, netId, source, isPlayer } | nil`
返回通过 **TAB** 循环选中的 ped。若目标是 NPC，则 `source` 为 `nil`。

```lua
local target = exports['avalon_party']:GetPartyTarget()
if target and target.isPlayer then
    TriggerServerEvent('combat:attack', target.source)
end
```

!!! tip "参考文件"
    资源内含 `example_integration.lua`：该文件不会被加载，仅作为可直接复制的示例集合。
