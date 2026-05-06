# POKÉDEAD — Code Architecture

Complete reference for `pokedead.html` internals. Use this to find exactly where to make any change.

---

## File layout

```
pokedead.html
├── <head>
│   └── <style>         CSS (~12 KB)
└── <body>
    ├── #launcher        Launcher screen
    ├── #char-creation   Character creation
    ├── #toast           Floating notification
    ├── #layout          Main game (flex: #game + #quest-panel)
    │   ├── #game
    │   │   ├── #header  Title + zone label
    │   │   ├── #stats   HP / Hunger / Vaccines / Day
    │   │   ├── #main    Canvas + overlays
    │   │   │   ├── #canvas        Game world
    │   │   │   ├── #overlay       Combat / inventory / modals
    │   │   │   └── #dialog-box    NPC dialog (portrait + typewriter)
    │   │   ├── #log     Action log (last ~10 lines)
    │   │   └── #controls Action buttons + d-pad
    │   └── #quest-panel  Right sidebar
    └── <script>         ~97 KB game engine
```

---

## All tile characters

### Exterior tiles (used in overworld zone maps)

| Char | Name | pass | Special |
|---|---|---|---|
| `.` | grass | ✅ | enc=0.18 |
| `F` | forest | ✅ | enc=0.32, hide during horde chase |
| `R` | road | ✅ | enc=0.05 |
| `C` | city | ✅ | enc=0.40, loot=city |
| `H` | hospital | ✅ | enc=0.20, loot=hospital |
| `X` | store | ✅ | enc=0.08, loot=store |
| `L` | lab | ❌ | interact → `handleHouseDoor()` |
| `h` | house | ❌ | interact → `handleHouseDoor()` |
| `P` | PokéCenter | ❌ | interact → `handleHouseDoor()` |
| `Q` | PokéMart | ❌ | interact → `handleHouseDoor()` |
| `A` | apartment | ❌ | interact → `handleHouseDoor()` |
| `M` | mountain | ❌ | blocked |
| `W` | water | ❌ | blocked |
| `G` | gate | ✅ | interact → `handleGate()` |

### Interior tiles (used in house/lab zone maps)

| Char | Name | pass | Special |
|---|---|---|---|
| `f` | floor | ✅ | — |
| `d` | door (exit) | ✅ | interact → `handleHouseExit()` |
| `b` | bed | ✅ | interact → `handleBed()` |
| `t` | stairs up | ✅ | interact → `handleStairs()` |
| `s` | shelf | ❌ | loot=house (bump to loot) |
| `S` | lab shelf | ❌ | loot=lab_shelf (bump to loot) |
| `w` | wall | ❌ | blocked |
| `e` | equipment | ❌ | blocked (decorative) |

**Loot tables:**

| Type | Drops |
|---|---|
| `house` | food (55%) or medkit (45%) |
| `lab_shelf` | vaccine (45%), medkit (35%), food (20%) |
| `hospital` | medkit (60%), vaccine (25%), food (15%) |
| `store` | food (65%), medkit (35%) |
| `city` | food (50%), medkit (30%), vaccine (20%) |

---

## Global state object `G`

```js
G = {
  mode: 'launcher' | 'starter' | 'exploring' | 'dialog' | 'combat' | 'menu' | 'gameover',

  player: {
    name, gender,
    x, y,                     // current tile position
    hp, mhp,                  // health
    hunger, mhunger,          // hunger (0→starvation)
    prevZone, prevX, prevY    // saved before entering interior (restored on exit)
  },

  zone: string,               // current zone ID (e.g. 'bourg_palette', 'h_player')

  inv: { food, medkit, vaccine, antibite },

  team: [{
    key, name, type, mhp, hp, atk, sprite,
    isZombie, cured,          // isZombie=caught as zombie; cured=successfully vaccinated
    bites,                    // 0–3; at 3 → zombified=true
    zombified                 // loses control in combat
  }],
  active: 0,                  // index of active Pokémon

  combat: {
    enemy: { key, name, type, mhp, hp, atk, sprite, isZombie, isHuman, boss, introBoss },
    turn: 'player' | 'enemy',
    locked: bool              // true during animations — MUST be reset or combat softlocks
  } | null,

  day: number,
  steps: number,

  looted: { zoneId: { 'x,y': 1 } },   // looted shelves and tiles
  flags: {
    introStarted, introDone,
    motherSaved,              // unlocks player house, triggers win condition
    // ...other story flags
  },
  quests: { qid: { active, completed, objDone: { oid: bool } } },
  log: [{ text, cls }],

  npcs: { zoneId: [{ id, x, y, sprite, visible }] },
  hordes: { zoneId: [{ id, x, y, enraged, defeated }] },
  hordeChase: { hordeId, zoneId, hideTurnsLeft } | null,
  houses: { houseId: { cleared, lootTaken: {} } }
}
```

---

## Key functions

### Movement & navigation

| Function | Trigger | What it does |
|---|---|---|
| `move(dx, dy)` | arrow keys / d-pad | Main movement logic — gates, doors, encounters, hunger |
| `handleGate(nx, ny)` | stepping on `G` tile | Switches zone via `zone.gates[key]` |
| `handleHouseDoor(nx, ny)` | bumping `h/L/P/Q/A` tile | Saves prevZone, switches to interior zone; triggers Chen lab / Cinnabar lab auto-dialogs |
| `handleHouseExit()` | stepping on `d` tile | If zone has `parent` → go to parent zone. Else → restore prevZone. |
| `handleStairs(nx, ny)` | stepping on `t` tile | Looks up `zone.stairs[key]`, switches to target zone |
| `handleBed()` | stepping on `b` tile | `doSleep()` — heals HP, advances day |
| `tryLoot(type, key)` | bumping `s/S` or city tile | Rolls loot table, adds to `G.inv` |

