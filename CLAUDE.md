# POKÉDEAD — Claude Context

> **Primary context file. Read this first before any work on the project.**

## What this is

POKÉDEAD is a single-file HTML/JS browser game — a Pokémon-style RPG set in a zombie apocalypse. Walking Dead × Pokémon, but specifically: an emotional drama where the player must reach a distant lab to synthesize a human vaccine to save their zombified mother.

**Tech stack:** plain HTML + CSS + vanilla JS, no build step, no dependencies. The entire game lives in `pokedead.html` and runs by double-clicking the file.

**Why single-file:** the project owner wants something portable, modifiable in one place, and shareable as a single download.

## Current state

| Version | Status | What it has |
|---|---|---|
| v0.1 | Archived | First playable prototype, simple combat |
| **v0.2** | **✅ Working baseline** | Launcher, char creation, intro dialogue with Chen, quest panel, save/load, basic combat with floating damage numbers |
| v0.3 | 🚧 **Incomplete — DO NOT use as-is** | Started: bigger UI, EN/FR translation, hordes, house interiors, bite system, combat layout flip, Chen-as-human boss. Got cut off mid-`enemyTurn` function. |

**The current `pokedead.html` is v0.2 (working).** v0.3 was attempted but interrupted; its design is documented in `docs/ROADMAP.md` for resumption.

## What's done in v0.2 (don't break these)

- 🎮 **Launcher screen** with title art, New / Continue / Import Save buttons
- 👤 **Character creation** (boy/girl + name)
- 📖 **Intro dialogue** with Prof. Chen explaining the situation, his transformation, choosing a starter
- ⚔️ **Boss fight intro** (currently against Chen's Bulbasaur — needs change to Chen himself in v0.3)
- 🗺️ **Two zones:** Bourg Palette (tutorial) and Kanto (main map)
- 👥 **NPCs with quest markers** (orange `!` main, yellow `?` side)
- 📜 **Quest panel** on the right (WoW-style) with main + side quests tracked
- 💉 **Combat:** turn-based, type effectiveness, vaccine capture mechanic, floating damage numbers, hit flash, screen shake, type effectiveness banners
- 🎒 **Inventory:** food / medkit / vaccine
- 💾 **Save system:** auto-save to localStorage + manual export/import as `.save` files
- 🏚 **Looting** in cities, hospitals, stores
- 💀 **Game over** if HP hits 0 or starvation

## What v0.3 was adding (resumption guide in ROADMAP.md)

1. **UI scaling** — tile size 24→32px, fonts ×1.3, wider layout (max 1080px)
2. **EN/FR translation system** with `t()` helper and full string tables
3. **Combat layout flip** — ally on left/bottom, enemy on right/top (classic Pokémon layout)
4. **Bite mark system** — Pokémon takes a bite per enemy hit, 3 bites = zombification, vaccine restores
5. **Hordes** — clusters of zombies on the map, detect at 5 tiles, chase 1/turn (or 2 if enraged, 1/3 chance)
6. **Hide tiles** — bushes that let you escape a horde for 3 turns
7. **House interiors** — small instanced maps you enter through doors, with loot, optional zombies, beds
8. **Bed-only sleep** — rest is no longer a free button; only at beds in cleared houses, with animated "ZzZ" overlay
9. **Human zombies** — weak (15-25 HP, 5-7 ATK), only basic Bite, never enraged. Chen becomes one.
10. **Chen-as-human-boss** — replace the Bulbasaur intro fight with Chen himself (you achieve him to keep your promise)
11. **Coma duration** — change "3 days" to "2 weeks" in dialogue (matches lore)
12. **Outbreak everywhere** — Chen's intro must say "Pokémon zombies are EVERYWHERE now" + boost early-game encounter rates

## Project structure

```
pokedead/
├── CLAUDE.md           ← you are here
├── README.md           ← Github landing page
├── pokedead.html       ← the game (single file, ~80 KB)
├── .gitignore
└── docs/
    ├── LORE.md         ← Full validated story bible (canon — do not contradict)
    ├── GAMEPLAY.md     ← All mechanics, tuning, type chart
    ├── ROADMAP.md      ← What's done / in progress / planned + v0.3 resumption guide
    ├── CHANGELOG.md    ← Version history
    └── ARCHITECTURE.md ← Code structure inside pokedead.html
```

## How to work on this project

### Running the game
Double-click `pokedead.html`. That's it. No npm, no server.

### Testing changes
After any JS change, validate syntax:
```bash
node -e "const fs=require('fs');const html=fs.readFileSync('pokedead.html','utf8');const m=html.match(/<script[^>]*>([\s\S]*?)<\/script>/);try{new Function(m[1]);console.log('OK',m[1].length);}catch(e){console.log('ERR:',e.message);}"
```

### Code conventions
- **Single-file rule:** everything stays in `pokedead.html`. No splits.
- **No dependencies, no build step.** Pure browser code.
- **Prefer surgical edits.** When adding a feature, find the right section (HTML / CSS / data / state / functions / boot) and add there. Don't rewrite the whole file.
- **Test syntax after every batch.** Easy to break a 78 KB file with one missing brace.
- **Keep sprite rows exactly 16 chars wide.** Sprites use a per-Pokémon palette; rows are indexed by character. Use `.` for transparent pixels.
- **All user-visible strings should go through `t()`** in v0.3. Right now (v0.2) most are inline French — that's the migration to do.

### Working method (recommended)
The owner explicitly preferred **incremental changes from a working baseline** over big rewrites. Lessons from v0.3:
- Rewriting everything at once → 100 KB+ JS in one shot → hard to debug, high risk of cascading errors.
- Better: take v0.2 (works), add ONE feature at a time, test, commit, next feature.

So: when implementing the v0.3 features list above, do them **one by one**, in the order listed, testing the game between each. Order matters — UI scaling first because it changes coordinate math everywhere; translation second because it touches all strings; combat layout flip third (low-risk visual change); then mechanics one at a time.

### Lore is canon
The `docs/LORE.md` file contains every validated narrative decision. **Do not invent contradicting lore.** If a question isn't covered there, ask the owner. The owner has been very specific about story tone (purely scientific, no biblical/mystical, emotionally grounded).

### Communication style
The owner speaks French primarily. Replies in French are welcome but English is fine too. Be honest about what's done vs not done. When you finish a feature, say what tests you ran. Don't claim something works without testing it.

## Quick links

- 📖 **Story:** [docs/LORE.md](docs/LORE.md)
- 🎮 **Mechanics:** [docs/GAMEPLAY.md](docs/GAMEPLAY.md)
- 🛣 **Next steps:** [docs/ROADMAP.md](docs/ROADMAP.md)
- 📐 **Code map:** [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- 📝 **History:** [docs/CHANGELOG.md](docs/CHANGELOG.md)
