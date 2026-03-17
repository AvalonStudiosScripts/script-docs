# AvalonStudios Script Docs

Welcome to the official FiveM script documentation for **AvalonStudios**.

Here you'll find the complete reference for all scripts: configuration, available exports, and usage examples.

---

## Available Scripts

| Script | Description | Exports |
|--------|-------------|:-------:|
| [avalon_coltivation](scripts/coltivation.md) | Plant cultivation system | ✅ |
| [avalon_dustman](scripts/dustman.md) | Garbage collector job (solo/group) | ❌ |
| [avalon_electrician](scripts/electrician.md) | Electrician job with minigames | ❌ |
| [avalon_lumberjack](scripts/lumberjack.md) | Lumberjack job with minigame | ❌ |
| [avalon_mining](scripts/mining.md) | Mining job with minigame | ❌ |
| [avalon_robbery](scripts/robbery.md) | Shop robbery system | ❌ |
| [avalon_trash](scripts/trash.md) | Trash searching system | ❌ |

---

## Common Dependencies

All scripts require the following resources to be running:

- **[ox_lib](https://github.com/overextended/ox_lib)** — Base library
- **[ox_target](https://github.com/overextended/ox_target)** — Interaction system
- **[ox_inventory](https://github.com/overextended/ox_inventory)** — Inventory management
- **[es_extended](https://github.com/esx-framework/esx_core)** — ESX Framework

!!! tip "Tip"
    Make sure all dependencies are started **before** Avalon scripts in your `server.cfg`.

---

## How to Use Exports

Exports allow other scripts to call functions from an Avalon script without direct code dependencies.

=== "Client"

    ```lua
    -- Basic syntax to call a client-side export
    exports['resource-name']:FunctionName(param1, param2)
    ```

=== "Server"

    ```lua
    -- Basic syntax to call a server-side export
    exports['resource-name']:FunctionName(param1, param2)
    ```

!!! warning "Important"
    **Client** exports can only be called from client-side scripts.
    **Server** exports can only be called from server-side scripts.

---

*Documentation by AvalonStudios — [GitHub](https://github.com/AvalonStudiosScripts)*
