# avalon_party

ระบบ**ปาร์ตี้แบบ MMORPG**ใน FiveM สร้างกลุ่มพร้อม HUD แบบเรียลไทม์ แถบพลังชีวิตที่ปรับแต่งได้ ระบบ ready check, waypoint ร่วม, โหมดสเปคเตเตอร์อัตโนมัติ, blip บนแผนที่และมาร์กเหนือหัวเพื่อนร่วมทีม, ระบบมาร์กเป้าหมาย และอินสแตนซ์ส่วนตัว (routing bucket)

**เวอร์ชัน:** 1.0.0 | **ผู้พัฒนา:** Avalon | **Framework:** ESX / QBCore / QBox / Standalone

---

## ความต้องการ

| Resource | จำเป็น |
|----------|:------:|
| ox_lib | ✅ |
| ox_target | ✅ |

---

## การติดตั้ง

```
ensure ox_lib
ensure ox_target
ensure avalon_party
```

ตรวจจับ framework อัตโนมัติ: `es_extended` → ESX, `qbx_core` → QBox, `qb-core` → QBCore ถ้าไม่พบจะทำงานแบบ standalone

---

## ภาษา

ข้อความทั้งหมดอยู่ใน `locales/<รหัส>.lua` มีให้: **อังกฤษ, อิตาลี, โปรตุเกส, สเปน, รัสเซีย, จีนตัวย่อ, ไทย**

```lua
-- config.lua (ให้บรรทัดนี้อยู่บนสุดของไฟล์)
Config.locale = 'th'   -- en | it | pt | es | ru | zh | th
```

คีย์ที่ขาดในไฟล์แปลจะย้อนกลับไปใช้ภาษาอังกฤษอัตโนมัติ วิธีเพิ่มภาษา: คัดลอก `locales/en.lua` เปลี่ยนชื่อ (เช่น `de.lua`) แปลค่าต่าง ๆ เปลี่ยน `Locales['en']` เป็น `Locales['de']` แล้วตั้ง `Config.locale = 'de'`

---

## คำสั่ง

| คำสั่ง | พารามิเตอร์ | คำอธิบาย |
|--------|-----------|----------|
| `/party` | — | เปิดเมนูจัดการปาร์ตี้ |
| `/partyinvite` | `[id ผู้เล่น]` | เชิญผู้เล่นเข้าปาร์ตี้ |
| `/partyaccept` | — | ยอมรับคำเชิญ |
| `/partydecline` | — | ปฏิเสธคำเชิญ |
| `/partyleave` | — | ออกจากปาร์ตี้ |
| `/partydisband` | — | ยุบปาร์ตี้ (หัวหน้า) |
| `/partykick` | `[id ผู้เล่น]` | ไล่สมาชิกออก (หัวหน้า) |
| `/partytransfer` | `[id ผู้เล่น]` | โอนตำแหน่งหัวหน้า (หัวหน้า) |
| `/partyready` | — | เริ่ม ready check (หัวหน้า) |
| `/partywp` | — | แชร์ waypoint (หัวหน้า) |

ไม่จำเป็นต้องมีปาร์ตี้อยู่ก่อน: คำเชิญแรกจะสร้างกลุ่มให้เอง เล็งไปที่ผู้เล่นคนอื่นด้วย **ox_target** จะเห็นตัวเลือก **"เชิญเข้าปาร์ตี้"** (ระยะ 3 เมตร)

---

## ปุ่มลัด

| ปุ่ม | การทำงาน |
|------|----------|
| **F5** | เปิดเมนูจัดการปาร์ตี้ |
| **F1** / **F2** | ยอมรับ / ปฏิเสธคำเชิญ |
| **F3** / **F4** | พร้อม / ไม่พร้อม (ready check) |
| **TAB** | สลับเป้าหมายที่อยู่ใกล้ (ต้องเปิดจากเมนูก่อน) |
| **CTRL** (กดค้าง) | เป้าเล็งสำหรับมาร์ก เฉพาะหัวหน้า — คลิกขวาที่ ped เพื่อเลือกสัญลักษณ์ |
| **← →** | เปลี่ยนผู้เล่นที่กำลังดูในโหมดสเปคเตเตอร์ |

