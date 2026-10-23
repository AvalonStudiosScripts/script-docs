# RedM Scripts Overview

AvalonStudios RedM scripts are **standalone**: they do not depend on a framework and connect to the other systems on the server through exports and events.

---

## Script List

| Script | Version | Client Exports | Server Exports | Database |
|--------|---------|:---:|:---:|:---:|
| [avalon_combat](combat.md) | beta | ✅ 10 | ✅ 28 | ❌ |

---

## Dependencies per Script

```
avalon_combat  → OneSync, avalon_weapons_rdr2, avalon_shields_tint, avalon_anims_* animation packs
```

!!! info "Animation packs"
    Combat styles come from the Avalon animation packs (`avalon_anims_*`): each pack ships its own styles and `avalon_combat` loads them on its own. See [Styles](combat.md#styles).
