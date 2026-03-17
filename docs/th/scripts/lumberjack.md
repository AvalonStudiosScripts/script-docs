# avalon_lumberjack

สคริปต์**งานตัดไม้**ใน FiveM ผู้เล่นโค่นต้นไม้ด้วยมินิเกมแบบสะสมคะแนน พร้อมระบบความทนทานและโบนัสทักษะ

**เวอร์ชัน:** 1.0 | **ผู้พัฒนา:** Avalon

---

## การตั้งค่า

```lua
Lang = "ไทย"

Config.scoreGame  = true
Config.scoreToWin = 50  -- คะแนนที่ต้องการเพื่อชนะ

Config.Axe = {
    ["money"]      = { propModel = "prop_tool_fireaxe", BonusLoot = 2, BonusScore = 1 },
    ["tool_consaw"]= { propModel = "prop_tool_consaw",  BonusLoot = 3, BonusScore = 2 },
}

Config.durabilitySystemEnabled = true
Config.MaxDurability            = 200
Config.Durability               = 15  -- ความทนทานที่ลดต่อการโค่น

Config.Loot = {
    { name = "raw_material_wood", label = "ไม้",    chance = 20, scoreGame = {min=1, max=5} },
    { name = "rubber",            label = "ยาง",    chance = 5,  scoreGame = {min=1, max=5} },
    { name = "fibers",            label = "เส้นใย", chance = 8,  scoreGame = {min=1, max=5} },
}
```

---

## Export ที่ใช้งานได้

!!! info "ไม่มี export ที่ลงทะเบียน"
    `avalon_lumberjack` ไม่เปิดเผย export สาธารณะ
