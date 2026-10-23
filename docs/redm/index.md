# Panoramica Script RedM

Gli script RedM di AvalonStudios sono **standalone**: non dipendono da un framework e si collegano agli altri sistemi del server tramite export ed eventi.

---

## Lista Script

| Script | Versione | Export Client | Export Server | Database |
|--------|----------|:---:|:---:|:---:|
| [avalon_combat](combat.md) | beta | ✅ 10 | ✅ 28 | ❌ |

---

## Dipendenze per Script

```
avalon_combat  → OneSync, pacchetti animazione avalon_anims_*
```

!!! info "Pacchetti animazione"
    Gli stili di combattimento arrivano dai pacchetti animazione Avalon (`avalon_anims_*`): ogni pacchetto porta i propri stili e `avalon_combat` li carica da solo. Vedi [Stili](combat.md#stili).
