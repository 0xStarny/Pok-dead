# POKÉDEAD — LLM Session Context

> **For any AI picking up this project:** read this file + CLAUDE.md before touching anything.
> This document summarizes what has been built, what was changed recently, and the current state of the codebase.

---

## TL;DR

Single-file HTML game (`pokedead.html`). Everything is in that one file. No build step, no npm, no modules. ~142 KB of JS inside `<script>`. Edit surgically, validate syntax after every change, commit after each working feature.

**GitHub:** `0xStarny/Pok-dead` — main branch, auto-deploys to Vercel.

---

## What was built in this session (major additions since v0.2 baseline)

### Engine & systems

| Feature | Where in code | Notes |
|---|---|---|
| **Camera scrolling** | `getCamera()`, `drawMap()` | `VIEW_W=19, VIEW_H=13` viewport, lerp smoothing (~22%/frame), resets on zone change. Module-level `_camLerpX/_camLerpY` vars. |
| **Bigger maps** | `ZONES.bourg_palette`, `ZONES.route_1` | bourg_palette 14×12 with Y-tree borders; route_1 10×14 with `j` tall-grass patches |
| **Leveling system** | `LEARNSETS`, `buildMoveSet()`, `gainXP()` | Species-based learnsets, XP per combat kill, level-up message, `G.team[i].level` |
| **Multi-attack moves with PP** | `MOVES`, `combatMove()` | 18 typed moves, power/accuracy/PP, STAB +50% bonus, depleted PP forces tackle fallback |
| **Type-specific bites** | `enemyTurn()` | Each zombie uses a typed bite (ghost = Spectral Bite bypasses def, etc.) |
| **Bite regen on rest** | `doSleep()` | Each non-active Pokémon loses 1 bite per sleep |
| **House zombie system** | `spawnHouseZombieNpcs()`, `move()` | On house entry, spawns 0–3 zombie NPCs visible on map at floor tiles ≥3 from entry; collide → combat; post-combat removes NPC by id |
| **Achievements** | `ACHIEVEMENTS`, `unlockAch()`, `checkAchievements()` | Dict of ~12 achievements, tracked in `G.flags.ach`, toast on unlock |
| **Heal pad tiles** | `+` tile in TILES, `handleHealPad()` | One-time heal tile in PokéCenters; flash animation on use (`G.flags.healFlash`) |
| **Walking dust particles** | `spawnWalkDust()` | Tiny canvas particles on each step |
| **Pickup text floats** | `spawnPickupText()` | "+food", "+vaccine" floats on loot |
| **Day banner** | `showDayBanner()` | "Jour N" banner fades in on new day |
| **Cure pulse** | `curePulse()` | Green ring pulse on successful vaccine capture |
| **Quest celebration** | `questCelebrate()` | Confetti burst on quest complete |
| **Dialog cursor blink** | in `showDialog()` | Blinking ▼ cursor at dialog bottom |
| **Best-move marker** | `showCombatScreen()` | ★ on move button with highest expected damage |
| **Heal flash** | `drawMap()` | Orange/gold flash animation when heal pad activates |
| **Team panel** | `openTeam()` | Sidebar team view with bite marks, level, moves, PP |
| **Capture % display** | `showCombatScreen()` | Shows "~XX% capture" below vaccine button |
| **Type badges** | `showCombatScreen()` | Colored type badge on enemy and ally |
| **Starter polish** | `showStarterSelect()` | Animated entry, level-1 moves shown, type badge |
| **Level-up sparkle** | `gainXP()` | CSS sparkle burst on level-up |
| **Combat keyboard shortcuts** | `keydown` handler | 1–4 for moves, V/H/F for actions |
| **Settings in-game** | `openMenu()` | Language toggle, text speed — also at launcher |
| **AZERTY/QWERTY toggle** | `openMenu()`, keydown | ZQSD or WASD depending on setting |
| **Save indicator** | top of screen | "💾 Saved" flash after auto-save |

### Art & visuals

| Area | What was done |
|---|---|
| **Player sprites** | `player_boy` / `player_girl` redrawn with shading, face detail, outfit colors |
| **Chen corpse** | `chen_corpse` sprite (lying on lab floor) |
| **Interior tiles** | `drawTileDetail()` expanded: bed (pillow+frame), shelf (stacked items), lab shelf (vials), wall (brick texture), floor (wood plank), equipment (computer), desk, chair, table, sofa, bookshelf, rug, kitchen |
| **Overworld tiles** | Mountain with snow cap; hospital with red cross; `Y` (tree) canopy; `j` (tall grass) animated sway |
| **House exterior** | `drawHouseSpecial()` — chimney + animated smoke particles + gradient roof + gold doorknob |
| **Lab exterior** | `drawLabSpecial()` — satellite dish (not cross!), blinking LED, gradient walls, window accents |
| **All 16×16 sprite rows** | Validated — every row exactly 16 chars |

### Zones & maps

| What | Notes |
|---|---|
| `bourg_palette` | Redesigned 14×12 with Y-tree borders, Chen lab at `[5,8]`, player house at `[3,6]` |
| `route_1` | 10×14 with `j` tall-grass patches, `F` forest stretches |
| All interior zones | `h_player`, `h_chen_lab`, `h_bertrand`, all city houses/labs/centers/marts, apartments — all mapped and working |
| Heal pads | `+` tiles added to `h_pcenter_ok` and `h_pcenter_dmg` zones |
| Zombie NPC spawns | All house interiors support zombie NPC spawn system |

### New tile characters (added in this session)

