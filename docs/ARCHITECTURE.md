# POKÉDEAD — Code Architecture

How the single `pokedead.html` is organized internally. This is a map of where to look for what.

## File structure

```
pokedead.html
├── <head>
│   └── <style>           CSS (~10 KB) — all styling
└── <body>
    ├── #launcher         Launcher screen markup
    ├── #char-creation    Character creation screen markup
    ├── #toast            Floating notification
    ├── #layout           Main game layout
    │   ├── #game         The game console
    │   │   ├── #header   Title bar + zone label
    │   │   ├── #stats    HP / Hunger / Vaccines / Day
    │   │   ├── #main     Canvas + overlays
    │   │   │   ├── #canvas        Game world rendering
    │   │   │   ├── #overlay       Modals (combat, menu, etc.)
    │   │   │   └── #dialog-box    NPC dialog
    │   │   ├── #log      Action log
    │   │   └── #controls Action buttons + d-pad
    │   └── #quest-panel  Right-side quest tracker
    └── <script>          ~60 KB game engine
```

## CSS sections (in order)

1. **Reset & body** — base typography
2. **Launcher** — title art, ASCII Pokéball, menu buttons, language toggle
3. **Char creation** — gender selector, name input
4. **Layout** — flex container for game + quest panel, responsive breakpoints
5. **Header / Stats / Main / Log / Controls** — game console internals
6. **D-pad** — directional control grid
7. **Horde alert** — pulsing top banner
8. **Quest panel** — sidebar styling
9. **Overlays & modals** — combat, inventory, team modals
10. **Dialog box** — portrait + text + typewriter cursor
11. **Combat stage** — animations (hit flash, screen shake, dmg numbers)
12. **Damage numbers / banners** — floating animations
13. **Team / Inventory grids** — modal layouts
14. **Toast** — global notification
15. **Media queries** — mobile breakpoints (1080px / 480px)

## JS sections (in order, denoted by `// ============ X ============` headers)

| Section | Purpose | Key entries |
|---|---|---|
| **TRANSLATIONS** | Language tables + `t()` helper | `T_EN`, `T_FR`, `LANG`, `t(key, params)`, `setLanguage()` |
| **TILES & ZONES** | Map data | `TILE_SIZE`, `TILES`, `ZONES` (each zone has `name`, `width`, `height`, `map`) |
| **SPRITES** | Pixel-art sprite strings | `SPRITES` (Pokémon), `HUMANS` (people) |
| **POKEMON DATA** | Stats and metadata | `STARTERS`, `ZOMBIES`, `CURED`, `TYPECHART`, `effectiveness()` |
| **QUESTS** | Quest definitions | `QUEST_DEFS` (id → type + objectives) |
| **STATE** | Runtime game state | `G` global object — see below |
| **UTIL** | Helpers | `$()`, `showToast()`, `logMsg()` |
| **SPRITE / DRAWING** | Pixel rendering | `drawSprite()`, `drawPortrait()` |
| **MAP RENDERING** | Tiles, hordes, NPCs | `drawMap()`, `drawTileDetail()`, `drawHouseSpecial()`, `drawLabSpecial()`, `drawZzZ()`, `drawHordeOnMap()`, `drawQuestMark()` |
| **STATS / QUEST UI** | Right panel + top bar | `refresh()`, `refreshQuests()`, `startQuest()`, `completeObjective()` |
| **DIALOG** | Cutscenes | `showDialog()`, `nextDialogLine()`, `typeText()`, `closeDialog()` |
| **HORDES** (v0.3) | Detection + chase | `updateHordes()`, `tryHide()`, `triggerHordeCombat()` |
| **MOVEMENT** | Tile-by-tile + interactions | `move(dx, dy)`, `handleGate()`, `handleHouseDoor()`, `handleHouseExit()`, `handleBed()`, `tryLoot()` |
| **NPC INTERACTIONS** | Dialog branching | `interactNPC(npc)` |
| **STARTER SELECT** | Choose starter modal | `showStarterSelect()`, `pickStarter()`, `startChenFight()` |
| **ENCOUNTERS** | Random combat trigger | `triggerEncounter()` |
| **COMBAT** | Battle screen + actions | `startCombat()`, `showCombatScreen()`, `combatAttack()`, `combatVaccinate()`, `combatItem()`, `combatRun()`, `combatSwitch()`, `enemyTurn()`, `endCombat()`, `capture()`, `spawnDmgNumber()`, `spawnEffBanner()`, `shakeCombat()`, `refreshCombatHP()` |
| **MENUS** | Inventory / team / settings | `openInventory()`, `openTeam()`, `openMenu()`, `useItem()`, `usePokemonHeal()`, `setActive()`, `closeOverlay()`, `showHelp()`, `restRecover()` (v0.2) / `doSleep()` (v0.3) |
| **CONTROLS** | Render action buttons | `renderControls()`, `actionInteract()` |
| **KEYBOARD** | Key event listener | `window.addEventListener('keydown', ...)` |
| **SAVE / LOAD** | Persistence | `autoSave()`, `hasContinue()`, `loadFromLocalStorage()`, `applySaveData()`, `exportSave()` |
| **BOOT** | Launcher → game flow | `bootLauncher()`, char creation handlers, `startGameFromState()`, `startAnimLoop()` |

## Global state object (`G`)

