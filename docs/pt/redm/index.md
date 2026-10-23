# Visão Geral dos Scripts RedM

Os scripts RedM da AvalonStudios são **standalone**: não dependem de uma framework e ligam-se aos outros sistemas do servidor através de exports e eventos.

---

## Lista de Scripts

| Script | Versão | Exports Client | Exports Server | Base de Dados |
|--------|--------|:---:|:---:|:---:|
| [avalon_combat](combat.md) | beta | ✅ 10 | ✅ 28 | ❌ |

---

## Dependências por Script

```
avalon_combat  → OneSync, pacotes de animação avalon_anims_*
```

!!! info "Pacotes de animação"
    Os estilos de combate vêm dos pacotes de animação Avalon (`avalon_anims_*`): cada pacote traz os seus próprios estilos e o `avalon_combat` carrega-os sozinho. Vê [Estilos](combat.md#estilos).
