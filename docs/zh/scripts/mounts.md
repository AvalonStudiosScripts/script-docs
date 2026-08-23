# avalon_mounts

FiveM **骑乘生物**脚本。玩家可以通过命令或物品背包召唤、骑乘和管理奇幻生物（龙、狮鹫、飞马、马、羊驼）。

**版本：** 1.0 | **作者：** Mino

---

## 依赖项

| 资源 | 是否必须 | 说明 |
|------|:-------:|------|
| ox_lib | ✅ | 通知和工具 |
| ox_inventory | ❌ | 仅当 `CONFIG.items = true` 时需要 |
| ox_fuel | ❌ | 自动保持燃料最大值 |
| qbx_core | ❌ | QBox 钥匙集成 |

---

## 配置

### 基本设置

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `CONFIG.command` | `boolean` | `true` | 启用命令召唤生物 |
| `CONFIG.commandName` | `string` | `"mount"` | 召唤命令名称 (`/mount`) |
| `CONFIG.delCommand` | `string` | `"delMount"` | 解散命令名称 (`/delMount`) |
| `CONFIG.items` | `boolean` | `true` | 使用背包物品召唤 |
| `CONFIG.invincible` | `boolean` | `false` | 使生物无敌 |
| `CONFIG.maxrangedespawn` | `number` | `200.0` | 自动消失距离 |
| `CONFIG.timeForSpawnAfterDeaths` | `number` | `10000` | 死亡后等待时间 (ms) |

### 按键

| 选项 | 默认按键 | 描述 |
|------|:--------:|------|
| `CONFIG.keyMount` | `51` (E) | 上/下马 |
| `CONFIG.keyDelete` | `74` (H) | 解散生物 |
| `CONFIG.keyFlymode` | `73` (X) | 切换飞行模式 |

### 飞行系统

```lua
CONFIG.flyWithTime = false   -- false = 无限飞行
CONFIG.flyTimeMax  = 60000   -- 飞行时间 (ms)
CONFIG.flyCooldown = 60000   -- 冷却时间 (ms)
```

---

## 可用生物

| 生物 | 变体数量 | 能飞 |
|------|:--------:|:----:|
| `necrodragon`（死亡龙） | 5 | ✅ |
| `griffon`（狮鹫） | 3 | ✅ |
| `pegaso`（飞马） | 3 | ✅ |
| `horse`（马） | 3 | ❌ |
| `alpaca`（羊驼） | 3 | ❌ |

---

## 可用导出函数

### `API` — 客户端

```lua
local MountsAPI = exports['avalon_mounts']:API()

-- 召唤死亡龙（变体1）
MountsAPI.spawn("necrodragon", 1)

-- 检查是否有活跃的坐骑
if MountsAPI.isSpawned() then
    print("玩家有一个活跃的坐骑")
end

-- 解散坐骑
MountsAPI.despawn()
```

#### API 函数列表

| 函数 | 描述 |
|------|------|
| `spawn(name, color)` | 召唤生物 |
| `despawn()` | 解散活跃生物 |
| `isSpawned()` | 是否有生物被召唤？ |
| `isOnMount()` | 玩家是否正在骑乘？ |
| `getVehicle()` | 获取生物车辆实体 |
| `getData(model)` | 获取特定生物数据 |
| `getAllData()` | 获取所有生物数据 |
| `syncInvisible(ent, bool)` | 同步所有客户端的可见性 |

### 动态物品导出

```lua
exports['avalon_mounts']:necrodragon_1()   -- 召唤死亡龙变体1
exports['avalon_mounts']:necrodragon_2()   -- 变体2
exports['avalon_mounts']:griffon_1()
exports['avalon_mounts']:pegaso_1()
exports['avalon_mounts']:horse_1()
exports['avalon_mounts']:alpaca_3()
```
