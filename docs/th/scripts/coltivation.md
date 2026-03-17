# avalon_coltivation

สคริปต์**ปลูกพืช**ใน FiveM ช่วยให้ผู้เล่นสามารถปลูก รดน้ำ ใส่ปุ๋ย และเก็บเกี่ยวพืชได้บนพื้นที่ที่รองรับ

**เวอร์ชัน:** 1.0 | **ผู้พัฒนา:** Mino | **ฐานข้อมูล:** ✅ MySQL

---

## ความต้องการ

| Resource | จำเป็น |
|----------|:------:|
| ox_lib | ✅ |
| oxmysql | ✅ |
| ox_target | ✅ |
| ox_inventory | ✅ |

---

## การติดตั้ง

```
ensure avalon_coltivation
```

---

## การตั้งค่า

### ตัวเลือกทั่วไป

| ตัวเลือก | ประเภท | ค่าเริ่มต้น | คำอธิบาย |
|----------|--------|------------|----------|
| `CONFIG.MIN_PLANT_DISTANCE` | `number` | `1.5` | ระยะห่างขั้นต่ำระหว่างพืชสองต้น |
| `CONFIG.MIN_STAGE_TIME` | `number` | `300000` | เวลาขั้นต่ำ (ms) สำหรับการเติบโตแต่ละขั้น |
| `CONFIG.StressMax` | `number` | `1000000` | ค่าความเครียดสูงสุดในการปลูก |
| `CONFIG.StressValue` | `number` | `100` | ค่าความเครียดที่เพิ่มทุกครั้งที่ปลูก |

### รายการพืช

| พืช | เมล็ด | เวลาเติบโต | ผลผลิต | ต้องการน้ำ |
|-----|------|-----------|---------|:---------:|
| `grain` (ข้าวสาลี) | `seme_grano` | 30 นาที | 5x `grano` | ✅ |
| `canapa` (กัญชง) | `seme_canapa` | 45 นาที | 3x `canapa` | ✅ |
| `coca` (โคคา) | `seme_coca` | 60 นาที | 2x `coca` | ✅ |
| `fungus` (เห็ด) | `seme_fungo` | 20 นาที | 4x `funghi` | ❌ |
| `papavero` (ฝิ่น) | `seme_papavero` | 50 นาที | 2x `papavero` | ✅ |

### เครื่องมือ

| เครื่องมือ | ไอเทม | ตัวคูณเวลา | ใช้ครั้งเดียว | ขั้นที่ใช้ได้ |
|----------|------|:----------:|:------------:|------------|
| บัวรดน้ำ | `watering_can` | -25% | ❌ | seed, small |
| ปุ๋ย | `fertilizer` | -40% | ✅ | small, grown |
| โคมปลูก | `grow_lamp` | -15% | ✅ | seed, small, grown |

---

## Export ที่ใช้งานได้

!!! info "ประเภท: Client-Side"
    Export เหล่านี้ลงทะเบียนในฝั่ง **client** ต้องเรียกใช้จากสคริปต์ client เท่านั้น

### `seme_grano`
เริ่มวางเมล็ดข้าวสาลี
```lua
exports['avalon_coltivation']:seme_grano()
```

### `seme_canapa`
เริ่มวางเมล็ดกัญชง
```lua
exports['avalon_coltivation']:seme_canapa()
```

### `seme_coca`
เริ่มวางเมล็ดโคคา
```lua
exports['avalon_coltivation']:seme_coca()
```

### `seme_fungo`
เริ่มวางเมล็ดเห็ด
```lua
exports['avalon_coltivation']:seme_fungo()
```

### `seme_papavero`
เริ่มวางเมล็ดฝิ่น
```lua
exports['avalon_coltivation']:seme_papavero()
```

---

## ตัวอย่างการใช้งาน

```lua
-- client/main.lua ในสคริปต์ของคุณ
RegisterCommand('ปลูกข้าวสาลี', function()
    exports['avalon_coltivation']:seme_grano()
end)
```