### Combat

| Function | What it does |
|---|---|
| `startCombat(enemy)` | Sets `G.mode='combat'`, initializes `G.combat`, calls `showCombatScreen()` |
| `showCombatScreen()` | Renders combat HTML in `#overlay` — enemy + ally sprites, HP bars, bite marks, action buttons |
| `combatAttack()` | Player attack → damage calc → `enemyTurn()` after timeout |
| `combatVaccinate()` | Cures zombified ally OR captures enemy (blocked if `isHuman`) |
| `combatItem()` | Uses medkit (+25 HP to active Pokémon) |
| `combatRun()` | 60% flee chance (blocked if `boss` or `introBoss`) |
| `combatZombieAttack()` | Uncontrolled attack when active Pokémon is zombified |
| `enemyTurn()` | Enemy attacks, applies bite mark, checks faint, clears `locked` |
| `endCombat(result)` | Cleans up `G.combat`, handles introBoss post-dialog, loot drops |
| `capture()` | Converts enemy to cured Pokémon and adds to team |

### House system

```
handleHouseDoor(nx, ny)
  └── looks up zone.houses['nx,ny']
      ├── if id === 'h_bourg_player' && !G.flags.motherSaved → blocked
      ├── saves prevZone/prevX/prevY
      └── switches to interior zone

handleHouseExit()
  ├── if current zone has .parent → go to parent zone (upper → lower floor)
  └── else → restore prevZone (house → overworld)

handleStairs(nx, ny)
  └── looks up zone.stairs['nx,ny'] → switches to target zone
```

### Multi-floor building pattern

```js
// Ground floor zone definition:
h_immeuble_g: {
  width: 10, height: 8, entry: [4, 6],
  stairs: { '3,3': { zone: 'h_immeuble_1', entry: [3, 6] } },
  map: [...]
}

// Upper floor zone definition:
h_immeuble_1: {
  width: 10, height: 8,
  parent: 'h_immeuble_g',    // ← exit goes here
  entry: [3, 6],
  entry_parent: [3, 3],      // ← where player lands on parent floor
  map: [...]
}
```

---

## Sprite system

Sprites are 16×16 grids as string arrays. Each char maps to a color in the per-sprite palette.

```js
SPRITES.bulbasaur = {
  p: ["................", ...],    // 16 rows of exactly 16 chars; '.' = transparent
  pal:  { '1': '#3a4d2e', ... },   // normal palette
  zpal: { '1': '#1f2a18', ... }    // zombie palette (used when isZombie=true)
}
```

`HUMANS` object: same structure, for human characters (`player_boy`, `player_girl`, `chen`, `chen_zombie`, `bertrand`, `mother_zombie`, `z_human`).

`drawSprite(ctx, key, ox, oy, scale, isZombie)` — looks up key in `SPRITES` then `HUMANS`.

**Rule:** every row must be exactly 16 chars. Validate before commit.

---

## Translation system

```js
t('key')                          // simple lookup
t('key', { name: 'Alice' })      // interpolation (%name% in string)
t('tile.' + tileKey)              // tile names
t('quest.q_id.obj_id')           // quest objectives
```

**Key namespace convention:**

```
menu.*       launcher
cc.*         character creation
hd.*         header
stat.*       stats bar
qp.*         quest panel
btn.*        buttons
tile.*       tile names
item.*       items
combat.*     combat strings
log.*        log messages
help.*       help modal
gameover.*   game over
type.*       type names
pkm.*        Pokémon names
quest.*      quest texts
chen.*       Chen's lines
bertrand.*   Bertrand's lines
open.*       opening cinematic
pl.*         player thoughts
horde.*      horde alerts
house.*      interior messages
mom.*        Mom-related
starter.*    starter selection
win.*        victory screen
```

---

## Zone map encoding

```js
ZONES.jadielle = {
  name: 'Jadielle',       // display name (string or {en,fr})
  width: 14,              // chars per row
  height: 10,             // number of rows
  map: [                  // array of exactly `height` strings, each `width` chars
    "MMMMGMMMMMMMMM",
    ...
  ],
  gates: {                // 'x,y' → { zone: id, x, y } (destination coords)
    '4,0': { zone: 'route_2', x: 4, y: 6 }
  },
  houses: {               // 'x,y' → { zone: id, id: houseId }
    '11,2': { zone: 'h_pcenter_dmg', id: 'h_jad_pc' }
  },
  encounters: ['z_rattata', 'z_pidgey']   // weighted random pool
}
```

Interior zones additionally have:
- `entry: [x, y]` — where player spawns on entry
- `stairs: { 'x,y': { zone, entry } }` — stair destinations (optional)
- `parent: 'zone_id'` — upper floors only; exit returns here
- `entry_parent: [x, y]` — where player lands on parent after exit (optional)

---

## Adding a new feature — checklist

1. Identify JS section (TILES & ZONES / COMBAT / MOVEMENT / etc.)
2. Add any new state fields to `G` → update `applySaveData()` for backward compat
3. Add new strings to `T_EN` and `T_FR`
4. Add new tiles to `TILES` + `drawTileDetail()` case
5. Add new sprites to `SPRITES` or `HUMANS` (validate 16×16 rows)
6. Wire to handlers in `move()` / `combatVaccinate()` / etc.
7. **Validate syntax** with node command
8. **Validate zone widths** if any map was changed
9. Play-test in browser
10. Commit
