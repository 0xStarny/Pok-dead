# Changelog

## v0.3 — 🚧 In progress (interrupted)

Started a major refresh, ran out of session time before finishing. See [ROADMAP.md](ROADMAP.md) for the full feature list and resumption plan.

**Was being added:**
- Bigger UI (32px tiles, larger fonts, wider layout)
- EN/FR translation system with language toggle
- Combat layout flip (ally on left, enemy on right — classic Pokémon layout)
- Bite mark system (3 bites = zombification)
- Hordes with detection / chase / hide mechanic
- House interiors with beds for sleeping
- Human zombies (low-stat encounter type)
- Chen-as-human boss fight (replacing the Bulbasaur intro)
- Lore corrections (2 weeks coma, zombies everywhere)

**Status:** the last edit was interrupted mid-`enemyTurn` function in the bite/zombification block. **The current `pokedead.html` is v0.2, not v0.3.** The v0.3 features must be re-implemented.

---

## v0.2 — ✅ Released (working baseline)

Full rebuild of the prototype with proper structure.

**Added:**
- Pokémon-style launcher with title art, ASCII Pokéball, lore intro
- Character creation (boy/girl + custom name)
- Intro narrative cutscene with Prof. Chen
  - Explains 2-week coma (was 3 days, will fix in v0.3)
  - Tells about Mom infected next door
  - Hands you a starter
  - Transforms and asks you to put him down
- Quest panel on the right (WoW-style)
  - Main quests (orange `!`)
  - Side quests (yellow `?`)
  - Animated NPC quest markers
- Bertrand side quest (2 rations → 3 vaccines)
- Combat improvements:
  - Floating damage numbers (color-coded)
  - Hit flash, screen shake
  - Type effectiveness banners
  - Critical hits (8% / 5%)
- Save/load:
  - Auto-save to localStorage
  - Continue button (when save exists)
  - Manual export to `.save` file
  - Manual import from `.save` file
- "Mom zombie" peeking from the locked house — early-game horror touch

**Technical:**
- Single-file HTML, ~80 KB
- No dependencies, no build step
- Pixel sprites for all characters and Pokémon
- 24×24 tile system

---

## v0.1 — Archived (first prototype)

Initial proof of concept.

**Had:**
- 16×12 single-zone map (Kanto)
- 3 starters, 7 zombie species + 1 boss
- Basic turn-based combat
- Vaccine capture mechanic
- Hunger system
- Rest button (always available — removed in v0.3)
- Random loot in city / hospital / store tiles
- Game over on HP=0 or starvation

**Limitations:**
- No story / no characters / no dialogue
- No quest system
- No save/load
- No launcher / character creation