```js
const G = {
  mode: 'launcher' | 'starter' | 'exploring' | 'dialog' | 'combat' | 'menu' | 'gameover',
  
  player: {
    name, gender,
    x, y,                    // current zone coords
    hp, mhp,
    hunger, mhunger,
    prevZone, prevX, prevY   // for house interior return
  },
  
  zone: 'bourg_palette' | 'kanto' | 'house_kanto_1' | ...,
  
  inv: { food, medkit, vaccine, antibite },
  
  team: [
    { key, name, type, mhp, hp, atk, sprite,
      isZombie, cured, bites, zombified }
  ],
  active: 0,  // index of active Pokémon
  
  combat: { enemy, turn, locked } | null,
  
  day, steps,
  
  looted: { zoneId: { 'x,y': 1 } },
  flags: { introStarted, introDone, motherSaved, ... },
  quests: { qid: { active, completed, objDone: { oid: bool } } },
  log: [{ text, cls }],
  
  npcs: { zoneId: [{ id, x, y, sprite, visible }] },
  hordes: { zoneId: [{ id, x, y, enraged, defeated }] },
  hordeChase: { hordeId, zoneId, hideTurnsLeft } | null,
  houses: { houseId: { clean, hasZombie, hasBody, lootTaken } }
};
```

## Sprite system

Sprites are 16×16 pixel grids encoded as strings, one per row. Each character maps to a color in the per-sprite palette.

```js
SPRITES.bulbasaur = {
  p: ["................",       // row 0: all transparent
      "....11....11....",        // row 1: '1' = dark green ear
      "...1331..1331...",        // ...
      ...
      ],
  pal:  { 1:'#3a4d2e', 2:'#222', 3:'#7fb069', 5:'#5a8a45', 'a':'#fff' },
  zpal: { 1:'#1f2a18', 2:'#c00',  3:'#5a6d4a', 5:'#3a5a30', 'a':'#8a1010' }
};
```

`pal` is the normal palette, `zpal` is the zombie variant (drawn when `isZombie` flag is true). Use `'.'` for transparent pixels.

Each row **must** be exactly 16 characters. There's a check script in `CLAUDE.md` for verifying.

## Zone map encoding

Each zone is a string array. Each character represents a tile:

```js
ZONES.bourg_palette = {
  name: { en: 'Pallet Town', fr: 'Bourg Palette' },
  width: 12, height: 10,
  map: [
    "MMMMMGMMMMMM",   // M=mountain, G=gate
    "MFF......FFM",   // F=forest, .=grass
    "MF.......L.M",   // L=lab
    ...
  ]
};
```

Each row must be exactly `width` characters. Tile chars are defined in `TILES`.

## Animation loop

A single `requestAnimationFrame` loop runs while `G.mode === 'exploring'`, redrawing the map every ~100 ms. This is what animates:
- Bouncing quest markers (`!` / `?`)
- Pulsing horde borders (enraged)
- Floating "ZzZ" above beds

Combat animations are pure CSS keyframes triggered by adding/removing classes — no JS animation.

## Translation key naming convention

```
menu.*           Launcher menu
cc.*             Character creation
hd.*             Header
stat.*           Stat labels
qp.*             Quest panel
btn.*            Buttons (generic)
tile.*           Tile names
item.*           Items (incl. .desc)
combat.*         Combat strings
log.*            Log messages
help.*           Help modal
gameover.*       Game over screen
type.*           Type names
pkm.*            Pokémon names
quest.{id}.*     Quest texts (.title, .desc, {oid} for objectives)
chen.*           Chen's dialogue
bertrand.*       Bertrand's dialogue
open.*           Opening cinematic
pl.*             Player thoughts
horde.*          Horde messages
house.*          House interior messages
mom.*            Mom-related strings
starter.*        Starter selection
win.*            Victory
```

Use `t('key', { name: G.player.name })` to interpolate variables (`%name%` in the string).

## Save format

The save file is a JSON serialization of `G`. To load, parse and `Object.assign(G, data)`. Note: NPCs and hordes have to be re-initialized from defaults if missing in a save — see `applySaveData()` for the upgrade logic.

## Adding a new feature — checklist

1. Identify which section of the JS the feature belongs to (e.g. a new combat action goes in COMBAT).
2. Identify any new state — add fields to `G` and update `applySaveData()` for backward compatibility.
3. Identify any new strings — add to `T_EN` and `T_FR`.
4. Identify any new tiles / sprites — add to respective dictionaries.
5. Wire up to existing handlers (movement, combat actions, render functions).
6. Test syntax (the snippet in CLAUDE.md).
7. Play-test in browser.
8. Commit.

## Things that are easy to break

- **Sprite row width** — must be 16 chars exactly.
- **Map row width** — must be `zone.width` chars exactly.
- **Tile char registration** — every char used in a zone map must exist in `TILES`.
- **Translation keys** — typos silently fall through to the key itself. Search `t('` to audit.
- **Save backward compat** — if you add fields to `G`, existing saves won't have them. Use `?? defaultValue` patterns and update `applySaveData()`.
- **Nested overlays** — `G.mode` must be reset to 'exploring' when closing overlays, otherwise input is dead.
- **Combat locked flag** — `G.combat.locked = true` during animation, must be cleared in `enemyTurn()` or after action. If not cleared, combat softlocks.
