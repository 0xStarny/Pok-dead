# POKÉDEAD — Changelog

---

## v0.3 — ✅ Complete

Full refresh of v0.2 baseline. All planned features implemented.

### New content
- **Full Kanto corridor** — 16 overworld zones: Bourg Palette → Route 1 → Jadielle → Route 2 → Argenta → Mt. Moon → Route 3 → Azuria → Route 4 → Lavandia → Route 8 → Céladopole → Route 7 → Safrania → Route 21 → Cinnabar
- **30+ interior zones** across all cities: houses (5 variants), labs, PokéCenters (3 variants), PokéMarts (3 variants), apartment buildings (multi-floor)
- **Marc NPC** in Lavandia + `q_mortimer` side quest
- **Multi-floor buildings** — `h_immeuble_g` (ground) + `h_immeuble_1` (floor 1), stairs navigation with parent-chain exit

### Engine
- **UI scaling** — `TILE_SIZE` 24 → 32px, fonts ×1.3, layout max 1080px, combat sprites 96×96
- **EN/FR translation system** — `T_EN`, `T_FR`, `t(key, params)`, language toggle (launcher + menu), persisted to localStorage
- **Combat layout flip** — enemy top-right, ally bottom-left (classic Pokémon)
- **Bite mark system** — 3 bites = zombification, vaccine cures ally; displayed in combat card
- **Horde system** — hordes placed in each zone, detect player at distance 5, chase 1 tile/turn (2 if enraged), forest hide for 3 turns
- **House interiors** — `handleHouseDoor()`, `handleHouseExit()`, `handleBed()`, `handleStairs()`
- **Bed-only sleep** — free Rest button removed; sleep only on `b` tiles in houses
- **Human zombies** — `z_human` (random encounter, 20 HP / 6 ATK), vaccine blocked on all `isHuman` enemies
- **Chen-as-human-boss** — `z_chen` replaces the Bulbasaur intro fight; uses `chen_zombie` sprite
- **Player house locked** until `motherSaved` flag — barricade message shown
- **Player starts at Chen's lab** (not center of village)

### Lore fixes
- Coma duration: 3 days → **2 weeks**
- Quest objective: "Put Chen's Pokémon to rest" → **"Put Chen to rest"** / "Achever Chen"

---

## v0.2 — ✅ Released (working baseline)

Full rebuild of the prototype with proper structure.

- Pokémon-style launcher, character creation, lore intro
- Intro dialogue with Prof. Chen (starter choice, transformation)
- Quest panel (WoW-style), Bertrand side quest
- Turn-based combat: floating damage numbers, hit flash, screen shake, type banners, crits
- Vaccine capture mechanic
- Save/load: auto-save localStorage + export/import `.save` files
- Two zones: Bourg Palette + Kanto placeholder

---

## v0.1 — Archived (first prototype)

Initial proof of concept.

- Single-zone 16×12 map
- 3 starters, 7 zombie species + 1 boss
- Basic combat, vaccine capture, hunger system
- No story, no characters, no save
