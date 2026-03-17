# avalon_robbery

สคริปต์**ปล้นร้านค้า**ใน FiveM ผู้เล่นเล็งอาวุธไปที่แคชเชียร์ NPC เพื่อเริ่มปล้น เก็บเงินทีละน้อย และเปิดตู้นิรภัยด้วยมินิเกม

**เวอร์ชัน:** 1.0 | **ผู้พัฒนา:** Mino

---

## การตั้งค่า

```lua
CONFIG.SHOPS = {
    ["shop00"] = {
        ped         = { model = "s_m_m_ammucountry", coords = vector4(-1221.71, -908.18, 12.326, 0.0) },
        lockbox     = { coords = vector4(-1221.310, -915.907, 11.096, 123.85), weight = 50000 },
        items       = { {name = "money", quantity = 1000} },
        policeNotify= "พบกิจกรรมน่าสงสัยในร้าน!",
        jobToNotify = "police",
        tools       = "stetoscopio",
        minigame    = { game = "Locked", options = nil },
        timeForRobbery  = 30000,   -- 30 วินาที
        startMoney      = 500,
        startMoneyStep  = 50,
        startMoneyDelay = 1000,
        shopResetTime   = 1800000  -- 30 นาที
    }
}
```

| คีย์ | คำอธิบาย |
|------|----------|
| `timeForRobbery` | ระยะเวลาปล้น (ms) |
| `startMoney` | เงินทั้งหมดในเครื่องเก็บเงิน |
| `startMoneyStep` | เงินที่ได้ต่อขั้นตอนแอนิเมชัน |
| `shopResetTime` | ms ก่อนที่ร้านจะรีเซ็ต |

---

## Export ที่ใช้งานได้

!!! info "ไม่มี export ที่ลงทะเบียน"
    `avalon_robbery` ไม่เปิดเผย export สาธารณะ