ค่าเริ่มต้นอยู่ใน `Config.Keys` และผู้เล่นแต่ละคนเปลี่ยนปุ่มได้จากการตั้งค่าปุ่มของ FiveM

---

## เมนูปาร์ตี้ (F5)

หน้าจอ NUI ประกอบด้วย: รายชื่อสมาชิก (หัวหน้าไล่ออกและโอนตำแหน่งได้), ออก/ยุบปาร์ตี้, เริ่ม ready check, แชร์ waypoint, สวิตช์เป้าหมาย TAB และการมาร์กสัญลักษณ์, การเปิดปิดแต่ละแถบและไอคอนสถานะ, การเปิดปิด blip และมาร์กเหนือหัว รวมถึงสเกล HUD ตั้งแต่ `0.5` ถึง `3.0`

!!! tip "การตั้งค่าถูกบันทึกไว้"
    ตัวเลือกในเมนูเป็นแบบ**รายผู้เล่น** และเก็บใน `localStorage` ของ NUI: ไม่มีทราฟฟิกไปเซิร์ฟเวอร์และไม่ต้องใช้ฐานข้อมูล

---

## การตั้งค่า

| ตัวเลือก | ค่าเริ่มต้น | คำอธิบาย |
|----------|------------|----------|
| `Config.locale` | `'en'` | ภาษาของข้อความ |
| `Config.maxPartySize` | `6` | จำนวนสมาชิกสูงสุด |
| `Config.inviteExpiry` | `30000` | เวลาก่อนคำเชิญหมดอายุ (ms) |
| `Config.offlineGracePeriod` | `120000` | เวลาที่สมาชิกหลุดยังรักษาที่ไว้ (ms) |
| `Config.readyCheckTimeout` | `15000` | ระยะเวลาของ ready check (ms) |

```lua
Config.Features = {
    distance   = true,  -- แสดงระยะห่างเป็นเมตรบนการ์ดสมาชิก
    readyCheck = true,  -- ระบบ ready check
    waypoint   = true,  -- waypoint ร่วม
    spectator  = true,  -- สเปคเตเตอร์อัตโนมัติเมื่อตาย
    targetMark = true,  -- มาร์กเป้าหมายด้วยสัญลักษณ์
}

Config.UI = {
    position       = 'top-left',  -- top-left | top-right | bottom-left | bottom-right
    pickClick      = 'right',     -- ปุ่มเมาส์สำหรับเลือกสมาชิก
    hudWidth       = 260,
    scale          = 1.0,         -- 1.0 = ปรับอัตโนมัติตามความละเอียด
    showStatuses   = true,
    updateInterval = 500,         -- ระยะเวลาอัปเดตค่าพลัง (ms)
    criticalPct    = 20,          -- ต่ำกว่านี้แถบจะกะพริบสีแดง
}
```

เมื่อปล่อย `scale = 1.0` HUD จะปรับขนาดเอง: `1.5` ตั้งแต่ 2560 px, `1.6` ตั้งแต่ 3440 px, `2.0` ตั้งแต่ 3840 px

### พลังชีวิตและสตามินา

ทั้งสองค่าไม่ผ่านเซิร์ฟเวอร์: ทุกไคลเอนต์อ่านจาก native ของ GTA สำหรับสมาชิกที่อยู่ในระยะสตรีม เมื่อออกนอกระยะ (~200 ม.) จะคงค่าล่าสุดไว้และทำเครื่องหมายว่าไม่อัปเดต

```lua
Config.Health = {
    displayMode     = 'usable',  -- 'usable' | 'raw' | 'percent'
    syncMaxHealth   = true,      -- แชร์ค่า HP สูงสุดระหว่างไคลเอนต์ (state bag)
    applyMaxToClone = true,      -- ยกเพดาน HP ของ ped ที่ถูกโคลน
    autoDeadStatus  = true,      -- เปิดสถานะ 'dead' อัตโนมัติเมื่อเลือดหมด
    deadStatusId    = 'dead',
}
```

