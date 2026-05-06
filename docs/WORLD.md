# POKÉDEAD — World Design: Kanto Zone by Zone

> This document covers every zone in Kanto: its state during the outbreak, what happens there, quests, NPCs, and gameplay mechanics. Companion to LORE.md.

---

## Epidemic Geography — Core Rules

- **Epicenter: Lavender Town** — where Necronos was born. Ground zero. Oldest and worst zombies (Stage III + Rage).
- **The further from Lavender, the more survivable** — generally. But large cities fall hard regardless of distance (population density = rapid spread).
- **Extreme zones are sanctuaries** — Mt. Moon (cold) and Cinnabar Island (volcanic heat) are virus-free. Normal Pokémon, calm atmosphere, dramatic contrast with the rest of the world.
- **Mega-hordes block certain zones** until specific events disperse them.

---

## Zone Reference

---

### 🏠 PALLET TOWN
**Act:** 1
**State:** Small village, partially standing. A few barricaded survivors.
**Epidemic status:** Late to fall. Remote, low population. Still dangerous — isolated zombies, no mega-horde.

**Story beats:**
- Player wakes from 2-week coma. Chen is here — not for long.
- Mom's house is locked. She's at the window.
- Chen transforms → player fights him with starter → quest begins.

**NPCs:**
- `chen` — intro, transformation, boss fight
- `bertrand` — side quest: 2 rations → 3 vaccines + post-quest type tips
- `mother_zombie` — visible at window, locked house

**Planned quests:**
- 🟧 The Awakening (main)
- 🟨 Bertrand's Hunger
- 🟨 Mom's Notebook — found near locked house, documents days before coma
- 🟨 The Aide's Pokémon — neighbor's vaccinated Pidgey, find what's left

**Tile notes:**
- Mom's house: locked (`h` tile), door inaccessible until Act 6
- Chen's lab: accessible from start
- Gate north: opens after Chen fight (leads to Route 1)

**Ambiance:** Home, but wrong. Quiet horror. Personal, not epic.

---

### 🛣️ ROUTE 1
**Act:** 2
**State:** Open road, scattered zombies. No mega-horde. First real danger after Pallet.
**Epidemic status:** Moderate. Zombies drifted down from Viridian. Stage II mostly.

