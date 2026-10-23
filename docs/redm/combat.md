# avalon_combat

Sistema di **combattimento corpo a corpo** per RedM. Ogni stile (licantropo, vampiro, katana, spada e scudo, pugile, maghi...) usa le animazioni dei pacchetti Avalon. I colpi li calcola lo script: ogni frame segue mani, piedi e lame lungo l'animazione e colpisce i ped che attraversano. Il danno passa sempre dal server.

Comprende: combo leggere e pesanti, attacchi accoppiati (due ped animati insieme), parata e contrattacco, armi e scudi impugnati, scatto, schivata, balzo e volo per le creature, NPC che combattono da soli ed export per collegare livelli, abilità, barre vita e magie.

**Versione:** beta | **Autore:** Avalon | **Gioco:** RedM | **Framework:** standalone

---

## Dipendenze

| Resource | Obbligatoria | A cosa serve |
|----------|:---:|---|
| OneSync | ✅ | NPC combattenti e parata (state bag) |
| `avalon_weapons_rdr2` | ✅ | I modelli delle armi impugnate |
| `avalon_shields_tint` | ✅ | I modelli degli scudi |
| Pacchetti animazione `avalon_anims_*` | ✅ almeno uno | Le animazioni e gli stili (vedi [Stili](#stili)) |

Nessun framework richiesto: lo script lavora sui ped e sui giocatori. Il collegamento con inventari, livelli o barre vita si fa con gli [export](#export-disponibili).

---

## Installazione

1. Copia la cartella `avalon_combat/` in `resources/`
2. Avvia le armi, gli scudi e i pacchetti animazione, poi la risorsa:

```
ensure avalon_weapons_rdr2
ensure avalon_shields_tint
ensure avalon_anims_werewolf
ensure avalon_anims_vampire
ensure avalon_anims_ka_02
# ... gli altri pacchetti che usi
ensure avalon_combat
```

!!! info "Ordine dei pacchetti"
    Un pacchetto avviato dopo `avalon_combat` (o riavviato) viene letto lo stesso: i suoi stili si aggiungono appena parte.

---

## Stili

Gli stili arrivano dai **pacchetti animazione**. Ogni pacchetto porta un file `avalon_combat.lua` con i propri stili e lo dichiara nel suo `fxmanifest.lua`. `avalon_combat` li raccoglie da solo tramite l'export `CombatStyles` del pacchetto.

| Stile | Pacchetto | Arma | Guardia | Extra |
|-------|-----------|------|---------|-------|
| `werewolf` | `avalon_anims_werewolf` | artigli | del gioco (mani nude) | scatto, schivata, balzo, attacchi accoppiati |
| `vampire` | `avalon_anims_vampire` | artigli | propria | volo, boost, attacchi accoppiati |
| `succube` | `avalon_anims_succube` | artigli | del gioco (mani nude) | volo, boost, attacchi accoppiati |
| `katana` `katana1` `katana3` `katana4` | `avalon_anims_ka_02` `_01` `_03` `_04` | `xur_katana` | del gioco (lama) | |
| `sword` `sword2` `sword3` | `avalon_anims_sw_01` `_02` `_03` | `xur_machete2` | del gioco (lama) | |
| `sword_shield` `sword_shield2` `sword_shield3` | `avalon_anims_sns_01` `_02` `_03` | `swn2craft` + scudo | propria / propria / del gioco | |
| `twinsword` | `avalon_anims_twinsword01` | `xur_katana2` in entrambe le mani | del gioco (lama) | |
| `hammer2h` | `avalon_anims_hammer2h` | `hammermoore` | del gioco (lama) | |
| `orientalmonk` | `avalon_anims_orientalmonk` | `giga` | del gioco (lama) | |
| `pugilist` | `avalon_anims_p_01` | mani nude | del gioco (mani nude) | |
| `firemage` `mage` `witch` `wizard` | `avalon_anims_firemage_01` `_mage_01` `_witch_01` `_wizard` | mani / bastone | del gioco (mani nude) | |

!!! warning "Magie"
    Gli stili da mago per ora colpiscono con le mani, con una portata più lunga (come farebbe l'incantesimo). I bastoni sono solo visivi: il punto da cui partiranno le magie si legge con l'export client `GetCastPoint()`.

Per scrivere uno stile tuo vedi [Creare uno stile](#creare-uno-stile).

---

## Comandi e tasti

### Modalità combattimento

| Comando | Descrizione |
|---------|-------------|
| `/avalon_combat <stile>` | Entra in combattimento con quello stile (senza argomento elenca gli stili) |
| `/avalon_combat off` | Esce dal combattimento |

| Tasto | Azione |
|-------|--------|
| **Click sinistro** | Combo leggera (premi di nuovo durante l'attacco per il colpo successivo) |
| **Click destro** | Combo pesante |
| **R** | Attacco accoppiato sul ped davanti (gli stili che ne hanno) oppure combo speciale |
| **CTRL** (tenuto) | Guardia |
| **E** | Aggancia il ped puntato con la camera (rombo rosso sopra la testa): attacchi e attacchi accoppiati vanno su di lui. E di nuovo lo rilascia |
| **Shift** | Scatto (licantropo) · volo (stili volanti, in aria) |
| **Alt** | Schivata (licantropo) · boost (stili volanti) |
| **Shift + Spazio** | Balzo (licantropo) · decollo e atterraggio (stili volanti) |

!!! info "Chi reagisce ai colpi"
    I colpi leggeri e pesanti fanno solo danno. La combo speciale, gli attacchi accoppiati e gli attacchi con `react = true` fanno anche barcollare o cadere la vittima.

### Comandi di prova e regolazione

| Comando | Descrizione |
|---------|-------------|
| `/avalon_attack <id>` | Esegue un attacco dello stile attuale (id o nome della clip) |
| `/avalon_pair [nome]` | Esegue un attacco accoppiato sul ped davanti |
| `/avalon_pair_offset <nome> <x> <y> <yaw>` | Sposta la vittima di un attacco accoppiato (solo su questo client) |
| `/avalon_weapon <nome\|off>` | Cambia l'arma in mano destra |
| `/avalon_offhand <nome\|off>` | Cambia l'arma in mano sinistra |
| `/avalon_shield <nome\|off>` | Cambia lo scudo |
| `/avalon_weapon_offset <x y z rx ry rz> [osso]` | Regola la posizione dell'arma e stampa la riga da copiare nel config (anche `_offhand_offset`, `_shield_offset`) |
| `/avalon_sound <nome>` | Suona un suono del config |
| `/avalon_sound_game <set> <nome> [bank]` | Prova un suono qualsiasi del gioco |
| `/avalon_npc <stile> [team] [modello]` | Crea un NPC combattente 4 m davanti |
| `/avalon_npc_clear` | Cancella gli NPC creati da te |
| `/avalon_npc_info` | Stato degli NPC combattenti entro 30 m |
| `/avalon_dummy [modello]` · `/avalon_dummy_clear` | Manichini fermi da colpire |
| `/avalon_hitbox` | Mostra le hitbox: corpi (verde), arti (giallo), lame (azzurro), colpi (rosso) |
| `/avalon_combat_debug` | Disegna i movimenti delle mani e stampa ogni colpo |

---

## Come funziona il danno

1. Il client di chi attacca controlla ogni frame arti e lame lungo l'animazione. Un arto più lento di `MinSweepSpeed` non colpisce (caricamento e ritorno del colpo).
2. Quando un arto o una lama tocca la hitbox di un ped, il client lo comunica al server.
3. Il server verifica il colpo (distanza, danno massimo, attacco sbloccato), calcola danno e guardia ed esegue gli *hit handler* delle altre risorse.
4. A seconda di `AvalonCombat.DamageMode`:
    - `'native'` — il danno va sulla vita vera del ped
    - `'external'` — nessun danno: il server emette l'evento `avalon_combat:hit` per il tuo sistema (barre vita finte, resistenze)

    La vittima esegue comunque la reazione.

**Formula:** danno base (quello dell'arma se colpisce la lama, altrimenti `damage` dello stile) × `mult` dell'attacco × moltiplicatore di progressione × moltiplicatore in uscita, arrotondato. Poi si toglie la difesa della guardia.

---

## Configurazione

I file di configurazione stanno in `config/`:

| File | Contenuto |
|------|-----------|
| `config/config.lua` | Impostazioni generali, combo, NPC, suoni, danno, difesa, input |
| `config/guard.lua` | Le guardie del gioco usabili per nome (`'blade'`, `'unarmed'`) |
| `config/weapons.lua` | Armi della mano destra |
| `config/offhand.lua` | Armi della mano sinistra |
| `config/shields.lua` | Scudi (una voce per colore) |
| `config/styles/` | Stili locali opzionali e `example.lua` con tutti i campi spiegati |

### Impostazioni generali

| Opzione | Default | Descrizione |
|---------|---------|-------------|
| `TargetRange` | `4.0` | m: i ped più lontani non vengono controllati |
| `LockRange` / `LockCone` | `3.0` / `45` | Un attacco si gira verso il ped più vicino entro questi metri e gradi |
| `LockOnRange` | `12.0` | m: raggio dell'aggancio con E |
| `MinSweepSpeed` | `2.0` | m/s: un arto più lento non colpisce |
| `SkipStart` | `0.10` | Parte iniziale di ogni attacco ignorata |
| `Rehit` | `0.45` | s prima che lo stesso attacco possa ricolpire lo stesso bersaglio |
| `MaxHitDamage` | `200` | Il server rifiuta colpi sopra questo danno |
| `MaxHitDistance` | `6.0` | Il server rifiuta colpi su giocatori più lontani di così |
| `PairRange` | `3.5` | m: distanza massima di presa di un attacco accoppiato |
| `HitboxScale` / `HitboxPad` | `1.0` / `0.0` | Ingrandisce le hitbox delle lame (per colpire più facilmente) |
| `SoundVolume` | `0.35` | Volume dei file audio |
| `DamageMode` | `'native'` | `'native'` o `'external'` (vedi sopra) |

### Combo

```lua
AvalonCombat.ComboWindow = 0.45 -- da questa fase dell'attacco una pressione mette in coda il colpo successivo
AvalonCombat.ComboCancel = 1.0  -- fase in cui parte il colpo in coda (1.0 = a fine animazione)
AvalonCombat.ComboReset = 0.8   -- s: oltre questa pausa la combo riparte dal primo colpo
```

### Modalità di input

```lua
AvalonCombat.InputMode = 'combo' -- 'combo' | 'keys'
AvalonCombat.Keys = { SimulWindow = 0.08, SequenceWindow = 0.6, DirAbove = 0.5 }
```

- `'combo'`: ogni tasto esegue il colpo successivo della sua catena (`light`, `heavy`, `special`)
- `'keys'`: come in un picchiaduro, i tasti eseguono le `moves` dello stile. Si combinano tasti premuti insieme (`'light+heavy'`), una direzione (`'F+heavy'`: avanti + pesante) o sequenze (`{ 'light', 'light', 'heavy' }`)

Ogni giocatore può averne una diversa con l'export `SetInputMode`.

### Guardia e parata

```lua
AvalonCombat.Defence = {
    Mode = 'always',      -- 'always' | 'timed' | 'stamina' | 'points'
    ParryWindow = 0.25,   -- s: guardia alzata entro questo tempo dal colpo = parata
    StaminaCost = 15.0,   -- 'stamina': stamina tolta per colpo parato
    Points = 100, PointsRegen = 10, PointsCost = 20, -- 'points': punti guardia
    BreakTime = 1.5,      -- s senza guardia dopo che si rompe
    Block = { percent = 0.75, flat = 0 }, -- difesa base della guardia
}
```

| `Mode` | Quando la guardia ferma un colpo |
|--------|----------------------------------|
| `always` | Sempre, danno ridotto dalla difesa |
| `timed` | Solo entro `ParryWindow` dall'alzata, altrimenti il colpo entra pieno |
| `stamina` | Sempre, ogni colpo costa stamina; finita la stamina la guardia si rompe |
| `points` | Sempre, ogni colpo costa punti guardia (export `GetGuard` / `SetGuard`); a zero si rompe |

Una **parata** annulla il danno e fa barcollare chi attacca. **Difesa** = `danno × (1 - percent) - flat`, mai sotto zero. Uno scudo impugnato sostituisce la difesa della guardia con la propria; quella data con l'export `SetDefence` si somma.

### Suoni

Ogni suono ha un nome in `AvalonCombat.Sounds`. Stili, attacchi, armi e attacchi accoppiati usano quei nomi: un nome, una lista da cui sceglierne uno a caso, oppure `{ light = ..., heavy = ..., special = ... }` per catena.

```lua
AvalonCombat.Sounds = {
    punch = { file = melee('punch', 11) },                 -- file in sounds/ (.wav .ogg .mp3)
    my_roar = { file = 'roar.ogg', volume = 0.8, range = 40 },
    beast_roar = { set = 'LSHNT_sounds', name = 'ROAR' },  -- suono del gioco
}
```

I suoni corpo a corpo inclusi sono quelli del gioco, estratti in `sounds/melee_*.wav`.

!!! tip "Suono al momento giusto"
    `soundAt` fa partire il suono a una fase precisa dell'animazione: `0.35` = al 35% della clip. Vale per gli attacchi, per gli attacchi accoppiati e per i movimenti dello stile (`soundAt = { leap = 0.27, dodge = 0.15 }`). Senza `soundAt` il suono parte al primo movimento veloce (attacchi) o subito.

### Hitbox del corpo

`AvalonCombat.BodyHitbox` elenca le capsule su cui si colpisce ogni ped (`{ osso da, osso a, raggio }`): testa, torace, addome, braccia e gambe. Un animale senza quelle ossa diventa una sfera di 0.4 m. `/avalon_hitbox` le mostra in gioco.

---

## Armi e scudi

Ogni voce di `config/weapons.lua`, `config/offhand.lua` e `config/shields.lua` è un modello impugnato:

```lua
AvalonCombat.Weapons.xur_katana = {
    model = 'weapon_xur_katana',
    bone = 'SKEL_R_Hand',                         -- osso a cui è attaccato
    offset = vector3(0.075, 0.00, -0.03),         -- m
    rot = vector3(-90.0, 0.0, 0.0),               -- gradi
    damage = 25,                                  -- per colpo di lama, × mult dell'attacco
    sounds = { swing = 'blade_swipe', hit = { 'machete_slash', 'blade_flesh' } },
    blade = { from = vector3(0, 0, 0.12), to = vector3(0, 0, 1.05), radius = 0.08 }, -- hitbox della lama
}
```

| Campo | Descrizione |
|-------|-------------|
| `model` | Archetipo del modello |
| `tint` | Opzionale: indice del colore (gli scudi hanno una voce per colore) |
| `bone`, `offset`, `rot` | Dove sta in mano. Si regola in gioco con `/avalon_weapon_offset`, che stampa la riga da copiare |
| `damage` | Danno per colpo di lama |
| `sounds` | `swing` e `hit` della lama |
| `blade` | Capsula della lama nello spazio del modello: colpisce al posto della mano |
| `cast` | Solo bastoni: il punto da cui partiranno le magie |
| `defence` | Solo scudi: `{ percent, flat }` al posto di quella della guardia |

---

## Creare uno stile

Uno stile si può scrivere in due posti:

=== "Nel pacchetto animazione"

    Il modo usato dai pacchetti Avalon. Accanto al `fxmanifest.lua` del pacchetto metti `avalon_combat.lua`:

    ```lua
    local Styles = {}

    Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
        block = 'blade',   -- un pacchetto nomina la guardia del gioco come stringa
    }

    exports('CombatStyles', function() return Styles end)
    ```

    e nel `fxmanifest.lua` del pacchetto:

    ```lua
    avalon_combat_styles 'yes'
    shared_script 'avalon_combat.lua'
    ```

    Una build di Avalon Explorer scrive queste due righe da sola se il file c'è.

=== "In config/styles"

    Un file `config/styles/mystyle.lua` dentro `avalon_combat`:

    ```lua
    AvalonCombat.Styles.mystyle = {
        dict = 'avalon@anims@mystyle',
        -- ...
    }
    ```

    Uno stile scritto qui **vince** su quello di un pacchetto con lo stesso nome: per modificare uno stile di un pacchetto, copialo qui.

`config/styles/example.lua` contiene uno stile completo, commentato campo per campo.

### Campi di uno stile

| Campo | Descrizione |
|-------|-------------|
| `dict` | Dizionario delle animazioni (`.ycd`) |
| `damage` | Danno di un colpo del corpo (pugni, calci, artigli) |
| `idle` | Animazione in loop da fermo |
| `weapon` / `offhand` / `shield` | Cosa impugna (chiavi di `Weapons`, `Offhand`, `Shields`) |
| `hands` | Gli arti che colpiscono: `{ osso da, osso a, portata }` |
| `reach` / `radius` | Portata oltre l'ultimo osso e spessore degli arti (m) |
| `light` / `heavy` / `special` | Le catene della combo: liste di attacchi |
| `moves` | Le mosse della modalità `'keys'` |
| `sounds` | `swing`, `hit`, `knockdown`, `block`, `leap`, `dodge`, `boost`, `float` |
| `soundAt` | Fase a cui partono i suoni dei movimenti |
| `block` | Guardia: `{ start, loop, stop, hit }` con clip proprie, oppure `'blade'` / `'unarmed'`. Senza `block` lo stile non para |
| `stagger` / `knockdown` | Clip di reazione ai colpi |
| `move` | Movimento speciale: `kind = 'runner'` (scatto, schivata, balzo) o `kind = 'flyer'` (volo, boost) |
| `pairs` | Attacchi accoppiati |

### Un attacco

```lua
light = {
    { clip = 'attack@slash1', mult = 1.0 },
    { clip = 'attack@slash2', mult = 1.25, knockdown = true, soundAt = 0.35 },
},
```

| Campo | Descrizione |
|-------|-------------|
| `clip` | L'animazione |
| `mult` | Moltiplicatore del danno |
| `knockdown` | La vittima cade |
| `react` | La vittima reagisce anche se non è un attacco speciale |
| `sound` / `soundAt` | Suono proprio dell'attacco e fase a cui parte |
| `keepTurn` | Il ped resta girato come lo lascia l'animazione |
| `locked` | Bloccato finché la progressione non lo sblocca (`SetAttackEnabled`) |
| `id` | Nome dell'attacco per gli export (default: la clip) |

### Attacco accoppiato

```lua
pairs = {
    bite = { att = 'bite@att', vic = 'bite@vic', x = 0.5, y = 2.8, yaw = 196,
             mult = 4.0, damageAt = 0.45, sound = 'beast_roar', soundAt = 0.2 },
},
```

`att` / `vic` sono le clip di chi attacca e della vittima. `x`, `y` e `yaw` dicono dove sta la vittima rispetto a chi attacca (m, gradi). `damageAt` è la fase in cui arriva il danno. Si regola in gioco con `/avalon_pair_offset`.

!!! info "Movimento delle animazioni"
    Le clip che spostano il ped seguono il loro *mover*, letto da `data/clip_movers.lua`. Le clip di un pacchetto nuovo hanno bisogno dei loro mover, generati con Avalon Explorer; senza, la clip resta sul posto.

---

## NPC combattenti

Un NPC combattente è un ped di rete che combatte da solo con uno stile: cerca un bersaglio, si avvicina, attacca a combo, para, schiva, salta o vola.

- **Team:** i giocatori sono `'player'`. Un NPC attacca i giocatori (a meno che il suo team sia `'player'`) e gli NPC degli altri team, mai i ped normali.
- **Abilità:** `AvalonCombat.Npc` in `config/config.lua` contiene i valori di default (raggio di aggressione, distanza d'attacco, probabilità di parata, schivata, attacco accoppiato...). Ogni NPC può cambiarne alcuni con `opts.skill`.

```lua
-- server
local net = exports.avalon_combat:SpawnFighter('werewolf', vector3(-300.0, 790.0, 118.0), 90.0, {
    team = 'bandits',
    health = 400,
    skill = { BlockChance = 0.6, Weights = { heavy = 0.6 } },
})

-- più tardi
exports.avalon_combat:StopFighter(net)
```

| `opts` | Descrizione |
|--------|-------------|
| `model` | Modello del ped (default `AvalonCombat.Npc.Model`) |
| `weapon` / `offhand` / `shield` | Cosa impugna (default: quelli dello stile; `shield = false` = nessuno) |
| `team` | Team (default `'npc'`) |
| `health` | Vita |
| `skill` | Solo i valori di `AvalonCombat.Npc` che cambiano |

!!! warning "Serve un giocatore"
    `SpawnFighter` crea il ped sul client del giocatore più vicino alle coordinate: con il server vuoto ritorna `nil`.

---

## Progressione (livelli e abilità)

avalon_combat **non ha** un sistema di livelli o abilità: quello è del tuo server. Lo script dà solo gli strumenti per decidere, per ogni giocatore o NPC combattente, quali attacchi può usare e quanto è forte. Il tuo sistema chiama gli export, avalon_combat li applica.

### Quali attacchi può usare

- Ogni attacco è **disponibile**, a meno che la sua voce nello stile abbia `locked = true`: allora parte bloccato per tutti.
- `SetAttackEnabled(target, stile, attackId, true | false)` sblocca o blocca un attacco per quel combattente e quello stile. Vince sul `locked` del config.
- `attackId` è l'`id` dell'attacco, altrimenti il nome della sua clip. Per un attacco accoppiato è la sua chiave in `pairs`. `GetAttacks(stile)` elenca tutti gli attacchi di uno stile con id, catena e `locked`.
- Le catene della combo (`light`, `heavy`, `special`) e le `moves` della modalità `'keys'` sono voci diverse, con id diversi: se il server usa entrambe le modalità, sblocca entrambe.
- Un attacco bloccato viene saltato: nella combo il tasto passa al successivo disponibile della catena (nessuno disponibile: il tasto non fa niente); in modalità `'keys'` la mossa non viene scelta; R non esegue l'attacco accoppiato. Gli NPC usano solo i loro attacchi disponibili.
- Il server scarta i colpi di un attacco bloccato: un client modificato non lo può usare lo stesso.

### Quanto è forte

| Export | Effetto |
|--------|---------|
| `SetAttackMultiplier(target, stile, attackId, m)` | Moltiplica il danno di un attacco |
| `SetOutgoingMultiplier(target, m)` | Moltiplica tutto il suo danno |
| `SetDefence(target, { percent, flat })` | Difesa propria, sommata a quella della guardia o dello scudo |
| `SetMaxGuard(target, max)` | Punti guardia massimi (modalità `'points'`) |
| `SetInputMode(target, 'combo' \| 'keys')` | Modalità di input del combattente |
| `SetStyle(src, stile)` · `SetHeld(src, slot, chiave)` | Stile e armi del giocatore (per esempio dalla classe o dall'inventario) |

Ogni `Set...` ha il suo `Get...` per leggere il valore attuale.

!!! warning "Niente viene salvato"
    La progressione resta in memoria: quando il giocatore esce (o un NPC si ferma) torna ai valori del config. Salvarla è compito del tuo sistema, che la rimanda a ogni ingresso, per esempio quando il personaggio è caricato.

```lua
-- server: il tuo sistema di livelli
local Unlocks = { -- livello minimo per attacco, per stile
    katana = { ['combo@a03@rm'] = 5, ['combo@a04@rm'] = 10, ['combo@d03@rm'] = 15 },
}

local function applyProgress(src, level)
    for style, attacks in pairs(Unlocks) do
        for attackId, minLevel in pairs(attacks) do
            exports.avalon_combat:SetAttackEnabled(src, style, attackId, level >= minLevel)
        end
    end
    exports.avalon_combat:SetOutgoingMultiplier(src, 1.0 + level * 0.02) -- +2% di danno per livello
end

AddEventHandler('mylevels:loaded', applyProgress)  -- personaggio caricato
AddEventHandler('mylevels:levelUp', applyProgress) -- nuovo livello
```

---

## Export Disponibili

Un `target` è l'ID server di un giocatore oppure `{ net = netId }` per un NPC combattente.

### Export Server-Side

| Export | Descrizione |
|--------|-------------|
| `SetStyle(src, stile \| nil)` | Mette il giocatore in combattimento con quello stile, o lo toglie |
| `GetStyle(src)` | Stile attuale del giocatore |
| `SetHeld(src, slot, chiave \| nil)` | Arma (`'weapon'`, `'offhand'`) o scudo (`'shield'`) in mano |
| `GetHeld(src)` | `{ slot = chiave }` |
| `SpawnFighter(stile, coords, heading, opts)` | Crea un NPC combattente, ritorna il net id |
| `StopFighter(net)` | Toglie un NPC combattente |
| `AddHitHandler(fn)` / `RemoveHitHandler(id)` | Funzione chiamata a ogni colpo (vedi sotto) |
| `GetDamageMode()` | `'native'` o `'external'` |
| `React(target, cosa)` | Fa eseguire `'stagger'`, `'knockdown'` o `'death'` al bersaglio |
| `SetAttackEnabled(target, stile, attackId, on)` / `IsAttackEnabled(...)` | Sblocca o blocca un attacco per quel combattente |
| `SetAttackMultiplier(target, stile, attackId, m)` / `GetAttackMultiplier(...)` | Moltiplicatore di un attacco |
| `SetOutgoingMultiplier(target, m)` / `GetOutgoingMultiplier(target)` | Moltiplicatore su tutto il suo danno |
| `SetDefence(target, { percent, flat })` / `GetDefence(target)` | Difesa propria, sommata a quella della guardia |
| `GetGuard` / `SetGuard` / `GetMaxGuard` / `SetMaxGuard` | Punti guardia (modalità `'points'`) |
| `SetInputMode(target, 'combo' \| 'keys' \| nil)` / `GetInputMode(target)` | Modalità di input del combattente |
| `GetStyles()` | Tutti gli stili caricati |
| `GetAttacks(stile)` | `{ { id, chain, clip, mult, locked, input } }` |
| `GetWeapon(chiave)` / `GetShield(chiave)` | Voce del config |

#### Il colpo (`hit`)

Gli hit handler e l'evento `avalon_combat:hit` ricevono questa tabella:

| Campo | Descrizione |
|-------|-------------|
| `mode` | `'native'` o `'external'` |
| `attacker` | `{ kind = 'player' \| 'fighter', src, net, style, team }` |
| `victim` | `{ kind = 'player' \| 'fighter' \| 'ped', src, net, style }` |
| `attack` | `{ id, chain, clip, mult, knockdown, react }` (`chain`: `light`, `heavy`, `special`, `moves`, `pair`) |
| `weapon` | `{ slot, key, data }` se ha colpito una lama, altrimenti `nil` |
| `base` | Danno dell'arma o dello stile |
| `attackMult`, `progressMult`, `outgoingMult` | I tre moltiplicatori |
| `damage` | Danno prima della guardia |
| `guard` | `{ blocked, parried, broken, percent, flat, cost }` |
| `final` | Danno dopo la guardia (quello applicato in `'native'`) |
| `direction` | `{ x, y }` da chi attacca alla vittima |

Gli handler partono dopo il calcolo della guardia. Un handler ritorna il colpo modificato, `nil` per lasciarlo com'è o `false` per annullarlo. Gli handler di una risorsa che si ferma vengono rimossi da soli.

```lua
-- server: resistenze e livelli
exports.avalon_combat:AddHitHandler(function(hit)
    if hit.victim.kind == 'player' and HasSilverArmor(hit.victim.src) and hit.attacker.style == 'werewolf' then
        hit.final = hit.final * 0.5 -- arrotondato dal server dopo gli handler
        return hit
    end
end)
```

```lua
-- server: DamageMode = 'external', vita gestita dal tuo sistema
AddEventHandler('avalon_combat:hit', function(hit)
    if hit.victim.kind ~= 'player' then return end
    local hp = MyHealth.Remove(hit.victim.src, hit.final)
    if hp <= 0 then exports.avalon_combat:React(hit.victim.src, 'death') end
end)
```

**Eventi server** (`AddEventHandler`, ricevono il colpo): `avalon_combat:hit` (modalità `'external'`), `avalon_combat:blocked`, `avalon_combat:parried`, `avalon_combat:guardBroken`.

---

### Export Client-Side

| Export | Ritorna |
|--------|---------|
| `GetStyle()` | Stile attuale o `nil` |
| `IsBlocking()` | `boolean` |
| `IsAttacking()` | `boolean` |
| `GetGuard()` | `{ v, max }` punti guardia (modalità `'points'`) |
| `GetStamina()` | Stamina del gioco, come la legge la modalità `'stamina'` |
| `SetCombatEnabled(on)` | `false` lascia i comandi al gioco (menu, cutscene) finché non torna `true` |
| `GetCastPoint()` | `vector3` da cui partono le magie del bastone, o `nil` |
| `GetInputMode()` | `'combo'` o `'keys'` |
| `MakeFighter(ped, stile, opts)` / `StopFighter(ped)` | Trasforma un ped in combattente, o lo ferma |

**Evento locale** `avalon_combat:onHurt` `(ped, info, attacker)`: a ogni colpo preso da un ped gestito da questo client, per un HUD.

```lua
-- client: barra vita e numeri di danno
AddEventHandler('avalon_combat:onHurt', function(ped, info, attacker)
    if ped ~= PlayerPedId() then return end
    if info.parried then
        ShowText('PARATA!')
    elseif info.blocked then
        ShowText(('-%d (parato)'):format(info.damage))
    else
        ShowText(('-%d'):format(info.damage))
    end
end)

-- client: niente combattimento mentre il menu è aperto
exports.avalon_combat:SetCombatEnabled(false)
```
