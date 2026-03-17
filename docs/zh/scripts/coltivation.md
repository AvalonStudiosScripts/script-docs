# avalon_coltivation

FiveM **植物种植**脚本。允许玩家在有效地形上种植、浇水、施肥和收获各种农作物。

**版本：** 1.0 | **作者：** Mino | **数据库：** ✅ MySQL

---

## 依赖项

| 资源 | 是否必须 |
|------|:-------:|
| ox_lib | ✅ |
| oxmysql | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## 安装

```
ensure avalon_coltivation
```

---

## 配置

### 基本设置

| 选项 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `CONFIG.MIN_PLANT_DISTANCE` | `number` | `1.5` | 两株植物之间的最小距离 |
| `CONFIG.MIN_STAGE_TIME` | `number` | `300000` | 进入下一生长阶段的最短时间（毫秒） |
| `CONFIG.StressMax` | `number` | `1000000` | 可以种植时的最大压力值 |
| `CONFIG.StressValue` | `number` | `100` | 每次种植增加的压力值 |

### 植物列表

| 植物 | 种子 | 生长时间 | 收获 | 需要浇水 |
|------|------|----------|------|:--------:|
| `grain`（小麦） | `seme_grano` | 30 分钟 | 5x `grano` | ✅ |
| `canapa`（大麻） | `seme_canapa` | 45 分钟 | 3x `canapa` | ✅ |
| `coca`（古柯） | `seme_coca` | 60 分钟 | 2x `coca` | ✅ |
| `fungus`（蘑菇） | `seme_fungo` | 20 分钟 | 4x `funghi` | ❌ |
| `papavero`（罂粟） | `seme_papavero` | 50 分钟 | 2x `papavero` | ✅ |

### 工具

| 工具 | 物品 | 时间倍率 | 一次性 | 适用阶段 |
|------|------|:--------:|:------:|----------|
| 浇水壶 | `watering_can` | -25% | ❌ | seed, small |
| 肥料 | `fertilizer` | -40% | ✅ | small, grown |
| 生长灯 | `grow_lamp` | -15% | ✅ | seed, small, grown |

---

## 可用导出函数

!!! info "类型：客户端"
    这些导出函数在**客户端**注册，必须从客户端脚本调用。

### `seme_grano`
开始放置小麦种子。
```lua
exports['avalon_coltivation']:seme_grano()
```

### `seme_canapa`
开始放置大麻种子。
```lua
exports['avalon_coltivation']:seme_canapa()
```

### `seme_coca`
开始放置古柯种子。
```lua
exports['avalon_coltivation']:seme_coca()
```

### `seme_fungo`
开始放置蘑菇种子。
```lua
exports['avalon_coltivation']:seme_fungo()
```

### `seme_papavero`
开始放置罂粟种子。
```lua
exports['avalon_coltivation']:seme_papavero()
```

---

## 使用示例

```lua
-- 在其他脚本的 client/main.lua 中
RegisterCommand('种植小麦', function()
    exports['avalon_coltivation']:seme_grano()
end)
```
