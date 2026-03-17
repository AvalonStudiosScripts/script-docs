# AvalonStudios เอกสารสคริปต์

ยินดีต้อนรับสู่เอกสารอย่างเป็นทางการของสคริปต์ FiveM จาก **AvalonStudios**

ที่นี่คุณจะพบคู่มืออ้างอิงครบถ้วนสำหรับสคริปต์ทั้งหมด: การตั้งค่า, ฟังก์ชัน export ที่ใช้งานได้ และตัวอย่างการใช้งาน

---

## สคริปต์ที่มีให้ใช้

| สคริปต์ | คำอธิบาย | Export |
|---------|-----------|:------:|
| [avalon_coltivation](scripts/coltivation.md) | ระบบปลูกพืช | ✅ |
| [avalon_dustman](scripts/dustman.md) | งานเก็บขยะ (เดี่ยว/กลุ่ม) | ❌ |
| [avalon_electrician](scripts/electrician.md) | งานช่างไฟฟ้า (มีมินิเกม) | ❌ |
| [avalon_lumberjack](scripts/lumberjack.md) | งานตัดไม้ (มีมินิเกม) | ❌ |
| [avalon_mining](scripts/mining.md) | งานขุดแร่ (มีมินิเกม) | ❌ |
| [avalon_robbery](scripts/robbery.md) | ระบบปล้นร้านค้า | ❌ |
| [avalon_trash](scripts/trash.md) | ระบบค้นหาในขยะ | ❌ |

---

## ไลบรารีที่จำเป็น

สคริปต์ทั้งหมดต้องการ resource เหล่านี้ที่ทำงานอยู่:

- **[ox_lib](https://github.com/overextended/ox_lib)** — ไลบรารีพื้นฐาน
- **[ox_target](https://github.com/overextended/ox_target)** — ระบบปฏิสัมพันธ์
- **[ox_inventory](https://github.com/overextended/ox_inventory)** — ระบบกระเป๋า
- **[es_extended](https://github.com/esx-framework/esx_core)** — ESX Framework

!!! tip "คำแนะนำ"
    ตรวจสอบให้แน่ใจว่า dependency ทั้งหมดเริ่มทำงาน **ก่อน** สคริปต์ Avalon ใน `server.cfg`

---

## วิธีใช้ Export

Export ช่วยให้สคริปต์อื่นเรียกใช้ฟังก์ชันจากสคริปต์ Avalon โดยไม่ต้องพึ่งพาโค้ดโดยตรง

=== "Client (ฝั่งลูกค้า)"

    ```lua
    exports['ชื่อ-resource']:ชื่อฟังก์ชัน(พารามิเตอร์1, พารามิเตอร์2)
    ```

=== "Server (ฝั่งเซิร์ฟเวอร์)"

    ```lua
    exports['ชื่อ-resource']:ชื่อฟังก์ชัน(พารามิเตอร์1, พารามิเตอร์2)
    ```

!!! warning "สำคัญ"
    Export ของ **client** เรียกได้จากสคริปต์ฝั่งลูกค้าเท่านั้น
    Export ของ **server** เรียกได้จากสคริปต์ฝั่งเซิร์ฟเวอร์เท่านั้น

---

*เอกสารโดย AvalonStudios — [GitHub](https://github.com/AvalonStudiosScripts)*