| Char | Name | Notes |
|---|---|---|
| `Y` | tree | Replaces `T` for trees (T was table) |
| `j` | tall_grass | Higher encounter rate (28%), animated |
| `+` | heal_pad | One-time HP restore in PokéCenters |
| `k` | kitchen | Interior decoration |
| `T` | table | Interior |
| `c` | chair | Interior |
| `B` | bookshelf | Interior |
| `D` | desk | Interior |
| `V` | sofa | Interior |
| `r` | rug | Interior |

---

## Current `G` state fields (additions from this session)

```js
G.flags.ach = {}              // achievement keys → true
G.flags.healFlash = false     // triggers heal pad animation
G.flags.lootFlash = false     // unused currently
G.flags.camLerpX = x          // camera lerp position (module-level vars too)
G.flags.camLerpY = y
G.team[i].level               // current level (1–20)
G.team[i].moves               // [{key, pp, maxPP}]
G.houses[id].zombieNpcs = []  // [{id, x, y, key, sprite, visible}]
G.inv.antibite                // count (already in struct, not yet droppable)
```

All new fields have `?? default` fallbacks in `applySaveData()`.

---

## Known working state

- Game starts in Chen's lab at `[5,8]` in `bourg_palette`
- Intro dialog triggers on first entry
- Chen boss fight works (`z_chen`, `introBoss`, `isHuman`)
- Chen corpse appears on floor after fight
- Player house is locked (`h_bourg_player` + `!G.flags.motherSaved`)
- All 16 overworld zones navigable via gates
- 30+ interior zones accessible
- Horde system active in all zones
- Bite system working (3 bites → zombification, vaccine cures)
- Combat keyboard shortcuts: 1–4 (moves), V (vaccine), H (heal), F (flee)
- AZERTY/QWERTY toggle in settings
- Auto-save after every meaningful action
- Export/import .save files working
- Vercel auto-deploy active (repo: 0xStarny/Pok-dead, branch: main)

---

## Known limitations / not yet implemented

| Feature | Status |
|---|---|
| Win condition | Infrastructure exists (`h_cinnabar_lab`, `h_cin_lab`, `G.flags.motherSaved`) but no ending trigger |
| Hospital interiors | `H` tile is walkable with loot, but no interior zones (could add `h_hospital_ok/dmg`) |
| Status effects (poison/burn/paralysis) | Not implemented yet |
| Day/night visual cycle | Not implemented |
| Infection stages (I/II/III/Rage) | Not implemented — all zombies use default stats |
| Caninos Pokémon | Not in game yet |
| Audio | No sound whatsoever |
| Human vaccine arc | `isHuman` blocks vaccine — the eventual arc of curing humans not started |
| Acts 2–6 | No story content past Act 1 (Bourg Palette) |

---

## Gotchas (things that have caused bugs before)

| Issue | Cause | Fix |
|---|---|---|
| Combat softlock | `G.combat.locked` not reset to `false` | Check every combat branch ends with `locked = false` |
| Zone renders black | Map row width ≠ `zone.width`, or unknown tile char | Validate with zone-width script |
| Tile char collision | Two tiles using same char key | `T` was both table and tree — renamed tree to `Y` |
| Lab exterior looks like a church | `drawLabSpecial` horizontal + vertical bars = cross | Changed to satellite dish design |
| Bedroom unreachable | Interior layout had wall/sofa blocking path to bed tile | Move furniture to col 1, not col 2 |
| Vercel deploy blocked | Hobby Plan blocked non-owner commits | Repo made public — fixed |
| Sprite validation | Row not exactly 16 chars | Run node sprite-check script |
| House door does nothing | `zone.houses` mapping missing or wrong coords | Check exact tile coord in map string |
| Save breaks on load | New G field without `applySaveData()` fallback | Always add `?? default` |

---

## Validation commands

```bash
# Validate JS syntax
node -e "const fs=require('fs');const html=fs.readFileSync('pokedead.html','utf8');const m=html.match(/<script[^>]*>([\s\S]*?)<\/script>/);try{new Function(m[1]);console.log('OK',m[1].length);}catch(e){console.log('ERR:',e.message);}"

# Validate zone row widths
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

## Document index

| File | Purpose |
|---|---|
| **CLAUDE.md** | Primary entry point — current state, conventions, section map, gotchas |
| **LLM.md** (this file) | Session history, what was built, current limitations |
| **docs/ARCHITECTURE.md** | Full tile table, zone list, G state shape, function reference |
| **docs/ZONES.md** | All zone IDs, connections, house mappings, entry coords, encounter pools |
| **docs/LORE.md** | Story canon — characters, acts, tone rules. Do not contradict. |
| **docs/GAMEPLAY.md** | Mechanics: type chart, loot tables, combat math, encounter scaling |
| **docs/WORLD.md** | Zone atmosphere guide for writers |
| **docs/ROADMAP.md** | Prioritized feature list for v0.4+ |
| **docs/CHANGELOG.md** | Version history |

---

## Suggested next session priorities

1. **Win condition** — implement Cinnabar lab synthesis ending (see ROADMAP Priority 1). Everything is scaffolded, just needs the trigger + cutscene + ending screen.
2. **Status effects** — poison/paralysis/burn icons + damage-over-time in `enemyTurn()`. Big combat depth gain for small code surface.
3. **Hospital interiors** — add `h_hospital_ok` / `h_hospital_dmg` interior zones. `H` tile already exists, just needs `handleHouseDoor()` mapping.
4. **Acts 2–3 story content** — Rival on Route 1, caravan NPC, Argenta lab with formula clue.
