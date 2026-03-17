# AvalonStudios Script Docs

Bienvenido a la documentación oficial de los scripts FiveM de **AvalonStudios**.

Aquí encontrarás la referencia completa de todos los scripts: configuración, exports disponibles y ejemplos de uso.

---

## Scripts Disponibles

| Script | Descripción | Exports |
|--------|-------------|:-------:|
| [avalon_coltivation](scripts/coltivation.md) | Sistema de cultivo de plantas | ✅ |
| [avalon_dustman](scripts/dustman.md) | Trabajo de basurero (solo/grupo) | ❌ |
| [avalon_electrician](scripts/electrician.md) | Trabajo de electricista con minijuegos | ❌ |
| [avalon_lumberjack](scripts/lumberjack.md) | Trabajo de leñador con minijuego | ❌ |
| [avalon_mining](scripts/mining.md) | Trabajo de minero con minijuego | ❌ |
| [avalon_robbery](scripts/robbery.md) | Sistema de robo a tiendas | ❌ |
| [avalon_trash](scripts/trash.md) | Sistema de búsqueda en basura | ❌ |

---

## Dependencias Comunes

Todos los scripts requieren los siguientes recursos activos:

- **[ox_lib](https://github.com/overextended/ox_lib)** — Librería base
- **[ox_target](https://github.com/overextended/ox_target)** — Sistema de interacciones
- **[ox_inventory](https://github.com/overextended/ox_inventory)** — Gestión de inventario
- **[es_extended](https://github.com/esx-framework/esx_core)** — Framework ESX

!!! tip "Consejo"
    Asegúrate de que todas las dependencias se inicien **antes** que los scripts Avalon en tu `server.cfg`.

---

## Cómo Usar los Exports

Los exports permiten que otros scripts llamen funciones de un script Avalon sin dependencias directas en el código.

=== "Client"

    ```lua
    -- Llamar un export desde el lado cliente
    exports['nombre-resource']:NombreFuncion(param1, param2)
    ```

=== "Server"

    ```lua
    -- Llamar un export desde el lado servidor
    exports['nombre-resource']:NombreFuncion(param1, param2)
    ```

!!! warning "Importante"
    Los exports de **cliente** solo pueden llamarse desde scripts del lado cliente.
    Los exports de **servidor** solo pueden llamarse desde scripts del lado servidor.

---

*Documentación por AvalonStudios — [GitHub](https://github.com/AvalonStudiosScripts)*
