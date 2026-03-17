# avalon_trash

สคริปต์**ค้นหาในขยะ**ใน FiveM ผู้เล่นสามารถค้นหาในถังขยะ ถังขยะขนาดใหญ่ และตู้จดหมายเพื่อหาของสุ่ม

**เวอร์ชัน:** 1.0 | **ผู้พัฒนา:** Mino

---

## การตั้งค่า

```lua
CONFIG.SEARCH_TIME        = 5000    -- เวลาแถบความคืบหน้า (ms)
CONFIG.COOLDOWN           = 60      -- วินาทีก่อนค้นหาถังเดิมได้อีก
CONFIG.TRASH_REFRESH_TIME = 5 * 60  -- วินาทีก่อนของรางวัลจะสร้างใหม่
```

### หมวดหมู่ถัง

| หมวดหมู่ | ไอเทมที่ต้องการ | โบนัส |
|---------|:-----------:|-------|
| `small_bin` (ถังขยะเล็ก) | ❌ | +25% โอกาสเมื่อมี `trash_gloves` |
| `dumpster` (ถังขยะใหญ่) | `trash_gloves` | +40% โอกาส |
| `mailbox` (ตู้จดหมาย) | `crowbar` | +60% โอกาส |

### ของรางวัล

```lua
CONFIG.TRASH_LOOT = {
    small_bin = { { item = "weed",  chance = 50, amount = {1,20} } },
    dumpster  = { { item = "scrap", chance = 70, amount = {2,6}  } },
    mailbox   = { { item = "letter",chance = 80, amount = {1,2}  } },
}
```

---

## Export ที่ใช้งานได้

!!! info "ไม่มี export ที่ลงทะเบียน"
    `avalon_trash` ไม่เปิดเผย export สาธารณะ
