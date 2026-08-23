# avalon_mounts

สคริปต์**สัตว์ขี่**ใน FiveM ผู้เล่นสามารถเรียก ขี่ และจัดการสิ่งมีชีวิตแฟนตาซี (มังกร กริฟฟิน เพกาซัส ม้า อัลปาก้า) ผ่านคำสั่งหรือไอเทม

**เวอร์ชัน:** 1.0 | **ผู้พัฒนา:** Mino

---

## ความต้องการ

| Resource | จำเป็น | หมายเหตุ |
|----------|:------:|----------|
| ox_lib | ✅ | การแจ้งเตือนและเครื่องมือ |
| ox_inventory | ❌ | เฉพาะเมื่อ `CONFIG.items = true` |
| ox_fuel | ❌ | รักษาเชื้อเพลิงอัตโนมัติ |
| qbx_core | ❌ | การรวม QBox key |

---

## การตั้งค่า

### ตัวเลือกทั่วไป

| ตัวเลือก | ประเภท | ค่าเริ่มต้น | คำอธิบาย |
|----------|--------|-----------|----------|
| `CONFIG.command` | `boolean` | `true` | เปิดใช้คำสั่งเรียกสัตว์ |
| `CONFIG.commandName` | `string` | `"mount"` | ชื่อคำสั่งเรียก (`/mount`) |
| `CONFIG.delCommand` | `string` | `"delMount"` | ชื่อคำสั่งไล่ออก (`/delMount`) |
| `CONFIG.items` | `boolean` | `true` | ใช้ไอเทมจากกระเป๋าเพื่อเรียก |
| `CONFIG.invincible` | `boolean` | `false` | ทำให้สัตว์ไม่แพ้ตาย |
| `CONFIG.maxrangedespawn` | `number` | `200.0` | ระยะทางก่อน auto-despawn |
| `CONFIG.timeForSpawnAfterDeaths` | `number` | `10000` | เวลารอ (ms) หลังเสียชีวิต |

### ปุ่มคีย์

| ตัวเลือก | ปุ่ม | คำอธิบาย |
|----------|:----:|----------|
| `CONFIG.keyMount` | `51` (E) | ขึ้น / ลงจากสัตว์ |
| `CONFIG.keyDelete` | `74` (H) | ไล่สัตว์ออก |
| `CONFIG.keyFlymode` | `73` (X) | เปิด/ปิดโหมดบิน |

### ระบบการบิน

```lua
CONFIG.flyWithTime = false   -- false = บินได้ไม่จำกัด
CONFIG.flyTimeMax  = 60000   -- เวลาบิน (ms)
CONFIG.flyCooldown = 60000   -- เวลาฟื้นตัว (ms)
```

---

## สัตว์ที่มีให้

| สัตว์ | จำนวนรูปแบบ | บินได้ |
|-------|:-----------:|:------:|
| `necrodragon` (มังกรมืด) | 5 | ✅ |
| `griffon` (กริฟฟิน) | 3 | ✅ |
| `pegaso` (เพกาซัส) | 3 | ✅ |
| `horse` (ม้า) | 3 | ❌ |
| `alpaca` (อัลปาก้า) | 3 | ❌ |

---

## Export ที่ใช้งานได้

### `API` — Client-Side

```lua
local MountsAPI = exports['avalon_mounts']:API()

-- เรียก necrodragon (รูปแบบที่ 1)
MountsAPI.spawn("necrodragon", 1)

-- ตรวจสอบว่ามีสัตว์ที่เรียกอยู่
if MountsAPI.isSpawned() then
    print("ผู้เล่นมีสัตว์ขี่ที่ใช้งานอยู่")
end

-- ไล่สัตว์ออก
MountsAPI.despawn()
```

#### ฟังก์ชัน API

| ฟังก์ชัน | คำอธิบาย |
|----------|----------|
| `spawn(name, color)` | เรียกสัตว์ |
| `despawn()` | ไล่สัตว์ที่ใช้งานออก |
| `isSpawned()` | มีสัตว์ที่เรียกอยู่หรือไม่? |
| `isOnMount()` | ผู้เล่นกำลังขี่อยู่หรือไม่? |
| `getVehicle()` | ได้รับ entity ของสัตว์ขี่ |
| `getData(model)` | ข้อมูลสัตว์เฉพาะตัว |
| `syncInvisible(ent, bool)` | ซิงค์การมองเห็นทุก client |

### Dynamic Item Exports

```lua
exports['avalon_mounts']:necrodragon_1()
exports['avalon_mounts']:griffon_1()
exports['avalon_mounts']:pegaso_1()
exports['avalon_mounts']:horse_1()
exports['avalon_mounts']:alpaca_1()
```
