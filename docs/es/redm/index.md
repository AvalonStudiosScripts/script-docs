# Resumen de Scripts RedM

Los scripts RedM de AvalonStudios son **standalone**: no dependen de un framework y se conectan con los demás sistemas del servidor mediante exports y eventos.

---

## Lista de Scripts

| Script | Versión | Exports Client | Exports Server | Base de datos |
|--------|----------|:---:|:---:|:---:|
| [avalon_combat](combat.md) | beta | ✅ 10 | ✅ 28 | ❌ |

---

## Dependencias por Script

```
avalon_combat  → OneSync, avalon_weapons_rdr2, avalon_shields_tint, paquetes de animación avalon_anims_*
```

!!! info "Paquetes de animación"
    Los estilos de combate provienen de los paquetes de animación Avalon (`avalon_anims_*`): cada paquete aporta sus propios estilos y `avalon_combat` los carga por sí solo. Ver [Estilos](combat.md#estilos).
