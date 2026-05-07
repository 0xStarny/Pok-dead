# POKÉDEAD — Roadmap

---

## v0.3 — ✅ Complete

All features shipped. See [CHANGELOG.md](CHANGELOG.md).

---

## v0.4 — 🔜 Next milestone

### 🏆 Priority 1 — Win condition (close the gameplay loop)

| Task | Status | Notes |
|---|---|---|
| Cinnabar lab entry trigger | ⬜ | `h_cin_lab` house already mapped, zone `h_cinnabar_lab` exists |
| Synthesis cutscene (5 vaccines spent) | ⬜ | Dialog sequence + cost check |
| `G.flags.motherSaved = true` | ⬜ | Unlocks player house, enables ending |
| Mom encounter at player house | ⬜ | Survive her approach to inject — tense, emotional, non-lethal |
| Victory screen | ⬜ | Epilogue text + stat summary + credits |

---

### 🎮 Priority 2 — Gameplay depth

#### Combat

| Task | Notes |
|---|---|
| **Status effects** | Poison (2 dmg/turn), Paralysis (50% skip turn), Burn (1 dmg/turn + -atk) — show icon in combat card |
| **Type-specific bite attacks** | Each zombie type has its own bite variant (see LORE.md §5) — e.g. ghost = Spectral Bite, poison = Poisonous Bite |
| **Infection stages** | Stage I (fresh, +ATK), Stage II (default), Stage III (old, -ATK), Rage (red eyes, +50% ATK) — different sprite tints + stat modifiers |
| **Anti-Bite item** | `G.inv.antibite`, removes 1 bite mark instantly. Rare loot drop. Already in `G.inv` struct |
| **Bite regen on rest** | Each non-active Pokémon loses 1 bite per sleep. Already documented in GAMEPLAY.md |
| **PP system refinement** | Currently implemented, needs balancing: PP per move, rest restores all PP |
| **Move variety** | Add 2–3 moves per species; give enemy encounters move variety based on tier |
| **Switch mid-combat** | Allow switching active Pokémon without wasting the turn (costs action, enemy attacks first) |

#### Exploration

| Task | Notes |
|---|---|
| **Hospital interior zones** | `H` tile currently walkable + loot, but no interior. Add `h_hospital_ok` / `h_hospital_dmg` interior zones for richer loot + atmosphere |
| **Night encounters** | 35% encounter chance on sleep; harder zombie tier at night |
| **Day/night visual cycle** | Dim canvas overlay when `G.day % 2 === 1` (nights), stars in sky on exterior map |
| **Loot body NPCs** | Static dead-body NPCs in houses — bump to loot (medkit/vaccine, one-time) |
| **Forest trader cabin** | Small hut in forest zones — NPC that trades medkits ↔ vaccines |
| **Scarcity curve** | Make food rarer in late zones (Celadon, Saffron); more vaccines but fewer medkits |

#### Pokémon & team

| Task | Notes |
|---|---|
| **Caninos** | New zombie Pokémon (Fire type). Introduced in Act 4c (Cinnabar zone). Ties to lore (Patient Zero's species) |
| **Necronos boss** | Unique boss — Fire/Ghost, Vector Bite (2 marks + drain), immune to normal, cannot flee. Must vaccinate, not kill |
| **Pokédex** | Short per-zombie lore note (1 line), readable from team panel. Flavor only |
| **Cured Pokémon names** | Give cured Pokémon their "healthy" name back (e.g. Z-Rattata → Rattata★) with different color |
| **Level display** | Show current level on team cards and in combat |

---

### 🗺 Priority 3 — World & story content (Act 2–6)

| Zone | Act | Key tasks |
|---|---|---|
| Route 1 / Jadielle | 2 | Caravan NPC, Rival (first contact, flees), guard post audio log |
| Route 2 / Argenta | 2 | Lab interior, formula fragment clue |
| Mt. Moon | 3 | Snow sanctuary feel, first uninfected Pokémon, Old Skier NPC |
| Lavandia | 4a | Pokémon Tower, Mortimer's hidden underground lab, Rival (2nd contact) |
| Cinnabar | 4c | Win condition (see Priority 1), Mortimer's early hospital notes, Caninos breeder NPC |
| Safrania | 5 | Team Vector HQ (Silph Co.), Mortimer confrontation, Rival (3rd contact) |
| Finale | 6 | Necronos boss fight at Indigo Plateau ruins → vaccinate → Caninos dies peacefully |

---

### ✨ Priority 4 — Polish & UX

| Task | Notes |
|---|---|
| **Settings menu** | Brightness, text speed, language toggle (available in-game, not just launcher) |
| **Mini-map** | Small overlay mini-map for current overworld zone (toggle with M key) |
| **Audio** | Minimal SFX: combat hit, vaccine use, footstep, bed sleep, item pickup |
| **Ambient sound toggle** | Optional ambient: forest wind, city silence, distant zombie groans |
| **Camera polish** | Already smooth lerp; could add screen-edge darkening (vignette follows camera) |
| **Save file name** | Include zone name in save filename: `pokedead_<name>_<zone>_jour<N>.save` |
| **Load screen** | Show last-played zone art in launcher Continue button tooltip |
| **Mobile touch** | D-pad already exists; confirm all overlays touch-compatible |

---

## Ideas backlog (no milestone yet)

These are solid improvements to do whenever bandwidth allows:

- **Horde mega-blocking** (Cerulean mega-horde dispersed by fuel explosion) — adds puzzle layer to navigation
- **Rival as NPC ally** during Saffron infiltration — appears on map for cutscene, not on team
- **Pokémon League radio transmissions** — receivable from Lavandia antenna, Act 3+
- **Mom's Notebook** — documents the 2 weeks before coma, found near locked house as side quest
- **The Pidgey Boy** — kid living in a tree on Route 2, moral choice quest
- **Dead Radios** — repair antenna in Lavandia, hear other survivors for the first time
- **Corrupt Soldier NPC** at Vermilion — selling vaccines black market (denounce or exploit?)
- **SS Anne** — hospital ship with survivor community, multiple quests
- **True ending variation** — Rival's sister found alive / fate unknown based on player choices

---

## How to work on this (LLM instructions)

1. **Read CLAUDE.md first** — always. It has current state, conventions, and gotchas.
2. **Read ARCHITECTURE.md** for the full code map, tile table, and function index.
3. **Read LORE.md** before writing any story content — canon is strict.
4. **One feature at a time** — validate syntax + zone widths + play-test after each.
5. **Never rewrite the whole file** — surgical edits only inside the correct JS section.
6. **Commit after each working feature** with a clear message.
7. **Validate after every JS change:**
   ```
   node -e "const fs=require('fs');const html=fs.readFileSync('pokedead.html','utf8');const m=html.match(/<script[^>]*>([\s\S]*?)<\/script>/);try{new Function(m[1]);console.log('OK',m[1].length);}catch(e){console.log('ERR:',e.message);}"
   ```
