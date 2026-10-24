# avalon_combat

ระบบ**ต่อสู้ระยะประชิด**สำหรับ RedM ทุกสไตล์ (มนุษย์หมาป่า, แวมไพร์, คาทานะ, ดาบและโล่, นักมวย, จอมเวทย์...) ใช้แอนิเมชันจากแพ็กเกจของ Avalon การโจมตีคำนวณโดยสคริปต์: ทุกเฟรมจะติดตามมือ เท้า และใบมีดไปตามแอนิเมชัน แล้วตีโดน ped ที่ผ่านเข้ามา ความเสียหายผ่านเซิร์ฟเวอร์เสมอ

ประกอบด้วย: คอมโบเบาและหนัก, ท่าโจมตีคู่ (ped สองตัวเล่นแอนิเมชันพร้อมกัน), การป้องกันและการสวนกลับ, อาวุธและโล่ที่ถือในมือ, การพุ่ง, การหลบ, การกระโดดและการบินสำหรับสิ่งมีชีวิต, NPC ที่ต่อสู้ได้เอง และ export สำหรับเชื่อมกับเลเวล สกิล แถบพลังชีวิต และเวทมนตร์

**เวอร์ชัน:** beta | **ผู้พัฒนา:** Avalon | **เกม:** RedM | **Framework:** standalone

---

## ความต้องการ

