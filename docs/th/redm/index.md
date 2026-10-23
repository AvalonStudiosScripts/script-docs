# ภาพรวมสคริปต์ RedM

สคริปต์ RedM ของ AvalonStudios เป็นแบบ **standalone** คือไม่ต้องพึ่งพา framework และเชื่อมต่อกับระบบอื่นของเซิร์ฟเวอร์ผ่าน export และ event

---

## รายการสคริปต์

| สคริปต์ | เวอร์ชัน | Export ฝั่งไคลเอนต์ | Export ฝั่งเซิร์ฟเวอร์ | ฐานข้อมูล |
|--------|----------|:---:|:---:|:---:|
| [avalon_combat](combat.md) | beta | ✅ 10 | ✅ 28 | ❌ |

---

## Dependency ของแต่ละสคริปต์

```
avalon_combat  → OneSync, avalon_weapons_rdr2, avalon_shields_tint, แพ็กเกจแอนิเมชัน avalon_anims_*
```

!!! info "แพ็กเกจแอนิเมชัน"
    สไตล์การต่อสู้มาจากแพ็กเกจแอนิเมชันของ Avalon (`avalon_anims_*`) แต่ละแพ็กเกจมีสไตล์ของตัวเอง และ `avalon_combat` จะโหลดให้อัตโนมัติ ดู [สไตล์](combat.md#styles)
