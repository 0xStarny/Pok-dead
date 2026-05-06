# POKÉDEAD — Roadmap

## Status legend
- ✅ Done in v0.2 (working baseline)
- 🚧 In progress (v0.3 — interrupted, needs resumption)
- 📋 Planned (post-v0.3)

---

## ✅ v0.2 — Working baseline

Everything below is implemented in the current `pokedead.html` and tested.

### Engine
- ✅ Single-file HTML/CSS/JS, no dependencies
- ✅ Canvas-based 2D rendering with pixel-art sprites
- ✅ Tile system (24×24px) with `TILES` config and per-zone maps as string arrays
- ✅ Multi-zone navigation (Bourg Palette ↔ Kanto via gate tile)

### UI
- ✅ Pokémon-style launcher with title art and ASCII Pokéball
- ✅ Character creation (boy/girl + name)
- ✅ Game layout: header + stats + canvas + log + controls
- ✅ Quest panel on the right (WoW-style cards, main orange / side yellow)
- ✅ Toast notifications
- ✅ Dialog box with portrait + speaker + typewriter effect

### Story (Act 1 — partial)
- ✅ Intro dialogue: Chen explains the situation (3 days coma — needs to change to 2 weeks in v0.3)
- ✅ Player chooses one of 3 starters (Bulbizarre / Salamèche / Carapuce)
- ✅ Chen turns into a zombie → boss fight (currently Chen's Bulbasaur — needs to be Chen himself in v0.3)
- ✅ After fight, player gets quest "L'antidote humain" / "The Human Antidote"
- ✅ Bertrand side quest: 2 rations → 3 vaccines

### Combat
- ✅ Turn-based with action buttons (Attack / Vaccinate / Heal / Flee)
- ✅ Type effectiveness with multiplier
- ✅ Floating damage numbers (color-coded by effectiveness)
- ✅ Hit flash, screen shake, effectiveness banners
- ✅ HP bars with color states (green / yellow / red pulsing)
- ✅ Critical hits (8% player / 5% enemy, ×1.7 multiplier)
- ✅ Vaccine capture mechanic with HP-scaled success rate
- ✅ Cured Pokémon (★ marker) with stat buff
- ✅ Multi-Pokémon team with active switching

### Inventory & Resources
- ✅ Food / Medkit / Vaccine
- ✅ Loot tables for hospital / store / city tiles
- ✅ Hunger system (depletes with steps, kills if 0)
- ✅ Day counter (+1 per 30 steps)

### Save system
- ✅ Auto-save to localStorage after every meaningful action
- ✅ Continue button (enabled only if save exists)
- ✅ Export save as `.save` file (JSON download)
- ✅ Import save from file (drag-into picker)

### Polish
- ✅ Sprite system: 16×16 sprite strings + per-Pokémon palettes (normal + zombie variants)
- ✅ NPC quest markers animated (bouncing `!` / `?`)
- ✅ "Mom zombie" peeking from window of locked house — ambient horror
- ✅ Mountain/forest/water/road/city/hospital/lab tile visuals

---

## 🚧 v0.3 — Major refresh (INTERRUPTED — to resume)

This version was started but cut off mid-implementation. The current `pokedead.html` is **v0.2, not v0.3**. Use this list to resume:

### Order of operations (recommended — do one, test, commit, next)

#### 1. UI scaling (touches everything — do first)
- [ ] Change `TILE_SIZE` from 24 to 32
- [ ] Update all `drawTileDetail` coordinates (×4/3 for new size)
- [ ] Bump font sizes throughout: body 11px → 13px, headers ×1.3
- [ ] Increase canvas width: max 480px → 640px
- [ ] Increase quest panel width: 280 → 320px
- [ ] Increase combat sprite scale: ×4 → ×5 (96×96 in stage)
- [ ] Increase combat stage height: 180px → 240px
- [ ] Larger dialog box and portrait (64 → 80px)
- [ ] D-pad buttons 40 → 48px
- [ ] Test layout still works on mobile (480px breakpoint)

#### 2. EN/FR translation system
- [ ] Add `T_EN` and `T_FR` dictionary objects with all UI strings
- [ ] Add `LANG` global, default 'en', read from localStorage on boot
- [ ] Add `t(key, params)` helper function
- [ ] Add language toggle on launcher (top-right, EN/FR buttons)
- [ ] Add language toggle in in-game Menu
- [ ] Replace ALL inline strings with `t()` calls — categories:
  - Launcher / char creation labels
  - Header subtitle, stat labels, button labels
  - Tile names (in tile-info bar)
  - All dialog texts (use templates with `%name%` interpolation)
  - All log messages
  - All quest titles, descs, objectives
  - Combat messages, modal titles
  - Game over / victory text
- [ ] Persist language choice to localStorage as `pokedead_lang`
- [ ] Make `setLanguage()` re-render all visible UI

#### 3. Combat layout flip (small, low-risk)
- [ ] In CSS, swap `.cs-enemy` and `.cs-ally` positions:
  - Enemy: top-RIGHT (was top-LEFT) + add `transform: scaleX(-1)`
  - Ally: bottom-LEFT (was bottom-RIGHT), no flip
- [ ] Swap info card positions: `.ic-enemy` to top-LEFT, `.ic-ally` to bottom-RIGHT
- [ ] Update hit-flash keyframes to match new flip directions
- [ ] Test combat visually

#### 4. Lore corrections in dialog
- [ ] Change "trois jours dans le coma" → "deux semaines dans le coma" (and EN equivalent)
- [ ] Add line: "Pokémon zombies are EVERYWHERE now, %name%. In the grass, the forests, the cities."
- [ ] Add line about humans being weakly susceptible: "Some humans too — but the virus seems weaker on us."

#### 5. Bite mechanic
- [ ] Add `bites: 0` and `zombified: false` to every team Pokémon spawn
- [ ] After every enemy hit on ally: `ally.bites++`
- [ ] Display bite count in combat info card and team panel ("Bites: 1/3")
- [ ] At `bites >= 3`: trigger zombification — log message, set `ally.zombified = true`, force switch to next ally
- [ ] If no living non-zombified Pokémon left: combat end / game over conditions
- [ ] Vaccine on zombified team Pokémon: heals back to normal
- [ ] Anti-Bite item (rare loot): uses `G.inv.antibite`, removes 1 bite mark
- [ ] On day change: each rested (not active in last fight) Pokémon -1 bite

#### 6. Combat layout: Chen the human is the intro boss
- [ ] Change `startChenFight()` to spawn an enemy with sprite `chen_zombie`, type `human`, ~40 HP, 11 ATK, named "Prof. Chen (turning)"
- [ ] Block vaccine option for human enemies (`enemy.human` flag check)
- [ ] Update post-fight dialogue: Chen thanks the player, dies in peace, references the lab and Mom

#### 7. Hordes
- [ ] Add `G.hordes` zone-keyed object (initialized with placed hordes — 2-3 in Kanto)
- [ ] Each horde: `{id, x, y, enraged, defeated}`. Set 1/3 to `enraged: true`.
- [ ] In `drawMap`: render hordes as cluster of 5 zombie sprites + tinted overlay; pulsing red border if enraged
- [ ] On player move, run `updateHordes()`:
  - If `G.hordeChase` active: move horde toward player (1 tile, 2 if enraged), check if caught
  - If not chasing: check Manhattan ≤ 5 to player, if any horde sees → start chase
- [ ] On chase start: show `#horde-alert` div, log "A horde spotted you!"
- [ ] On chase end (caught / hidden / horde defeated): hide alert
- [ ] If caught: trigger `triggerHordeCombat()` — sequence of 3 zombies, no flee

#### 8. Hide tiles
- [ ] Add tile char `b` (hideout bush) to `TILES` with `pass:1, hide:1`
- [ ] Place several `b` tiles in Kanto zone map
- [ ] Render bushes visually (green dense bush sprite over grass)
- [ ] When player presses A on a `b` tile during chase: enter hide state for 3 turns
- [ ] After 3 turns: log "You lost them.", clear `G.hordeChase`

#### 9. House interiors
- [ ] Add tile chars: `d` (door), `E` (exit), `_` (floor), `#` (wall), `B` (bed)
- [ ] Add zone definitions for `house_kanto_1` (and any others) with `isInterior: true`
- [ ] Create `G.houses[zoneId] = {clean, hasZombie, hasBody, lootTaken}` state
- [ ] On entering door: save `G.player.prevZone/prevX/prevY`, switch to house zone
- [ ] If house has zombie: trigger combat on entry
- [ ] If house has body: dispatch loot + log "A body lies in the corner..."
- [ ] On house cleared (no zombie + loot taken): set `clean=true`, log "Bed is now safe to use"
- [ ] Animated "ZzZ" sprite above bed when usable
- [ ] Walking onto bed = sleep (only if `clean`)

#### 10. Sleep mechanic update
- [ ] Remove the always-available Rest button from `renderControls()`
- [ ] Sleep only triggered by walking onto a `B` (bed) tile in a clean interior
- [ ] Sleep effect: HP +40%, bites -1, +1 day, -20 hunger
- [ ] If hunger < 20: "Too hungry to sleep"
- [ ] 35% chance of night encounter still applies

#### 11. Human zombies as encounter type
- [ ] Add `zombie_man` and `zombie_woman` to `HUMANS` sprite dictionary
- [ ] Add to `ZOMBIES` data with `human: 1` flag, low stats (15-25 HP, 5-7 ATK)
- [ ] Include in encounter pool for cities
- [ ] Block vaccine option in combat (`if enemy.human: disable vaccinate`)
- [ ] On house entry, 60% chance of zombie encounter, 40% chance of dead body event

#### 12. Increased early-game zombie density
- [ ] Bump `enc` rates: forest 32% → 40%, city 40% → 42%, hospital 20% → 25%
- [ ] Add atmospheric note in Chen's intro: zombies are EVERYWHERE
- [ ] Ensure first 30 minutes of play feels appropriately dangerous

---

## 📋 v0.4+ — Post-v0.3 priorities

Once v0.3 is shipped and stable:

### Story expansion (per LORE.md Acts 2-6)
- [ ] Act 2 zone: Route 1 + Viridian Forest with caravan / Pidgey boy / guard post side quests
- [ ] Act 3 zone: Viridian city with main lab interior, formula reveal, 3 ingredient hunt teaser
- [ ] Acts 4a/4b/4c: Mt. Moon (snow), Lavender Town (ghost), Cinnabar Island (volcano)
- [ ] Act 5: Team Vector HQ + Mortimer confrontation
- [ ] Act 6: Necronos boss fight + Mom rescue ending

### Mechanics
- [ ] **Infection stages** — different stat profiles for I/II/III/Rage zombies, varied sprites
- [ ] **Bite-derived attacks** — burning bite, poisonous bite, spectral bite, etc., per type
- [ ] **Necronos** — fully unique sprite + fight + Vector Bite attack (2 marks + drain)
- [ ] **Caninos** as a vaccinable Pokémon (origin reveal in Act 4c)

### Polish
- [ ] **Day/night cycle** with visual difference (dim overlay at night, more zombies)
- [ ] **Audio** — minimal SFX (combat hits, vaccine, footstep), maybe ambient track
- [ ] **More NPCs in cities** — survivors, traders, story flavor
- [ ] **The Rival** (ex-Vector grunt) appearing in Acts 2/4/5
- [ ] **Pokémon League radio transmissions** from Act 3 onward
- [ ] **Pokédex entry per zombie** — short scientific lore note per species

### Quality of life
- [ ] **Settings menu** with brightness, text speed, language toggle
- [ ] **Mini-map** for large zones
- [ ] **Inventory expansion** — keep multiple of each item type
- [ ] **Pokémon storage box** beyond the team of 4

---

## How to use this roadmap with Claude Code

When asking Claude Code to work on this project:

1. Have it **read CLAUDE.md and ROADMAP.md first** (it does this automatically with CLAUDE.md, but mention ROADMAP explicitly).
2. Ask it to **work on one v0.3 item at a time**, in the order listed.
3. After each item: **test the syntax**, **play-test the game**, then commit.
4. Don't let Claude Code rewrite the whole file. Surgical edits via `str_replace` are safer.
5. The owner prefers **incremental progress over big rewrites**. Lesson learned from v0.3 attempt: a 100KB rewrite in one shot is too risky.

When in doubt about lore or design intent, **always check `docs/LORE.md` first**, then ask the owner.