| Resource | จำเป็น | ใช้ทำอะไร |
|----------|:---:|---|
| OneSync | ✅ | NPC ที่ต่อสู้และการป้องกัน (state bag) |
| `avalon_weapons_rdr2` | ❌ | ไม่บังคับ: โมเดลอาวุธใน config ที่ให้มา อาวุธที่คุณมีอยู่แล้วก็ใช้ได้ ([อาวุธและโล่](#weapons)) |
| `avalon_shields_tint` | ❌ | ไม่บังคับ: โมเดลโล่ใน config ที่ให้มา โล่ที่คุณมีอยู่แล้วก็ใช้ได้ |
| แพ็กเกจแอนิเมชัน `avalon_anims_*` | ✅ อย่างน้อยหนึ่งชุด | แอนิเมชันและสไตล์ (ดู [สไตล์](#styles)) |

ไม่ต้องใช้ framework: สคริปต์ทำงานกับ ped และผู้เล่นโดยตรง การเชื่อมกับกระเป๋า เลเวล หรือแถบพลังชีวิตทำผ่าน [export](#exports)

---

## การติดตั้ง

1. คัดลอกโฟลเดอร์ `avalon_combat/` ไปไว้ใน `resources/`
2. เริ่มแพ็กเกจแอนิเมชันและ resource ของโมเดลอาวุธและโล่ของคุณ แล้วจึงเริ่ม resource:

```
# ... resource ของอาวุธและโล่ของคุณ
ensure avalon_anims_werewolf
ensure avalon_anims_vampire
ensure avalon_anims_ka_02
# ... แพ็กเกจอื่นที่คุณใช้
ensure avalon_combat
```

!!! info "ลำดับของแพ็กเกจ"
    แพ็กเกจที่เริ่มหลัง `avalon_combat` (หรือถูกรีสตาร์ท) ก็ยังถูกอ่านเช่นกัน: สไตล์ของแพ็กเกจจะถูกเพิ่มทันทีที่เริ่มทำงาน

---

## สไตล์ {#styles}

สไตล์มาจาก**แพ็กเกจแอนิเมชัน** แต่ละแพ็กเกจมีไฟล์ `avalon_combat.lua` ที่เก็บสไตล์ของตัวเอง และประกาศไว้ใน `fxmanifest.lua` ของแพ็กเกจ `avalon_combat` จะรวบรวมเองผ่าน export `CombatStyles` ของแพ็กเกจ

| สไตล์ | แพ็กเกจ | อาวุธ | การป้องกัน | พิเศษ |
|-------|-----------|-------|------------|-------|
| `werewolf` | `avalon_anims_werewolf` | กรงเล็บ | ของเกม (มือเปล่า) | พุ่ง, หลบ, กระโดด, ท่าโจมตีคู่ |
| `vampire` | `avalon_anims_vampire` | กรงเล็บ | ของตัวเอง | บิน, boost, ท่าโจมตีคู่ |
| `succube` | `avalon_anims_succube` | กรงเล็บ | ของเกม (มือเปล่า) | บิน, boost, ท่าโจมตีคู่ |
| `katana` `katana1` `katana3` `katana4` | `avalon_anims_ka_02` `_01` `_03` `_04` | `xur_katana` | ของเกม (ใบมีด) | |
| `sword` `sword2` `sword3` | `avalon_anims_sw_01` `_02` `_03` | `xur_machete2` | ของเกม (ใบมีด) | |
| `sword_shield` `sword_shield2` `sword_shield3` | `avalon_anims_sns_01` `_02` `_03` | `swn2craft` + โล่ | ของตัวเอง / ของตัวเอง / ของเกม | |
| `twinsword` | `avalon_anims_twinsword01` | `xur_katana2` ทั้งสองมือ | ของเกม (ใบมีด) | |
| `hammer2h` | `avalon_anims_hammer2h` | `hammermoore` | ของเกม (ใบมีด) | |
| `orientalmonk` | `avalon_anims_orientalmonk` | `giga` | ของเกม (ใบมีด) | |
| `pugilist` | `avalon_anims_p_01` | มือเปล่า | ของเกม (มือเปล่า) | |
| `firemage` `mage` `witch` `wizard` | `avalon_anims_firemage_01` `_mage_01` `_witch_01` `_wizard` | มือ / คทา | ของเกม (มือเปล่า) | |

!!! warning "เวทมนตร์"
    สไตล์จอมเวทย์ตอนนี้โจมตีด้วยมือ โดยมีระยะไกลกว่าปกติ (เหมือนที่คาถาจะทำ) คทาเป็นเพียงภาพประกอบ: จุดที่เวทมนตร์จะพุ่งออกไปอ่านได้จาก export ฝั่งไคลเอนต์ `GetCastPoint()`

หากต้องการเขียนสไตล์ของคุณเอง ดู [สร้างสไตล์](#create-style)

---

## คำสั่งและปุ่ม

!!! warning "ใช้ได้เฉพาะเมื่อเปิด debug"
    คำสั่งแชททั้งหมดจะมีอยู่ก็ต่อเมื่อตั้ง `AvalonCombat.Debug = true` ใน `config/config.lua` เท่านั้น: ใช้สำหรับตั้งค่าและทดสอบสไตล์ อาวุธ และ NPC บนเซิร์ฟเวอร์จริงให้ตั้ง `Debug = false` และให้ผู้เล่นเข้าสู่การต่อสู้จากสคริปต์ของคุณด้วย export ฝั่งเซิร์ฟเวอร์ `SetStyle` ส่วนปุ่มต่างๆ ใช้ได้เสมอ

### โหมดต่อสู้

| คำสั่ง | คำอธิบาย |
|---------|-------------|
| `/avalon_combat <สไตล์>` | เข้าสู่การต่อสู้ด้วยสไตล์นั้น (ไม่ใส่อาร์กิวเมนต์จะแสดงรายการสไตล์) |
| `/avalon_combat off` | ออกจากการต่อสู้ |

| ปุ่ม | การกระทำ |
|-------|--------|
| **คลิกซ้าย** | คอมโบเบา (กดอีกครั้งระหว่างโจมตีเพื่อต่อท่าถัดไป) |
| **คลิกขวา** | คอมโบหนัก |
| **R** | ท่าโจมตีคู่กับ ped ที่อยู่ตรงหน้า (สไตล์ที่มี) หรือคอมโบพิเศษ |
| **CTRL** (กดค้าง) | ป้องกัน |
| **E** | ล็อก ped ที่กล้องเล็งอยู่ (ข้าวหลามตัดสีแดงเหนือหัว): การโจมตีและท่าโจมตีคู่จะพุ่งไปที่เป้าหมายนั้น กดอีกครั้งเพื่อปล่อย |
| **Shift** | พุ่ง (มนุษย์หมาป่า) · บิน (สไตล์บิน, ขณะอยู่กลางอากาศ) |
| **Alt** | หลบ (มนุษย์หมาป่า) · boost (สไตล์บิน) |
| **Shift + Space** | กระโดด (มนุษย์หมาป่า) · ขึ้นบินและลงจอด (สไตล์บิน) |

!!! info "ใครมีปฏิกิริยาเมื่อโดนตี"
    การโจมตีเบาและหนักสร้างความเสียหายเท่านั้น คอมโบพิเศษ ท่าโจมตีคู่ และการโจมตีที่มี `react = true` จะทำให้เหยื่อเซหรือล้มด้วย

### คำสั่งทดสอบและปรับแต่ง

| คำสั่ง | คำอธิบาย |
|---------|-------------|
| `/avalon_attack <id>` | ใช้ท่าโจมตีของสไตล์ปัจจุบัน (id หรือชื่อ clip) |
| `/avalon_pair [ชื่อ]` | ใช้ท่าโจมตีคู่กับ ped ที่อยู่ตรงหน้า |
| `/avalon_pair_offset <ชื่อ> <x> <y> <yaw>` | ย้ายตำแหน่งเหยื่อของท่าโจมตีคู่ (เฉพาะไคลเอนต์นี้) |
| `/avalon_weapon <ชื่อ\|off>` | เปลี่ยนอาวุธมือขวา |
| `/avalon_offhand <ชื่อ\|off>` | เปลี่ยนอาวุธมือซ้าย |
| `/avalon_shield <ชื่อ\|off>` | เปลี่ยนโล่ |
| `/avalon_weapon_offset <x y z rx ry rz> [กระดูก]` | ปรับตำแหน่งอาวุธและพิมพ์บรรทัดที่ต้องคัดลอกไปใส่ใน config (มี `_offhand_offset`, `_shield_offset` ด้วย) |
| `/avalon_sound <ชื่อ>` | เล่นเสียงจาก config |
| `/avalon_sound_game <set> <ชื่อ> [bank]` | ทดลองเสียงใดก็ได้ของเกม |
| `/avalon_npc <สไตล์> [team] [โมเดล] [scale]` | สร้าง NPC นักสู้ห่างไปข้างหน้า 4 เมตร |
| `/avalon_npc_clear` | ลบ NPC ที่คุณสร้าง |
| `/avalon_npc_info` | สถานะของ NPC นักสู้ในระยะ 30 เมตร |
| `/avalon_dummy [โมเดล]` · `/avalon_dummy_clear` | หุ่นนิ่งสำหรับฝึกตี |
| `/avalon_hitbox` | แสดง hitbox: ลำตัว (เขียว), แขนขา (เหลือง), ใบมีด (ฟ้า), การโจมตี (แดง) |
| `/avalon_combat_debug` | วาดการเคลื่อนไหวของมือและพิมพ์ทุกการโจมตี |

---

## ความเสียหายทำงานอย่างไร

1. ไคลเอนต์ของผู้โจมตีตรวจแขนขาและใบมีดทุกเฟรมตามแอนิเมชัน แขนขาที่ช้ากว่า `MinSweepSpeed` จะไม่ตีโดน (ช่วงง้างและช่วงดึงกลับ)
2. เมื่อแขนขาหรือใบมีดแตะ hitbox ของ ped ไคลเอนต์จะแจ้งเซิร์ฟเวอร์
3. เซิร์ฟเวอร์ตรวจสอบการโจมตี (ระยะ ความเสียหายสูงสุด ท่าโจมตีที่ปลดล็อกแล้ว) คำนวณความเสียหายและการป้องกัน แล้วรัน *hit handler* ของ resource อื่น
4. ขึ้นอยู่กับ `AvalonCombat.DamageMode`:
    - `'native'` — ความเสียหายลงที่พลังชีวิตจริงของ ped
    - `'external'` — ไม่มีความเสียหาย: เซิร์ฟเวอร์ส่ง event `avalon_combat:hit` ให้ระบบของคุณ (แถบพลังชีวิตจำลอง, ค่าต้านทาน)

    เหยื่อยังคงเล่นปฏิกิริยาตามปกติ

**สูตร:** ความเสียหายพื้นฐาน (ของอาวุธถ้าตีด้วยใบมีด มิฉะนั้นใช้ `damage` ของสไตล์) × `mult` ของท่าโจมตี × ตัวคูณความก้าวหน้า × ตัวคูณขาออก ปัดเศษ จากนั้นหักค่าป้องกันของการป้องกัน

---

## การตั้งค่า

ไฟล์ตั้งค่าอยู่ใน `config/`:

| ไฟล์ | เนื้อหา |
|------|-----------|
| `config/config.lua` | การตั้งค่าทั่วไป, คอมโบ, NPC, เสียง, ความเสียหาย, การป้องกัน, อินพุต |
| `config/guard.lua` | การป้องกันของเกมที่เรียกใช้ด้วยชื่อ (`'blade'`, `'unarmed'`) |
| `config/weapons.lua` | อาวุธมือขวา |
| `config/offhand.lua` | อาวุธมือซ้าย |
| `config/shields.lua` | โล่ (หนึ่งรายการต่อสี) |
| `config/styles/` | สไตล์ท้องถิ่น (ไม่บังคับ) และ `example.lua` ที่อธิบายทุกฟิลด์ |

!!! info "ไฟล์ที่แก้ไขได้"
    ไฟล์ใน `config/` (และ `config/styles/`) เปิดให้แก้ไข: ส่วนที่เหลือของรีซอร์สถูกป้องกันด้วย Cfx Escrow การปรับแต่งทั้งหมดทำผ่านไฟล์เหล่านี้และ [export](#exports)

### การตั้งค่าทั่วไป

| ตัวเลือก | ค่าเริ่มต้น | คำอธิบาย |
|--------|---------|-------------|
| `Debug` | `false` | เปิดคำสั่งแชทและ `/avalon_combat_debug` (สำหรับทดสอบเท่านั้น) |
| `TargetRange` | `4.0` | เมตร: ped ที่ไกลกว่านี้จะไม่ถูกตรวจ |
| `LockRange` / `LockCone` | `3.0` / `45` | การโจมตีจะหันไปหา ped ที่ใกล้ที่สุดภายในระยะเมตรและองศาเหล่านี้ |
| `AttackTowardsCamera` | `true` | การโจมตีของผู้เล่น (และท่าคู่ของ R) จะไปทางที่กล้องชี้: ตัวละครจะหันไปทางนั้นเมื่อเริ่มแต่ละท่า `false`: ไปทางที่ตัวละครหันอยู่ |
| `LockOnRange` | `12.0` | เมตร: รัศมีการล็อกเป้าด้วย E |
| `MinSweepSpeed` | `2.0` | เมตร/วินาที: แขนขาที่ช้ากว่านี้จะไม่ตีโดน |
| `SkipStart` | `0.10` | ช่วงต้นของทุกการโจมตีที่ถูกข้าม |
| `Rehit` | `0.45` | วินาทีก่อนที่การโจมตีเดิมจะตีเป้าหมายเดิมซ้ำได้ |
| `MaxHitDamage` | `200` | เซิร์ฟเวอร์ปฏิเสธการโจมตีที่ความเสียหายเกินค่านี้ |
| `MaxHitDistance` | `6.0` | เซิร์ฟเวอร์ปฏิเสธการโจมตีผู้เล่นที่อยู่ไกลกว่านี้ |
| `PairRange` | `3.5` | เมตร: ระยะจับสูงสุดของท่าโจมตีคู่ |
| `HitboxScale` / `HitboxPad` | `1.0` / `0.0` | ขยาย hitbox ของใบมีด (ให้ตีโดนง่ายขึ้น) |
| `SoundVolume` | `0.35` | ระดับเสียงของไฟล์เสียง |
| `DamageMode` | `'native'` | `'native'` หรือ `'external'` (ดูด้านบน) |

### คอมโบ

```lua
AvalonCombat.ComboWindow = 0.45 -- ตั้งแต่เฟสนี้ของการโจมตี การกดหนึ่งครั้งจะเข้าคิวท่าถัดไป
AvalonCombat.ComboCancel = 1.0  -- เฟสที่ท่าในคิวเริ่มทำงาน (1.0 = เมื่อแอนิเมชันจบ)
AvalonCombat.ComboReset = 0.8   -- วินาที: หากหยุดนานเกินนี้ คอมโบจะเริ่มใหม่จากท่าแรก
```

### โหมดอินพุต

```lua
AvalonCombat.InputMode = 'combo' -- 'combo' | 'keys'
AvalonCombat.Keys = { SimulWindow = 0.08, SequenceWindow = 0.6, DirAbove = 0.5 }
```

- `'combo'`: แต่ละปุ่มใช้ท่าถัดไปในสายของตัวเอง (`light`, `heavy`, `special`)
- `'keys'`: แบบเกมต่อสู้ ปุ่มจะใช้ `moves` ของสไตล์ รวมปุ่มที่กดพร้อมกัน (`'light+heavy'`), ทิศทาง (`'F+heavy'`: ไปข้างหน้า + หนัก) หรือลำดับ (`{ 'light', 'light', 'heavy' }`) ได้

ผู้เล่นแต่ละคนตั้งโหมดของตัวเองได้ด้วย export `SetInputMode`

### การป้องกันและการปัดป้อง

```lua
AvalonCombat.Defence = {
    Mode = 'always',      -- 'always' | 'timed' | 'stamina' | 'points'
    ParryWindow = 0.25,   -- วินาที: ยกการป้องกันภายในเวลานี้ก่อนโดนตี = ปัดป้อง (parry)
    StaminaCost = 15.0,   -- 'stamina': สตามินาที่หักต่อการป้องกันหนึ่งครั้ง
    Points = 100, PointsRegen = 10, PointsCost = 20, -- 'points': แต้มป้องกัน
    BreakTime = 1.5,      -- วินาทีที่ป้องกันไม่ได้หลังการป้องกันแตก
    Block = { percent = 0.75, flat = 0 }, -- ค่าป้องกันพื้นฐานของการป้องกัน
}
```

| `Mode` | เมื่อใดที่การป้องกันหยุดการโจมตีได้ |
|--------|----------------------------------|
| `always` | เสมอ ความเสียหายลดลงตามค่าป้องกัน |
| `timed` | เฉพาะภายใน `ParryWindow` หลังยกการป้องกัน มิฉะนั้นโดนเต็ม |
| `stamina` | เสมอ ทุกการโจมตีเสียสตามินา เมื่อสตามินาหมดการป้องกันจะแตก |
| `points` | เสมอ ทุกการโจมตีเสียแต้มป้องกัน (export `GetGuard` / `SetGuard`) เมื่อเหลือศูนย์จะแตก |

**การปัดป้อง** ยกเลิกความเสียหายและทำให้ผู้โจมตีเซ **ค่าป้องกัน** = `damage × (1 - percent) - flat` และไม่ต่ำกว่าศูนย์ โล่ที่ถือในมือจะแทนที่ค่าป้องกันของการป้องกันด้วยค่าของตัวเอง ส่วนค่าที่ตั้งด้วย export `SetDefence` จะนำมาบวกเพิ่ม

### เสียง

ทุกเสียงมีชื่ออยู่ใน `AvalonCombat.Sounds` สไตล์ ท่าโจมตี อาวุธ และท่าโจมตีคู่ใช้ชื่อเหล่านั้น: ใส่ชื่อเดียว, รายการที่สุ่มเลือกหนึ่งอัน หรือ `{ light = ..., heavy = ..., special = ... }` แยกตามสาย

```lua
AvalonCombat.Sounds = {
    punch = { file = melee('punch', 11) },                 -- ไฟล์ใน sounds/ (.wav .ogg .mp3)
    my_roar = { file = 'roar.ogg', volume = 0.8, range = 40 },
    beast_roar = { set = 'LSHNT_sounds', name = 'ROAR' },  -- เสียงของเกม
}
```

เสียงต่อสู้ระยะประชิดที่รวมมาเป็นเสียงของเกม ที่แยกไว้ใน `sounds/melee_*.wav`

!!! tip "เสียงในจังหวะที่พอดี"
    `soundAt` ทำให้เสียงเริ่มที่เฟสที่แน่นอนของแอนิเมชัน: `0.35` = ที่ 35% ของ clip ใช้ได้กับการโจมตี ท่าโจมตีคู่ และการเคลื่อนไหวของสไตล์ (`soundAt = { leap = 0.27, dodge = 0.15 }`) หากไม่มี `soundAt` เสียงจะเริ่มเมื่อมีการเคลื่อนไหวเร็วครั้งแรก (การโจมตี) หรือเริ่มทันที

### Hitbox ของลำตัว

`AvalonCombat.BodyHitbox` ระบุแคปซูลที่ใช้ตีแต่ละ ped (`{ กระดูกเริ่ม, กระดูกจบ, รัศมี }`): หัว อก ท้อง แขน และขา สัตว์ที่ไม่มีกระดูกเหล่านั้นจะกลายเป็นทรงกลมรัศมี 0.4 เมตร `/avalon_hitbox` แสดงให้เห็นในเกม

---

## อาวุธและโล่ {#weapons}

อาวุธและโล่เป็นของเซิร์ฟเวอร์คุณเอง: `model` คือชื่อของ prop ใดก็ได้ที่ stream อยู่ ขอแค่ resource ของมันเริ่มทำงานแล้ว config ที่ให้มาระบุโมเดลของ `avalon_weapons_rdr2` และ `avalon_shields_tint`: แทนที่ด้วยของคุณหรือเพิ่มเติมได้

ทุกรายการใน `config/weapons.lua`, `config/offhand.lua` และ `config/shields.lua` คือโมเดลที่ถือในมือ:

```lua
AvalonCombat.Weapons.xur_katana = {
    model = 'weapon_xur_katana',
    bone = 'SKEL_R_Hand',                         -- กระดูกที่ยึดอาวุธไว้
    offset = vector3(0.075, 0.00, -0.03),         -- เมตร
    rot = vector3(-90.0, 0.0, 0.0),               -- องศา
    damage = 25,                                  -- ต่อการฟันหนึ่งครั้ง × mult ของท่าโจมตี
    sounds = { swing = 'blade_swipe', hit = { 'machete_slash', 'blade_flesh' } },
    blade = { from = vector3(0, 0, 0.12), to = vector3(0, 0, 1.05), radius = 0.08 }, -- hitbox ของใบมีด
}
```

| ฟิลด์ | คำอธิบาย |
|-------|-------------|
| `model` | archetype ของโมเดล: prop ใดก็ได้ที่ stream อยู่ |
| `tint` | ไม่บังคับ: ดัชนีสี (โล่มีหนึ่งรายการต่อสี) |
| `bone`, `offset`, `rot` | ตำแหน่งในมือ ปรับในเกมด้วย `/avalon_weapon_offset` ซึ่งจะพิมพ์บรรทัดที่ต้องคัดลอก |
| `damage` | ความเสียหายต่อการฟันหนึ่งครั้ง |
| `sounds` | `swing` และ `hit` ของใบมีด |
| `blade` | แคปซูลของใบมีดในพื้นที่ของโมเดล: ใช้ตีแทนมือ |
| `cast` | เฉพาะคทา: จุดที่เวทมนตร์จะพุ่งออกไป |
| `defence` | เฉพาะโล่: `{ percent, flat }` แทนของการป้องกัน |

### อาวุธจากกระเป๋า

สไตล์และอาวุธแยกกัน: ทุกสไตล์ถืออาวุธใดก็ได้ใน config ดังนั้นหลายสไตล์จึงใช้อาวุธเดียวกันได้ จากสคริปต์ของคุณ เลือกอาวุธ (เช่น จากไอเทมที่ใช้) และสไตล์ (เช่น จากคลาส) ได้ทั้งผู้เล่นและ NPC:

```lua
-- เซิร์ฟเวอร์: ไอเทมเลือกอาวุธ คลาสเลือกสไตล์
local ItemWeapons = { katana_item = 'xur_katana', machete_item = 'xur_machete2' }

RegisterNetEvent('my_inventory:useWeapon', function(item)
    local src = source
    local weapon = ItemWeapons[item]
    if not weapon then return end
    -- สไตล์ต่างกันใช้อาวุธเดียวกัน
    local style = MyClasses.IsMaster(src) and 'katana3' or 'katana'
    exports.avalon_combat:SetStyle(src, style, { weapon = weapon, shield = false })
end)

-- เปลี่ยนแค่อาวุธ สไตล์คงเดิม
exports.avalon_combat:SetHeld(src, 'weapon', 'xur_machete2')

-- NPC: ทำแบบเดียวกันด้วย { net = netId }
local net = exports.avalon_combat:SpawnFighter('katana', coords, 90.0, { weapon = 'xur_machete2' })
exports.avalon_combat:SetStyle({ net = net }, 'katana4', { weapon = 'xur_machete2' })
```

---

## สร้างสไตล์ {#create-style}

เขียนสไตล์ได้สองที่:

=== "ในแพ็กเกจแอนิเมชัน"

    วิธีที่แพ็กเกจของ Avalon ใช้ วางไฟล์ `avalon_combat.lua` ไว้ข้าง `fxmanifest.lua` ของแพ็กเกจ:

    ```lua
    local Styles = {}

    Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
        block = 'blade',   -- แพ็กเกจระบุการป้องกันของเกมเป็น string
    }

    exports('CombatStyles', function() return Styles end)
    ```

    และใน `fxmanifest.lua` ของแพ็กเกจ:

    ```lua
    avalon_combat_styles 'yes'
    shared_script 'avalon_combat.lua'
    ```

=== "ใน config/styles"

    ไฟล์ `config/styles/mystyle.lua` ภายใน `avalon_combat`:

    ```lua
    AvalonCombat.Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
    }
    ```

    สไตล์ที่เขียนที่นี่ **มีผลเหนือกว่า** สไตล์ของแพ็กเกจที่ชื่อเดียวกัน: หากต้องการแก้สไตล์ของแพ็กเกจ ให้คัดลอกมาไว้ที่นี่

`config/styles/example.lua` มีสไตล์ครบชุดพร้อมคอมเมนต์อธิบายทีละฟิลด์

### ฟิลด์ของสไตล์

| ฟิลด์ | คำอธิบาย |
|-------|-------------|
| `dict` | dictionary ของแอนิเมชัน (`.ycd`) |
| `damage` | ความเสียหายของการโจมตีด้วยร่างกายหนึ่งครั้ง (หมัด เตะ กรงเล็บ) |
| `idle` | แอนิเมชันวนซ้ำตอนยืนนิ่ง |
| `weapon` / `offhand` / `shield` | สิ่งที่ถือ (คีย์ของ `Weapons`, `Offhand`, `Shields`) |
| `hands` | แขนขาที่ใช้ตี: `{ กระดูกเริ่ม, กระดูกจบ, ระยะ }` |
| `reach` / `radius` | ระยะที่เลยกระดูกสุดท้ายออกไป และความหนาของแขนขา (เมตร) |
| `light` / `heavy` / `special` | สายคอมโบ: รายการท่าโจมตี |
| `moves` | ท่าของโหมด `'keys'` |
| `sounds` | `swing`, `hit`, `knockdown`, `block`, `leap`, `dodge`, `boost`, `float` |
| `soundAt` | เฟสที่เสียงของการเคลื่อนไหวเริ่มเล่น |
| `block` | การป้องกัน: `{ start, loop, stop, hit }` ที่มี clip ของตัวเอง หรือ `'blade'` / `'unarmed'` หากไม่มี `block` สไตล์นั้นจะป้องกันไม่ได้ |
| `stagger` / `knockdown` | clip ปฏิกิริยาเมื่อโดนตี |
| `move` | การเคลื่อนไหวพิเศษ: `kind = 'runner'` (พุ่ง หลบ กระโดด) หรือ `kind = 'flyer'` (บิน boost) |
| `pairs` | ท่าโจมตีคู่ |

### ท่าโจมตี

```lua
light = {
    { clip = 'attack@slash1', mult = 1.0 },
    { clip = 'attack@slash2', mult = 1.25, knockdown = true, soundAt = 0.35 },
},
```

| ฟิลด์ | คำอธิบาย |
|-------|-------------|
| `clip` | แอนิเมชัน |
| `mult` | ตัวคูณความเสียหาย |
| `knockdown` | เหยื่อล้มลง |
| `react` | เหยื่อมีปฏิกิริยาแม้ไม่ใช่ท่าพิเศษ |
| `sound` / `soundAt` | เสียงเฉพาะของท่าโจมตี และเฟสที่เสียงเริ่มเล่น |
| `keepTurn` | ped คงทิศทางที่แอนิเมชันหมุนไว้ |
| `locked` | ถูกล็อกจนกว่าความก้าวหน้าจะปลดล็อก (`SetAttackEnabled`) |
| `id` | ชื่อของท่าโจมตีสำหรับ export (ค่าเริ่มต้น: ชื่อ clip) |

### ท่าโจมตีคู่

```lua
pairs = {
    bite = { att = 'bite@att', vic = 'bite@vic', x = 0.5, y = 2.8, yaw = 196,
             mult = 4.0, damageAt = 0.45, sound = 'beast_roar', soundAt = 0.2 },
},
```

`att` / `vic` คือ clip ของผู้โจมตีและของเหยื่อ `x`, `y` และ `yaw` บอกตำแหน่งของเหยื่อเทียบกับผู้โจมตี (เมตร, องศา) `damageAt` คือเฟสที่ความเสียหายเกิดขึ้น ปรับในเกมด้วย `/avalon_pair_offset`

!!! info "การเคลื่อนที่ของแอนิเมชัน"
    clip ของแพ็กเกจ Avalon จะขยับ ped ตามการเคลื่อนไหวของมันเอง (ก้าว พุ่ง กระโดด) ส่วน clip ของแพ็กเกจจากผู้อื่นจะเล่นอยู่กับที่

---

## NPC นักสู้

NPC นักสู้คือ ped เครือข่ายที่ต่อสู้ได้เองด้วยสไตล์หนึ่ง: มองหาเป้าหมาย เข้าใกล้ โจมตีต่อเนื่องเป็นคอมโบ ป้องกัน หลบ กระโดด หรือบิน

- **Team:** ผู้เล่นคือ `'player'` NPC จะโจมตีผู้เล่น (เว้นแต่ team ของมันคือ `'player'`) และ NPC ของ team อื่น ไม่โจมตี ped ปกติ
- **สกิล:** `AvalonCombat.Npc` ใน `config/config.lua` เก็บค่าเริ่มต้น (รัศมีความก้าวร้าว ระยะโจมตี โอกาสป้องกัน หลบ ท่าโจมตีคู่...) NPC แต่ละตัวเปลี่ยนบางค่าได้ด้วย `opts.skill`

```lua
-- server
local net = exports.avalon_combat:SpawnFighter('werewolf', vector3(-300.0, 790.0, 118.0), 90.0, {
    team = 'bandits',
    health = 400,
    scale = 1.5,
    skill = { BlockChance = 0.6, Weights = { heavy = 0.6 } },
})

-- ภายหลัง
exports.avalon_combat:StopFighter(net)
```

| `opts` | คำอธิบาย |
|--------|-------------|
| `model` | โมเดลของ ped (ค่าเริ่มต้น `AvalonCombat.Npc.Model`) |
| `weapon` / `offhand` / `shield` | สิ่งที่ถือ: คีย์ใน config (ค่าเริ่มต้น: ของสไตล์; `false` = มือเปล่า) คีย์ที่ไม่มีอยู่จะทำให้การสร้างล้มเหลว |
| `team` | Team (ค่าเริ่มต้น `'npc'`) |
| `health` | พลังชีวิต |
| `scale` | ขนาดของ NPC (`1` = ปกติ ตั้งแต่ `0.2` ถึง `5`) hitbox ระยะโจมตี การเคลื่อนที่ของแอนิเมชัน และระยะการต่อสู้จะขยายตามไปด้วย |
| `skill` | เฉพาะค่าของ `AvalonCombat.Npc` ที่ต้องการเปลี่ยน |

!!! warning "ต้องมีผู้เล่น"
    `SpawnFighter` สร้าง ped บนไคลเอนต์ของผู้เล่นที่อยู่ใกล้พิกัดมากที่สุด: หากเซิร์ฟเวอร์ว่างจะคืนค่า `nil`

---

## ความก้าวหน้า (เลเวลและสกิล) {#progression}

avalon_combat **ไม่มี**ระบบเลเวลหรือสกิล: ส่วนนั้นเป็นของเซิร์ฟเวอร์คุณเอง สคริปต์นี้ให้เพียงเครื่องมือสำหรับกำหนดว่าผู้เล่นหรือ NPC นักสู้แต่ละตัวใช้การโจมตีใดได้บ้างและแข็งแกร่งแค่ไหน ระบบของคุณเรียก export แล้ว avalon_combat จะนำไปใช้

### ใช้การโจมตีใดได้บ้าง

- ทุกการโจมตี**ใช้ได้**ตามค่าเริ่มต้น เว้นแต่รายการในสไตล์จะมี `locked = true`: แล้วจะเริ่มต้นแบบล็อกสำหรับทุกคน
- `SetAttackEnabled(target, สไตล์, attackId, true | false)` ปลดล็อกหรือล็อกการโจมตีสำหรับนักสู้และสไตล์นั้น และมีผลเหนือ `locked` ใน config
- `attackId` คือ `id` ของการโจมตี ถ้าไม่มีคือชื่อคลิป สำหรับการโจมตีคู่คือคีย์ใน `pairs` ส่วน `GetAttacks(สไตล์)` จะแสดงการโจมตีทั้งหมดของสไตล์พร้อม id, ชุดคอมโบ และ `locked`
- ชุดคอมโบ (`light`, `heavy`, `special`) และ `moves` ของโหมด `'keys'` เป็นรายการแยกกัน มี id ต่างกัน: ถ้าเซิร์ฟเวอร์ใช้ทั้งสองโหมด ต้องปลดล็อกทั้งสองฝั่ง
- การโจมตีที่ถูกล็อกจะถูกข้าม: ในคอมโบปุ่มจะไปยังการโจมตีถัดไปที่ใช้ได้ของชุดนั้น (ไม่มีเลย: ปุ่มไม่ทำอะไร) ในโหมด `'keys'` ท่านั้นจะไม่ถูกเลือก และ R จะไม่เริ่มการโจมตีคู่ NPC ใช้เฉพาะการโจมตีที่ใช้ได้ของตัวเอง
- เซิร์ฟเวอร์จะทิ้งการโดนของการโจมตีที่ถูกล็อก: ไคลเอนต์ที่ถูกดัดแปลงก็ยังใช้ไม่ได้

### แข็งแกร่งแค่ไหน

| Export | ผล |
|--------|----|
| `SetAttackMultiplier(target, สไตล์, attackId, m)` | ตัวคูณความเสียหายของการโจมตีหนึ่ง |
| `SetOutgoingMultiplier(target, m)` | ตัวคูณความเสียหายทั้งหมดของมัน |
| `SetDefence(target, { percent, flat })` | การป้องกันของตัวเอง บวกเพิ่มจากการป้องกันของการกันหรือโล่ |
| `SetMaxGuard(target, max)` | แต้มการกันสูงสุด (โหมด `'points'`) |
| `SetInputMode(target, 'combo' \| 'keys')` | โหมดการกดของนักสู้ |
| `SetStyle(target, สไตล์, อาวุธ)` · `SetHeld(target, slot, คีย์)` | สไตล์และอาวุธของผู้เล่นและ NPC (เช่น จากคลาสหรือกระเป๋า) |

ทุก `Set...` มี `Get...` คู่กันสำหรับอ่านค่าปัจจุบัน

!!! warning "ไม่มีการบันทึกข้อมูล"
    ความก้าวหน้าเก็บอยู่ในหน่วยความจำเท่านั้น: เมื่อผู้เล่นออก (หรือ NPC หยุด) จะกลับเป็นค่าใน config การบันทึกเป็นหน้าที่ของระบบคุณ ซึ่งต้องส่งค่ากลับมาใหม่ทุกครั้งที่เข้าเกม เช่นเมื่อโหลดตัวละครเสร็จ

```lua
-- server: ระบบเลเวลของคุณ
local Unlocks = { -- เลเวลขั้นต่ำต่อการโจมตี แยกตามสไตล์
    katana = { ['combo@a03@rm'] = 5, ['combo@a04@rm'] = 10, ['combo@d03@rm'] = 15 },
}

local function applyProgress(src, level)
    for style, attacks in pairs(Unlocks) do
        for attackId, minLevel in pairs(attacks) do
            exports.avalon_combat:SetAttackEnabled(src, style, attackId, level >= minLevel)
        end
    end
    exports.avalon_combat:SetOutgoingMultiplier(src, 1.0 + level * 0.02) -- +2% ความเสียหายต่อเลเวล
end

AddEventHandler('mylevels:loaded', applyProgress)  -- โหลดตัวละครแล้ว
AddEventHandler('mylevels:levelUp', applyProgress) -- เลเวลใหม่
```

---

## Export ที่ใช้ได้ {#exports}

`target` คือ ID เซิร์ฟเวอร์ของผู้เล่น หรือ `{ net = netId }` สำหรับ NPC นักสู้

### Export ฝั่งเซิร์ฟเวอร์

| Export | คำอธิบาย |
|--------|-------------|
| `SetStyle(target, สไตล์ \| nil, อาวุธ)` | ให้ผู้ต่อสู้เข้าสู่การต่อสู้ด้วยสไตล์นั้น `อาวุธ`: `{ weapon, offhand, shield }` แต่ละอันเป็นคีย์ใน config, `false` คือมือเปล่า, ไม่ระบุคือใช้ของสไตล์ `nil` ให้ผู้เล่นออกจากการต่อสู้ (NPC ลบด้วย `StopFighter`) |
| `GetStyle(target)` | สไตล์ปัจจุบันของผู้ต่อสู้ |
| `SetHeld(target, slot, คีย์ \| nil)` | อาวุธ (`'weapon'`, `'offhand'`) หรือโล่ (`'shield'`) ในมือ โดยคงสไตล์ไว้ |
| `GetHeld(target)` | `{ slot = คีย์ }` |
| `SpawnFighter(สไตล์, coords, heading, opts)` | สร้าง NPC นักสู้ คืนค่า net id |
| `StopFighter(net)` | ลบ NPC นักสู้ |
| `AddHitHandler(fn)` / `RemoveHitHandler(id)` | ฟังก์ชันที่ถูกเรียกทุกการโจมตี (ดูด้านล่าง) |
| `GetDamageMode()` | `'native'` หรือ `'external'` |
| `React(target, อะไร)` | ให้เป้าหมายเล่น `'stagger'`, `'knockdown'` หรือ `'death'` |
| `SetAttackEnabled(target, สไตล์, attackId, on)` / `IsAttackEnabled(...)` | ปลดล็อกหรือล็อกท่าโจมตีของนักสู้คนนั้น |
| `SetAttackMultiplier(target, สไตล์, attackId, m)` / `GetAttackMultiplier(...)` | ตัวคูณของท่าโจมตี |
| `SetOutgoingMultiplier(target, m)` / `GetOutgoingMultiplier(target)` | ตัวคูณของความเสียหายทั้งหมดที่เขาสร้าง |
| `SetDefence(target, { percent, flat })` / `GetDefence(target)` | ค่าป้องกันของตัวเอง บวกเพิ่มจากของการป้องกัน |
| `GetGuard` / `SetGuard` / `GetMaxGuard` / `SetMaxGuard` | แต้มป้องกัน (โหมด `'points'`) |
| `SetInputMode(target, 'combo' \| 'keys' \| nil)` / `GetInputMode(target)` | โหมดอินพุตของนักสู้ |
| `GetStyles()` | สไตล์ทั้งหมดที่โหลดแล้ว |
| `GetAttacks(สไตล์)` | `{ { id, chain, clip, mult, locked, input } }` |
| `GetWeapon(คีย์)` / `GetShield(คีย์)` | รายการใน config |

#### การโจมตี (`hit`)

hit handler และ event `avalon_combat:hit` ได้รับ table นี้:

| ฟิลด์ | คำอธิบาย |
|-------|-------------|
| `mode` | `'native'` หรือ `'external'` |
| `attacker` | `{ kind = 'player' \| 'fighter', src, net, style, team }` |
| `victim` | `{ kind = 'player' \| 'fighter' \| 'ped', src, net, style }` |
| `attack` | `{ id, chain, clip, mult, knockdown, react }` (`chain`: `light`, `heavy`, `special`, `moves`, `pair`) |
| `weapon` | `{ slot, key, data }` หากตีโดนด้วยใบมีด มิฉะนั้นเป็น `nil` |
| `base` | ความเสียหายของอาวุธหรือของสไตล์ |
| `attackMult`, `progressMult`, `outgoingMult` | ตัวคูณทั้งสามตัว |
| `damage` | ความเสียหายก่อนหักการป้องกัน |
| `guard` | `{ blocked, parried, broken, percent, flat, cost }` |
| `final` | ความเสียหายหลังหักการป้องกัน (ค่าที่ใช้จริงในโหมด `'native'`) |
| `direction` | `{ x, y }` จากผู้โจมตีไปยังเหยื่อ |

handler จะทำงานหลังคำนวณการป้องกัน handler คืนค่าการโจมตีที่แก้ไขแล้ว, `nil` เพื่อปล่อยไว้ตามเดิม หรือ `false` เพื่อยกเลิก handler ของ resource ที่หยุดทำงานจะถูกลบออกเอง

```lua
-- server: ค่าต้านทานและเลเวล
exports.avalon_combat:AddHitHandler(function(hit)
    if hit.victim.kind == 'player' and HasSilverArmor(hit.victim.src) and hit.attacker.style == 'werewolf' then
        hit.final = hit.final * 0.5 -- เซิร์ฟเวอร์ปัดเศษหลังจาก handler ทำงานเสร็จ
        return hit
    end
end)
```

```lua
-- server: DamageMode = 'external', พลังชีวิตจัดการโดยระบบของคุณ
AddEventHandler('avalon_combat:hit', function(hit)
    if hit.victim.kind ~= 'player' then return end
    local hp = MyHealth.Remove(hit.victim.src, hit.final)
    if hp <= 0 then exports.avalon_combat:React(hit.victim.src, 'death') end
end)
```

**Event ฝั่งเซิร์ฟเวอร์** (`AddEventHandler`, รับข้อมูลการโจมตี): `avalon_combat:hit` (โหมด `'external'`), `avalon_combat:blocked`, `avalon_combat:parried`, `avalon_combat:guardBroken`

---

### Export ฝั่งไคลเอนต์

| Export | คืนค่า |
|--------|---------|
| `GetStyle()` | สไตล์ปัจจุบัน หรือ `nil` |
| `IsBlocking()` | `boolean` |
| `IsAttacking()` | `boolean` |
| `GetGuard()` | `{ v, max }` แต้มป้องกัน (โหมด `'points'`) |
| `GetStamina()` | สตามินาของเกม ตามที่โหมด `'stamina'` อ่าน |
| `SetCombatEnabled(on)` | `false` คืนการควบคุมให้เกม (เมนู คัตซีน) จนกว่าจะเป็น `true` อีกครั้ง |
| `GetCastPoint()` | `vector3` ที่เวทมนตร์ของคทาพุ่งออก หรือ `nil` |
| `GetInputMode()` | `'combo'` หรือ `'keys'` |
| `MakeFighter(ped, สไตล์, opts)` / `StopFighter(ped)` | เปลี่ยน ped ให้เป็นนักสู้ หรือหยุดมัน |

**Event ภายในเครื่อง** `avalon_combat:onHurt` `(ped, info, attacker)`: ทุกครั้งที่ ped ที่ไคลเอนต์นี้ดูแลโดนตี ใช้สำหรับ HUD

```lua
-- client: แถบพลังชีวิตและตัวเลขความเสียหาย
AddEventHandler('avalon_combat:onHurt', function(ped, info, attacker)
    if ped ~= PlayerPedId() then return end
    if info.parried then
        ShowText('ปัดป้อง!')
    elseif info.blocked then
        ShowText(('-%d (ป้องกัน)'):format(info.damage))
    else
        ShowText(('-%d'):format(info.damage))
    end
end)

-- client: ไม่ต่อสู้ขณะเมนูเปิดอยู่
exports.avalon_combat:SetCombatEnabled(false)
```
