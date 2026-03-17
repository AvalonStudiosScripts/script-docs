# avalon_dustman

สคริปต์**งานเก็บขยะ**ใน FiveM รองรับโหมดเดี่ยว (รถตู้เล็ก) และโหมดกลุ่ม (รถขยะ)

**เวอร์ชัน:** 1.0 | **ผู้พัฒนา:** Mino

---

## การตั้งค่า

| ตัวเลือก | ค่าเริ่มต้น | คำอธิบาย |
|----------|-----------|----------|
| `Config.Debug` | `true` | เปิดใช้ log การดีบัก |
| `Config.PartyMembers` | `4` | จำนวนสมาชิกกลุ่มสูงสุด |
| `Config.BagsPerSector` | `10` | จำนวนถุงที่ต้องเก็บเพื่อเสร็จโซน |
| `Config.StressMax` | `10000` | ค่าความเครียดสูงสุด |
| `Config.StressValue` | `100` | ความเครียดที่เพิ่มต่อการเก็บถังขยะ 1 ใบ |

### โหมดการทำงาน

=== "เดี่ยว"
    ```lua
    Config.Single = { vehicle = "utillitruck3", ticketRate = 5 }
    ```
=== "กลุ่ม"
    ```lua
    Config.Group  = { vehicle = "trash", ticketRate = 4, maxTrucks = 4 }
    ```

---

## Export ที่ใช้งานได้

!!! info "ไม่มี export ที่ลงทะเบียน"
    `avalon_dustman` ไม่เปิดเผย export สาธารณะ