| `displayMode` | สิ่งที่แสดง |
|---------------|-------------|
| `usable` | ชาย `87/100`, หญิง `62/75` — พลังชีวิตหักเกณฑ์การตาย |
| `raw` | `187/200` และ `162/175` — ค่าดิบจาก native |
| `percent` | `87/100` เท่ากันทุกคน |

### องค์ประกอบในโลกและการมาร์ก

`Config.World` ควบคุม blip บนแผนที่ (`showBlips`, `blipSprite`, `blipColorMember`, `blipColorLeader`, `blipScale`) และมาร์กเหนือหัว (`showHeadMarker`, `headMarkerDist`, `headMarkerOffsetZ`, `headMarker`) ค่าเหล่านี้เป็นค่าเริ่มต้น ผู้เล่นแต่ละคนปิดได้จากเมนู

`Config.TargetMark` ปรับระยะตรวจจับของ TAB, สีและขนาดสัญลักษณ์ และไอคอนเป้าเล็ง (`ctrlIcon` ใช้คลาส Font Awesome ใดก็ได้) สัญลักษณ์ที่ใช้ได้กำหนดใน `Config.TargetSymbols`: `warning` ⚠️, `target` 🎯, `attack` ❌, `defend` 🛡️, `heal` 💊, `focus` ⭐

### แถบสถานะ

แถบที่ตั้ง `builtin = true` อัปเดตเองจาก native ของ GTA ส่วนแถบที่ `builtin = false` จะ**ซ่อนไว้จนกว่าสคริปต์ของคุณจะส่งค่า**ผ่าน `SetMemberBar`

| ID | ป้ายชื่อ | Builtin | สี |
|----|----------|:-------:|-----|
| `health` | HP | ✅ | แดง |
| `stamina` | สตามินา | ✅ | เขียว |
| `mana` | มานา | ❌ | ฟ้า |
| `shield` | โล่ | ❌ | เทา |
| `rage` | เกรี้ยวกราด | ❌ | ส้ม |

```lua
-- config.lua ภายใน Config.Bars
{
    id = 'energy', label = 'พลังงาน', color = '#f39c12', bgColor = '#3d2200',
    icon = '⚡', defaultMax = 100, builtin = false, visible = true,
},
```

!!! tip "ใช้ระบบของคุณคุมพลังชีวิต"
    ตั้ง `builtin = false` ที่แถบ `health` (หรือ `stamina`) รีซอร์สจะหยุดเขียนค่าลงแถบนั้น และค่าจะมาจาก `SetMemberBar` ของคุณแทน

### สถานะ

`combat` ⚔️ · `poisoned` ☠️ · `buffed` ✨ · `dead` 💀 · `afk` 💤 — เปิดปิดด้วย `SetMemberStatus` ยกเว้น `dead` ที่เป็นอัตโนมัติตราบใดที่ `Config.Health.autoDeadStatus = true`

```lua
-- config.lua ภายใน Config.Statuses
{ id = 'stunned', label = 'มึนงง', icon = '💫', color = '#9b59b6', duration = 0 },
```

---

## Export ที่ใช้งานได้

### ฝั่งเซิร์ฟเวอร์

#### `IsInParty(source)` → `boolean`
```lua
if exports['avalon_party']:IsInParty(source) then
    -- ผู้เล่นอยู่ในปาร์ตี้
end
```

#### `GetPartyMembers(source)` → `table | nil`
คืนรายการ server ID ของสมาชิกทุกคน (รวมหัวหน้า) หรือ `nil` ถ้าไม่ได้อยู่ในปาร์ตี้
```lua
local members = exports['avalon_party']:GetPartyMembers(source)
if members then
    for _, id in ipairs(members) do
        exports.ox_inventory:AddItem(id, 'health_potion', 1)
    end
end
```

#### `GetPartyLeader(source)` → `number | nil`
```lua
local leader = exports['avalon_party']:GetPartyLeader(source)
```

