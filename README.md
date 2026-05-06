# 🧟 POKÉDEAD — Apocalypse Edition

> A single-file Pokémon-style RPG set in a zombie apocalypse.
> Walking Dead × Pokémon Gen I.

```
     ▄████████▄
   ▄██▀░░░░░░▀██▄
  ██▀░░░░▄▄░░░░▀██
 ██░░░▄██████▄░░░██
 ██▄▄██████████▄▄██
 ██░░░▀██████▀░░░██
  ██▄░░░░▀▀░░░░▄██
   ▀██▄░░░░░░▄██▀
     ▀████████▀
```

## What it is

POKÉDEAD is a browser-based Pokémon-style RPG with a dark twist: an outbreak has turned most Pokémon (and some humans) into zombies. The player wakes from a 2-week coma to discover their world destroyed, their mother infected and locked in their house, and the only ally — Prof. Chen — turning into a zombie in front of them.

The goal: travel across a devastated Kanto, find a research lab with the formula for a human vaccine, and come back to save Mom.

## Play it

**No install. No build. Just open the file.**

1. Download `pokedead.html`
2. Double-click it
3. Play in your browser

Works on desktop and mobile. Saves automatically. Export/import save files anytime.

## Features

- 🎮 **Pokémon-style turn-based combat** with type effectiveness
- 💉 **Vaccine capture mechanic** — convert wild zombies back into your team
- 🗺️ **Multi-zone exploration** — Pallet Town tutorial → Kanto wilderness
- 📜 **Quest system** — main + side quests with NPC markers
- 💀 **Survival systems** — hunger, infection, save scumming-resistant deaths
- 🎨 **Pixel-art sprites** for every Pokémon and human character
- 💾 **Save/load** to file or browser localStorage

## Project status

Current playable version: **v0.2** (working baseline).

A major refresh is planned — see [docs/ROADMAP.md](docs/ROADMAP.md):
- Bigger UI
- EN/FR language toggle
- Bite mark system (3 bites = zombification)
- Hordes that chase you
- House interiors with beds for sleeping
- Human zombies
- And more

## Development

This is a single-file project. Everything lives in `pokedead.html`. To modify:

1. Edit `pokedead.html` in any text editor
2. Refresh the browser
3. That's it

For LLM-assisted development (Claude Code or similar), start by reading [`CLAUDE.md`](CLAUDE.md) — it contains the full project context, lore, and development guidelines.

### Documentation

- 📖 [`docs/LORE.md`](docs/LORE.md) — Story bible (canon, do not contradict)
- 🎮 [`docs/GAMEPLAY.md`](docs/GAMEPLAY.md) — Mechanics reference
- 🛣 [`docs/ROADMAP.md`](docs/ROADMAP.md) — What's done, in progress, planned
- 📐 [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — Code map
- 📝 [`docs/CHANGELOG.md`](docs/CHANGELOG.md) — Version history

## Tech

- Plain HTML5 + CSS3 + ES6+ JavaScript
- Canvas 2D for world rendering
- No frameworks, no dependencies, no build step
- Single-file delivery (~80 KB)

## Inspirations

- Pokémon Red/Blue (Game Boy, 1996)
- The Walking Dead
- Project Zomboid
- Last of Us

## License

This is a hobby project — credit appreciated, no specific license yet.
