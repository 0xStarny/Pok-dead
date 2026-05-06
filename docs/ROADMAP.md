# POKÉDEAD — Roadmap

---

## v0.3 — ✅ Complete

All features shipped. See [CHANGELOG.md](CHANGELOG.md).

---

## v0.4 — 🔜 Next milestone

### Story — Acts 2–6 (per LORE.md)

| Zone | Act | Status | What's needed |
|---|---|---|---|
| Route 1 / Jadielle | 2 | ⬜ | Caravan NPC, Pidgey boy, guard post side quests |
| Route 2 / Argenta | 2 | ⬜ | Lab interior, formula reveal, 3-ingredient hunt teaser |
| Mt. Moon | 3 | ⬜ | Snow sanctuary feel, unique cold-zone encounter design |
| Lavandia | 4a | ⬜ | Tower / Ghost zone, epidemic epicenter atmosphere |
| Cinnabar Island | 4c | ⬜ | **Win condition**: Cinnabar lab synthesis → save Mom ending |
| Team Vector HQ | 5 | ⬜ | Mortimer confrontation |
| Necronos boss | 6 | ⬜ | Unique boss fight, Mom rescue |

### Win condition (priority — needed to complete the loop)

- [ ] Cinnabar lab (`h_cinnabar_lab`) already exists and has the `h_cin_lab` house mapping
- [ ] `G.flags.motherSaved` already exists in state
- [ ] On entering Cinnabar lab: trigger synthesis cutscene (5 vaccines spent)
- [ ] Set `G.flags.motherSaved = true`
- [ ] Unlock Mom's house (player's house no longer barricaded)
- [ ] Final dialogue + victory screen

### Mechanics

- [ ] **Infection stages** — Stage I / II / III / Rage variants with different stats and sprites
- [ ] **Anti-Bite item** — `G.inv.antibite`, removes 1 bite mark (rare loot drop)
- [ ] **Bite regen on rest** — each non-active Pokémon loses 1 bite per rest
- [ ] **Caninos** as vaccinable Pokémon (introduced in Act 4c)
- [ ] **Necronos** unique boss with Vector Bite (2 marks + drain)
- [ ] **Night encounters** — 35% encounter on sleep, harder zombies at night

### NPCs and quests

- [ ] Story characters for each city (traders, survivors, flavor)
- [ ] The Rival (ex-Vector grunt, appears Acts 2 / 4 / 5)
- [ ] Pokémon League radio transmissions from Act 3 onward
- [ ] Mom's Notebook side quest (found near locked house)

### Polish

- [ ] Day/night visual cycle (dim overlay at night)
- [ ] Settings menu (brightness, text speed, language toggle)
- [ ] Mini-map for large zones
- [ ] Pokédex entry per zombie (short scientific lore note)
- [ ] Audio: minimal SFX (combat hits, vaccine, footstep) + optional ambient

---

## How to use this with Claude Code

1. Read **CLAUDE.md first** (always — it has current state and conventions)
2. Read **ARCHITECTURE.md** for the code map and tile/zone reference
3. Check **LORE.md** before writing any story content
4. Work **one feature at a time** — validate syntax + zone widths + play-test after each
5. Commit after each working feature
6. Never rewrite the whole file — surgical edits only