#### `SetMemberBar(source, barId, value, maxValue)` → `boolean`
อัปเดตแถบใน HUD ที่สมาชิกทุกคนเห็นแบบเรียลไทม์ ถูกปฏิเสธหากแถบนั้นตั้ง `builtin = true`

```lua
RegisterNetEvent('magic:cast')
AddEventHandler('magic:cast', function(cost)
    local src = source
    local newMana = math.max(0, GetPlayerMana(src) - cost)
    exports['avalon_party']:SetMemberBar(src, 'mana', newMana, 100)
end)
```

#### `SetMemberStatus(source, statusId, active)` → `boolean`
แสดงหรือซ่อนไอคอนสถานะบน HUD

```lua
exports['avalon_party']:SetMemberStatus(source, 'poisoned', true)
exports['avalon_party']:SetMemberStatus(source, 'poisoned', false)
```

#### `SetPartyBucket(source, bucket)` → `boolean`
ย้ายทั้งปาร์ตี้เข้า routing bucket (อินสแตนซ์ส่วนตัว) bucket จะผูกกับปาร์ตี้: คนที่เข้าทีหลังถูกย้ายตามอัตโนมัติ ส่วนคนที่ออกจะกลับไป bucket 0 ไม่จำเป็นต้องเป็นหัวหน้า

```lua
RegisterNetEvent('dungeon:enter')
AddEventHandler('dungeon:enter', function(id)
    local src = source
    local ok = exports['avalon_party']:SetPartyBucket(src, 1000 + id)
    if not ok then
        TriggerClientEvent('ox_lib:notify', src, { type='error', description='ต้องอยู่ในปาร์ตี้ก่อน!' })
    end
end)
```

#### `GetPartyBucket(source)` → `number`
```lua
local bucket = exports['avalon_party']:GetPartyBucket(source)  -- 0 = โลกหลัก
```

---

### ฝั่งไคลเอนต์

#### `RequestMemberPick(mode, callbackEvent)`
เปิดโหมดเลือกสมาชิก: เคอร์เซอร์จะปรากฏบนการ์ดปาร์ตี้ แล้วผู้เล่นคลิกเลือกสมาชิกที่ต้องการ

- `mode` `"self"` → อีเวนต์ไปที่ผู้เลือก · `"target"` → อีเวนต์ไปที่สมาชิกที่ถูกเลือก
- callback ได้รับ `{ id, name, bars, statuses, isLeader, offline, picker }`

!!! warning "ยกเลิกแล้วไม่มี callback"
    หากกด **ESC** หรือเรียก `CancelMemberPick()` อีเวนต์จะ**ไม่**ถูกส่ง ปุ่มเมาส์ที่ใช้คลิกกำหนดด้วย `Config.UI.pickClick`

```lua
RegisterCommand('heal_ally', function()
    exports['avalon_party']:RequestMemberPick('self', 'healing:targetPicked')

    SetTimeout(10000, function()
        exports['avalon_party']:CancelMemberPick()
    end)
end)

RegisterNetEvent('healing:targetPicked')
AddEventHandler('healing:targetPicked', function(member)
    if member.offline then return end
    TriggerServerEvent('healing:apply', member.id, 50)
end)
```

#### `CancelMemberPick()`
ยกเลิกการเลือกสมาชิกที่กำลังทำงานอยู่ โดยไม่ส่งอีเวนต์ใด ๆ

```lua
exports['avalon_party']:CancelMemberPick()
```

#### `GetPartyTarget()` → `{ ped, netId, source, isPlayer } | nil`
คืนข้อมูล ped ที่เลือกอยู่จากการวน **TAB** หากเป้าหมายเป็น NPC ค่า `source` จะเป็น `nil`

```lua
local target = exports['avalon_party']:GetPartyTarget()
if target and target.isPlayer then
    TriggerServerEvent('combat:attack', target.source)
end
```

!!! tip "ไฟล์ตัวอย่าง"
    รีซอร์สมีไฟล์ `example_integration.lua` ซึ่งไม่ถูกโหลดเข้าเกม เป็นเพียงชุดตัวอย่างโค้ดให้คัดลอกไปใช้
