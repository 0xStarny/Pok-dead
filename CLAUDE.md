# POKÉDEAD — Claude Context (PRIMARY ENTRY POINT)

> **Read this file first. It is the source of truth for current project state.**

---

## What this is

POKÉDEAD is a **single-file HTML/JS/CSS browser game** — Pokémon × Walking Dead. Turn-based RPG, zombie apocalypse, emotional drama. Player wakes from a 2-week coma, must reach Cinnabar Island to synthesize a human vaccine and save their zombified mother.

**Stack:** plain HTML + CSS + vanilla JS. No build step, no dependencies. Double-click `pokedead.html` to run.

**Single-file rule:** everything lives in `pokedead.html`. No splits, no modules. ~97 KB of JS.

---

## Current version: v0.3 ✅ COMPLETE

All v0.3 features are implemented and working. See [docs/CHANGELOG.md](docs/CHANGELOG.md) for full history.

### What's in the game right now

| Area | Status |
|---|---|
| Launcher (title art, New/Continue/Import) | ✅ |
| Character creation (boy/girl + name) | ✅ |
| EN/FR translation system (`t()` helper, language toggle) | ✅ |
| Intro dialogue with Prof. Chen (2-week coma, Mom infected, starter choice) | ✅ |
| Chen transforms → boss fight (Chen himself, not his Pokémon) | ✅ |
| 3 starters: Bulbizarre / Salamèche / Carapuce | ✅ |
| Full Kanto map: 16 overworld zones (Bourg Palette → Cinnabar) | ✅ |
| 30+ interior zones (houses, labs, PokéCenters, PokéMarts, apartments) | ✅ |
| Multi-floor buildings with stairs (h_immeuble_g → h_immeuble_1) | ✅ |
| NPC quest markers (animated `!` / `?`) | ✅ |
| Quest panel (WoW-style, main orange / side yellow) | ✅ |
| Bertrand side quest (2 rations → 3 vaccines) | ✅ |
| Marc NPC + q_mortimer side quest (Lavandia) | ✅ |
| Turn-based combat (attack / vaccinate / heal / flee) | ✅ |
| Type effectiveness, crits, floating damage numbers | ✅ |
| Vaccine capture mechanic (HP-scaled success rate) | ✅ |
| Bite mark system (3 bites = zombification, vaccine cures) | ✅ |
| Human zombies (`z_human`, `z_chen`) — vaccine blocked on humans | ✅ |
| Horde system (detection r=5, chase, forest hide 3 turns) | ✅ |
| House interiors with loot, optional beds | ✅ |
| Bed-only sleep (no free Rest button) | ✅ |
| Loot: hospital / store / city / house / lab_shelf | ✅ |
| Hunger system (→ HP drain → game over) | ✅ |
| Mom's house locked (opens only after motherSaved flag) | ✅ |
| Save: auto-save localStorage + export/import .save files | ✅ |
| Game over on HP=0 or starvation | ✅ |

### What's NOT in the game yet (v0.4+)

- Win condition (synthesize vaccine, save Mom — Cinnabar lab trigger exists but no ending)
- Acts 2–6 narrative content (see [docs/LORE.md](docs/LORE.md))
- Day/night visual cycle
- Full NPC roster per city (story characters, traders)
- Audio

---

## How to run

```
Double-click pokedead.html
```

No npm, no server, no setup.

---

## How to validate after any JS change

```bash
node -e "const fs=require('fs');const html=fs.readFileSync('pokedead.html','utf8');const m=html.match(/<script[^>]*>([\s\S]*?)<\/script>/);try{new Function(m[1]);console.log('OK',m[1].length);}catch(e){console.log('ERR:',e.message);}"
```

## How to validate zone row widths

```bash
node << 'EOF'
const fs=require('fs'),html=fs.readFileSync('pokedead.html','utf8');
const m=html.match(/const ZONES=\{([\s\S]*?)\};\s*function S/);
const zones=new Function('return {'+m[1]+'}')();
let ok=true;
for(const [id,z] of Object.entries(zones)){
  if(!z.map)continue;
  z.map.forEach((row,i)=>{if(row.length!==z.width){console.log('ERR',id,'row',i,'len',row.length,'exp',z.width);ok=false;}});
}
if(ok)console.log('ALL OK');
EOF
```

---

## Code conventions

- **Single-file rule.** Everything in `pokedead.html`. No splits.
- **Surgical edits only.** Find the right section, add there. Don't rewrite the whole file.
- **Validate syntax after every batch.** Use the node command above.
- **Validate zone row widths** after any map edit.
- **Sprite rows must be exactly 16 chars.** Use `.` for transparent pixels.
- **All user-visible strings go through `t(key, params)`** — add to both `T_EN` and `T_FR`.
- **Save backward compat:** if you add fields to `G`, update `applySaveData()` with `?? default`.
- **Test `G.combat.locked`** — must be set `false` at end of every combat branch or it softlocks.

