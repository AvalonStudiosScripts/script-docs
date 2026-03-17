# AvalonStudios Script Docs

Benvenuto nella documentazione ufficiale degli script FiveM di **AvalonStudios**.

Qui trovi la reference completa per tutti gli script: configurazione, export disponibili ed esempi di utilizzo.

---

## Script Disponibili

| Script | Descrizione | Export |
|--------|-------------|:------:|
| [avalon_coltivation](scripts/coltivation.md) | Sistema di coltivazione piante | ✅ |
| [avalon_dustman](scripts/dustman.md) | Lavoro netturbino (singolo/gruppo) | ❌ |
| [avalon_electrician](scripts/electrician.md) | Lavoro elettricista con minigiochi | ❌ |
| [avalon_lumberjack](scripts/lumberjack.md) | Lavoro taglialegna con minigioco | ❌ |
| [avalon_mining](scripts/mining.md) | Lavoro minatore con minigioco | ❌ |
| [avalon_robbery](scripts/robbery.md) | Sistema rapina negozi | ❌ |
| [avalon_trash](scripts/trash.md) | Ricerca oggetti nei bidoni | ❌ |

---

## Dipendenze Comuni

Tutti gli script richiedono le seguenti risorse attive sul server:

- **[ox_lib](https://github.com/overextended/ox_lib)** — Libreria base
- **[ox_target](https://github.com/overextended/ox_target)** — Sistema interazioni
- **[ox_inventory](https://github.com/overextended/ox_inventory)** — Gestione inventario
- **[es_extended](https://github.com/esx-framework/esx_core)** — Framework ESX

!!! tip "Suggerimento"
    Assicurati che tutte le dipendenze siano avviate **prima** degli script Avalon nell'`server.cfg`.

---

## Come usare gli Export

Gli export permettono ad altri script di richiamare funzioni di uno script Avalon senza dipendenze dirette nel codice.

=== "Client"

    ```lua
    -- Sintassi base per chiamare un export client-side
    exports['nome-resource']:NomeFunzione(parametro1, parametro2)
    ```

=== "Server"

    ```lua
    -- Sintassi base per chiamare un export server-side
    exports['nome-resource']:NomeFunzione(parametro1, parametro2)
    ```

!!! warning "Importante"
    Gli export **client** si chiamano solo da script lato client.
    Gli export **server** si chiamano solo da script lato server.

---

*Documentazione realizzata da AvalonStudios — [GitHub](https://github.com/AvalonStudiosScripts)*