**Story beats:**
- **First contact with the Rival** — he flees on sight, mistakes player for a Vector grunt (player carries Chen's vaccines)
- Overturned supply caravan — side quest

**NPCs:**
- `rival_1` — brief encounter, scripted flee, no fight
- `caravan_survivor` — optional, gives info on Viridian

**Houses on route:**
- House 1: clean, one survivor inside. Gives info, trades food.
- House 2: infested, one dead body (loot), one zombie inside.

**Encounters:** Z-Rattata, Z-Pidgey (tutorial tier, weak)

**Quests:**
- 🟨 The Lost Caravan — scattered supply crates, 2-3 zombies still around
- 🟨 First Rival encounter (no quest, scripted event)

**Ambiance:** Wide open, false sense of freedom. Tall grass moves. You hear them before you see them.

---

### 🌲 VIRIDIAN FOREST + ROUTE 2
**Act:** 2
**State:** Dense forest, Stage III zombies (slow, old, many). A patrolling horde on a fixed loop.
**Epidemic status:** Old infection. Zombies decomposed, slower but numerous.

**Story beats:**
- First horde with a **fixed patrol pattern** — memorizable, avoidable
- The Pidgey Boy hidden in a tree
- Abandoned guard post with audio journal

**NPCs:**
- `pidgey_boy` — 8-year-old alone in a tree, 3 vaccinated Pidgeys. Refuses to come down. Moral choice: force him or leave him.
- (no NPC at guard post — audio only)

**Quests:**
- 🟨 The Pidgey Boy — moral choice, affects epilogue
- 🟨 The Guard Post — reconstruct the 14-day timeline from audio fragments

**Horde:** 1 patrolling horde, loop pattern, enraged variant (1/3 chance). First `b` hide tiles available.

**Encounters:** Z-Caterpie, Z-Weedle, Z-Pidgey — Stage III variants (weaker stats)

**Ambiance:** Claustrophobic, green-dark. Forest muffles sound. Zombies heard before seen.

---

### 🏙️ VIRIDIAN CITY
**Act:** 3
**State:** Partially standing. Military checkpoint holds the center. Periphery fallen.
**Epidemic status:** Moderate-heavy. First major city. Army present but shrinking.

**Story beats:**
- Chen's main lab is here — the formula is inside
- Checkpoint soldiers let through players with vaccines (proof of non-contamination)
- Formula recovered → 3 ingredients still missing
- Antenna reactivation → first radio contact with other survivors

**NPCs:**
- `soldier_checkpoint` — tense, pragmatic. Has lab key (side quest to obtain it).
- `pharmacist` — barricaded, suspicious. Negotiate or trick.
- (Nurse Joy — dead at desk, voicemail only)

**Key locations:**
- Chen's Lab: locked, key from soldier side quest. Contains formula + research notes.
- Pharmacy: barricaded door, negotiation puzzle.
- Pokémon Center: empty, Joy's skeleton, voicemail plays on approach. No combat.
- Radio tower: broken antenna, repairable with parts found in the city.

**Quests:**
- 🟧 Find Chen's Lab + Recover the Formula (main)
- 🟨 The Reluctant Pharmacist
- 🟨 The Empty Pokémon Center (ambient, no objectives — pure atmosphere)
- 🟨 The Dead Radios — repair antenna, hear survivors

**Encounters:** Z-Rattata, Z-Mankey (city tier)

**Ambiance:** Last organized resistance. Soldiers pretend it's under control. It isn't.

---

### ⛏️ PEWTER CITY
**Act:** 4a
**State:** Army fallback camp. 20-30 survivors, tents in the museum courtyard. Rationed, tense, but alive.
**Epidemic status:** Low — army maintains the perimeter. Outlying routes are dangerous.

**Story beats:**
- Museum = makeshift hospital
- Old Skier NPC knows Mt. Moon's secret paths
- Side quest: find a lost child in Mt. Moon

**NPCs:**
- `camp_commander` — military, strict. Controls access north.
- `old_skier` — naturally cold-immune. Gives secret Mt. Moon path. Warm, dry humor.
- `lost_child_mother` — side quest giver

**Quests:**
- 🟨 The Lost Child — find the child in Mt. Moon, optional reward: rare vaccine
- 🟨 Old Skier's Path — unlocks a shortcut through Mt. Moon (avoids main zombie path)

**Loot note:** Ingredient findable in an abandoned house on the west side (no puzzle, simple exploration).

**Ambiance:** Fragile normalcy. People doing chores, rationing food. Military discipline barely holding grief at bay.

---

### 🏔️ MT. MOON *(Cold Sanctuary)*
**Act:** 4a
**State:** **SANCTUARY ZONE.** Virus inactive. No zombies. Pokémon normal.
**Epidemic status:** Zero — extreme cold kills/suspends the viral strain.

**Story beats:**
- Atmosphere completely shifts. Silence. Snow. Alive.
- First uninfected wild Pokémon catchable (via standard vaccine mechanic = capture)
- The Cave of Cries: optional mini-boss (half-frozen human mid-transformation — slow, tragic fight)

**Wild Pokémon (normal, capturable):** Zubat (healthy), Geodude, Clefairy, Jigglypuff, Swinub, Snorunt

**Special note:** The player's Pokémon **cannot get bite marks here** — zombie encounters are absent.

**Quests:**
- 🟨 The Cave of Cries — optional, emotional fight, no real reward except closure
- 🟨 Lost Child (from Pewter) found in a side cave

**Ambiance:** Peace after horror. White, quiet, cold. The player exhales for the first time in hours.

---

### 🌊 CERULEAN CITY
**Act:** 4b
**State:** **MEGA-HORDE BLOCKS CENTER.** Periphery accessible. Center locked until explosion event.
**Epidemic status:** Heavy — evacuation failed, horde settled in the city center.

**Unlock condition:** Find fuel tank controls at the port → trigger explosion → horde disperses into the sea → ~15 turn window to explore center before stragglers return.

**Story beats:**
- Port area accessible from start: abandoned boats, scattered luggage (visual storytelling)
- After explosion: Department Store, Pokémon Center (partially flooded), city center loot
- Suspended bridge side quest

**NPCs:**
- `bridge_survivor` — lone survivor on Route 24 side of the cut bridge. Refuses trust. Long dialogue, player choice.

**Quests:**
- 🟧 Disperse the Cerulean Horde (main — triggers center access)
- 🟨 The Suspended Bridge
- 🟨 The Submerged Pokémon Center — flooded, rare loot, zombies wade in water

**Encounters (center, post-explosion):** Z-Psyduck, Z-Goldeen, Z-Poliwag — water-type zombies

**Ambiance:** Tragedy frozen in time. Suitcases everywhere. A queue that never moved.

---

### ⚡ VERMILION CITY
**Act:** 4c
**State:** **Military stronghold.** The only fully organized human zone in Kanto. Strict, controlled, safe — for now.
**Epidemic status:** Minimal inside the perimeter. Army-patrolled.

**Story beats:**
- Earn the Admiral's trust → access to SS Anne + boat to Cinnabar
- SS Anne: hospital ship, survivor community, multiple quests
- Corrupt soldier black market side quest

**NPCs:**
- `admiral` — rigid, pragmatic, not cruel. Knows the situation is worse than official reports. Will cooperate if player proves worth.
- `corrupt_soldier` — selling vaccines privately. Discoverable via exploration.

**Access to Cinnabar:** Admiral provides a small motorboat after player completes a trust-building task (TBD in Act design).

**Quests:**
- 🟧 Earn the Admiral's Trust (main — unlocks Cinnabar)
- 🟨 The SS Anne — survivor community hub, 2-3 quests
- 🟨 The Corrupt Soldier — denounce (soldier arrested, player gets reward from Admiral) or exploit (buy cheap vaccines)

**Loot note:** Ingredient #2 findable in the SS Anne cargo hold.

**Ambiance:** Order imposed on chaos. Soldiers know it's temporary. Player can feel the clock ticking.

---

### 🌋 CINNABAR ISLAND *(Volcanic Sanctuary)*
**Act:** 4c
**State:** **SANCTUARY ZONE.** Volcanic heat suppresses the virus. Intact, alive.
**Epidemic status:** Zero — island residents sealed off the ferry port on Day 2. Nobody came. Nobody left.

**Story beats:**
- Mortimer's childhood here: first lab, hospital research notes, the innocent beginning
- The Caninos Breeder NPC: unknowingly describes young Mortimer. Key narrative revelation.
- Player starts to understand Mortimer as a person, not a monster

**Wild Pokémon (normal, capturable):** Vulpix, Growlithe, Magmar, Ponyta

**Key locations:**
- Mortimer's First Lab: small, dusty. Childhood notebooks, early cellular research. Photo of him at 12 with a healthy Caninos.
- Mortimer's Hospital: degenerative disease ward. Patient files (Caninos). His notes grow more desperate across years.
- Volcano summit (optional): high-level Pokémon, rare loot, spectacular view

**NPCs:**
- `caninos_breeder` — 60s, warm, been raising Caninos for 30 years. Mentions "a doctor friend from his youth with a sick Caninos." Doesn't connect the dots. Player does.

**Quests:**
- 🟧 Find Mortimer's Artifacts (main — narrative, no combat required)
- 🟨 The Volcano Summit — optional combat zone, rare Pokémon

**Ambiance:** Warm, melancholy, retrospective. The most human zone in the game. Player's anger toward Mortimer starts to soften.

---

### 👻 LAVENDER TOWN *(The Epicenter)*
**Act:** 4d
**State:** **COMPLETELY DEVASTATED.** Ground zero. Stage III zombies everywhere + frequent Rage variants. Necronos is nearby — unseen, but felt.
**Epidemic status:** Maximum. Oldest infection in the world. The air feels wrong.

**Story beats:**
- First serious Vector grunt encounter → dropped dossier → first evidence of the organization
- Pokémon Tower explored floor by floor → trapdoor found at basement level
- Underground lab discovered → full truth of the outbreak revealed
- Rival breaks down, confides everything about Vector

**Key locations:**
- **Pokémon Tower** (above ground): 7 floors, infested with Z-Haunter and Z-Gengar (Spectral Bite — ignores defenses). Top floor has the trapdoor entrance.
- **Mortimer's Underground Lab** (below tower): 3 rooms. Notes, equipment, photos. A destroyed wall where Necronos broke through. The moment the player understands everything.
- **Necronos sighting**: scripted event — player glimpses Necronos in the lab ruins. It flees. Cannot be caught here.

**NPCs:**
- `rival_2` — found in the ruins, alone. Shares everything. Does not join team.
- `vector_grunt_1` — flees on encounter, drops dossier. No fight intended (can fight if cornered).

**Quests:**
- 🟧 Explore the Tower + Find the Lab (main)
- 🟧 Second Rival contact (scripted, main narrative)
- 🟨 The Transmissions — repair radio in the lab, receive Pokémon League signals
- 🟨 The Old Trainer — floor 5 of the tower, a zombie in a trainer uniform who seems to recognize the player. Tragic fight.

**Encounters:** Z-Haunter, Z-Gengar, Z-Gastly (ghost type — Spectral Bite, ignores defenses), Z-Cubone

**Ambiance:** The worst place in the world. And the most important. Pure horror, pure truth.

---

### 🏬 CELADON CITY *(The Fallen Fortress)*
**Act:** 5
**State:** **MEGA-HORDE BLOCKS ENTIRE CITY.** Inaccessible until explosion event.
**Epidemic status:** Extreme. Army's last stand. Fell 10 days ago. Stage I/II mix (relatively recent fall).

**Unlock condition:** Find the military armory depot schematics (in Viridian archives or Vermilion command) → trigger controlled explosion of the peripheral ammo dump → horde disperses → **limited exploration window** (~20 turns) before stragglers return.

**Story beats:**
- Department Store: 8 floors, each a different "biome" (food, medical, armory, command)
- General's journal: the 10-day defense, day by day. Heartbreaking.
- Survivor still alive, barricaded in a bathroom on floor 4

**Key locations:**
- Department Store Floor 1: Entrance, overturned barricades, bodies of soldiers and zombies together
- Floor 2: Food — most looted but scraps remain
- Floor 3: Medical — vaccines, medkits, anti-bite items
- Floor 5: Armory — weapons, military gear (flavor only), ingredient #3
- Floor 7: Command — General's post. Journal. Radio with one last transmission.
- Floor 4 Bathroom: Survivor barricaded inside (side quest)

**Quests:**
- 🟧 Unlock and Explore Celadon (main — ingredient #3 here)
- 🟨 The General's Journal (ambient — read all 10 entries)
- 🟨 The Bathroom Survivor — rescue or witness

**Encounters (during window):** Z-Eevee, Z-Vaporeon, Z-Flareon — Stage I (dangerous, fast)

**Ambiance:** Grandeur in ruins. The best-equipped place in Kanto, now the most dangerous. Time pressure creates tension.

---

### 🎯 FUCHSIA CITY
**Act:** 5
**State:** Partially standing. South residential area held by survivors. Safari Zone = catastrophic.
**Epidemic status:** Moderate-high. Zoo breach released unknown Pokémon types into the city.

**Story beats:**
- South survivor community (20 people, organized, not military)
- Safari Zone: escaped zoo Pokémon, extremely dangerous, high reward
- Quest to close the zoo gates and protect the community

**NPCs:**
- `safari_keeper` — last zoo employee. Knows every Pokémon in there. Gives tactical info.
- `community_leader` — runs the south residential group, gives quests

**Quests:**
- 🟧 Close the Safari Zone Gates (main — protects community, unlocks safer south district)
- 🟨 Safari Zone Hunt — optional, rare Pokémon (Z-Kangaskhan, Z-Tauros, Z-Scyther), exceptional loot

**Encounters (Safari Zone):** High-tier zombies, all Stage I or Rage. Very dangerous.

**Ambiance:** Unexpected life in the south. Unexpected death in the north. A city split in two.

---

### 🏢 SAFFRON CITY *(Vector HQ)*
**Act:** 5
**State:** Team Vector-controlled. Perimeter maintained. Surface relatively safe (Vector's doing). Underground = the real threat.
**Epidemic status:** Managed by Vector — they keep the surface clear to maintain operations.

**Story beats:**
- Infiltration: avoid patrols or find a grunt disguise
- Silph Co. tower: 10 floors, increasingly dangerous Vector presence
- Underground lab: 3 levels, Mortimer's most advanced facility
- Mortimer confrontation at level -3
- Necronos escapes during the fight

**NPCs:**
- `rival_3` — already infiltrated. Joins for the assault. First time they fight alongside.
- `vector_grunts` — morally complex. Some threaten. Some doubt. One dying in a corridor tells the truth about Vector's promises.
- `mortimer` — final confrontation. Tired, not epic. Conversation, then combat, then the formula, then silence.

**Infiltration options:**
- Stealth (hide tiles in the city, avoid patrol routes)
- Disguise (side quest: find Vector grunt uniform in the city)
- Direct assault (harder, more combat)

**Quests:**
- 🟧 Infiltrate Silph Co. (main)
- 🟧 Reach Mortimer — Underground Level -3 (main)
- 🟧 Mortimer confrontation (main — formula obtained, Necronos escapes)
- 🟨 The Rival's Assault — coordinate with Rival for a distraction (reduces grunt encounters)
- 🟨 The Doubting Grunt — help a Vector grunt defect (affects epilogue)

**Encounters:** Vector grunts (human type — no vaccine), Vector Pokémon (vaccinated by Vector, not zombies — normal combat)

**Ambiance:** Institutional horror. Clean corridors, fluorescent lights, the smell of disinfectant. The most human evil — organized, bureaucratic, desperate.

---

### 🏔️ INDIGO PLATEAU *(The Bunker)*
**Act:** 6
**State:** Sealed. Accessible only after synthesizing the human vaccine at Viridian.
**Epidemic status:** Zero — fortified, sealed. The League has been here since Day 3.

**Story beats:**
- The Champion: exhausted, not heroic. Believes in reconstruction. Equips the player.
- Necronos tracked to the Plateau ruins (old arena, overgrown)
- Final battle: vaccinate Necronos → Caninos reverts → dies peacefully on Mortimer's grave
- Then: return to Pallet. Mom. The end.

**NPCs:**
- `champion` — NPC, gives resources, explains what the League has been doing
- `league_technician` — has been tracking Necronos's movements. Gives location data.

**Final battle notes:**
- Necronos uses Vector Bite: 2 bite marks guaranteed per hit. Player's team will take heavy damage.
- Cannot flee. Must vaccinate (vaccine button available even at full HP — unique to this fight).
- After vaccination: cutscene. Caninos reverts. Lies down. Fire glands flicker. Out.

**Ambiance:** End of the world, beginning of something else. Quiet. Cold. The arena empty except for one small dog.

---

## Zone Connection Map

```
Indigo Plateau
      ↑
  [Route 23]
      ↑
  Viridian City ←──── Route 2 ←──── Pewter City ←── Mt. Moon ←── Cerulean City
      ↑                                                                  ↑
  Route 1                                                          Routes 24-25
      ↑
  Pallet Town

  Cerulean ──→ Routes 5-6-7 ──→ Vermilion City ──→ [boat] ──→ Cinnabar Island

  Vermilion ──→ Route 11-12 ──→ Lavender Town

  Lavender ──→ Routes 8-9-10 ──→ Celadon City

  Celadon ──→ Routes 15-16-17 ──→ Fuchsia City

  Fuchsia ──→ Routes 13-14 ──→ [connects back to Cerulean area]

  Saffron City: center of Kanto, connected to Cerulean / Vermilion / Celadon / Lavender
```

---

## Blocked Zones Summary

| Zone | Blocked By | Unlock Trigger |
|---|---|---|
| Viridian City (north gate) | Locked until Chen fight done | Complete Act 1 |
| Cerulean City (center) | Mega-horde | Port fuel tank explosion |
| Celadon City | Mega-horde | Controlled armory explosion (schematics from Viridian/Vermilion) |
| Indigo Plateau | League sealed roads | Synthesize human vaccine in Act 6 |
| Saffron City (Silph interior) | Vector checkpoints | Infiltration or disguise |
| Mom's house | Locked door | Human vaccine + Act 6 return |

---

## Ingredient Locations (non-puzzle, casual loot)

The 3 ingredients for the human vaccine formula are **not quest-gated**. They're found through normal exploration:

| # | Location | How |
|---|---|---|
| 1 | Pewter City — abandoned house, west side | Normal loot exploration |
| 2 | SS Anne cargo hold (Vermilion) | Exploration of the ship |
| 3 | Celadon Department Store — Floor 5 armory | During the timed exploration window |

If a player misses one during their first visit, a fallback loot location exists in Viridian City (Chen's lab storage — he had partial ingredients already).