---

## JS sections (in order inside `<script>`)

| Section header | What's there |
|---|---|
| `TRANSLATIONS` | `T_EN`, `T_FR`, `LANG`, `t()`, `setLanguage()` |
| `TILES & ZONES` | `TILE_SIZE=32`, `TILES`, `ZONES` |
| `SPRITES` | Pokémon pixel-art (16×16 strings + pal/zpal) |
| `POKEMON DATA` | `STARTERS`, `ZOMBIES`, `CURED`, `TYPECHART`, `effectiveness()` |
| `QUESTS` | `QUEST_DEFS` |
| `STATE` | `G` global object |
| `UTIL` | `$()`, `showToast()`, `logMsg()` |
| `SPRITE / DRAWING` | `drawSprite()`, `drawPortrait()` |
| `MAP RENDERING` | `drawMap()`, `drawTileDetail()`, `drawHouseSpecial()`, `drawLabSpecial()` |
| `STATS / QUEST UI` | `refresh()`, `refreshQuests()`, `startQuest()`, `completeObjective()` |
| `DIALOG` | `showDialog()`, `typeText()`, `closeDialog()` |
| `HORDES` | `updateHordes()`, `tryHide()`, `triggerHordeCombat()` |
| `MOVEMENT` | `move()`, `handleGate()`, `handleHouseDoor()`, `handleHouseExit()`, `handleStairs()`, `handleBed()`, `tryLoot()` |
| `NPC INTERACTIONS` | `interactNPC()` |
| `STARTER SELECT` | `showStarterSelect()`, `pickStarter()`, `startChenFight()` |
| `ENCOUNTERS` | `triggerEncounter()` |
| `COMBAT` | `startCombat()`, `showCombatScreen()`, `combatAttack()`, `combatVaccinate()`, `combatItem()`, `combatRun()`, `combatSwitch()`, `combatZombieAttack()`, `enemyTurn()`, `endCombat()`, `capture()` |
| `MENUS` | `openInventory()`, `openTeam()`, `openMenu()`, `useItem()`, `closeOverlay()`, `doSleep()` |
| `CONTROLS` | `renderControls()`, `actionInteract()` |
| `KEYBOARD` | `window.addEventListener('keydown', ...)` |
| `SAVE / LOAD` | `autoSave()`, `hasContinue()`, `loadFromLocalStorage()`, `applySaveData()`, `exportSave()` |
| `BOOT` | `bootLauncher()`, char creation handlers, `startGameFromState()`, `startAnimLoop()` |

---

## Key gotchas (common bugs)

| Symptom | Cause |
|---|---|
| Combat softlocks after action | `G.combat.locked` not reset to `false` at end of branch |
| Input dead after overlay | `G.mode` not reset to `'exploring'` on overlay close |
| Zone appears black | Map row width ≠ `zone.width`, or tile char not in `TILES` |
| Translation shows key name | Typo in key, or key missing from `T_EN`/`T_FR` |
| Save breaks on load | New `G` field added without updating `applySaveData()` |
| Sprite invisible | Key not in `SPRITES` or `HUMANS`, or row length ≠ 16 |
| House door does nothing | `zone.houses` mapping missing for that tile coord |

---

## Document index

| File | Purpose |
|---|---|
| **CLAUDE.md** (this file) | Primary context — state, conventions, entry point |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Complete tile table, zone list, G state, function index |
| [docs/ZONES.md](docs/ZONES.md) | All zone IDs, connections, house mappings, entry coords |
| [docs/LORE.md](docs/LORE.md) | Story canon — **do not contradict** |
| [docs/GAMEPLAY.md](docs/GAMEPLAY.md) | Mechanics reference (type chart, loot tables, combat math) |
| [docs/WORLD.md](docs/WORLD.md) | Narrative zone guide (atmosphere, NPCs, planned quests) |
| [docs/ROADMAP.md](docs/ROADMAP.md) | v0.3 done, v0.4 features planned |
| [docs/CHANGELOG.md](docs/CHANGELOG.md) | Version history |

---

## Working method

The owner prefers **incremental edits from a working baseline**:
1. One feature at a time
2. Validate syntax after each change
3. Validate zone widths after any map edit
4. Commit after each working feature
5. Never rewrite the whole file

The game is deployable as-is to Vercel (`vercel.json` rewrites `/` → `/pokedead.html`).
