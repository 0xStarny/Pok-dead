# POKÉDEAD — Gameplay Reference

All mechanics, tuning values, and interactions. Implementation reference.

## Core stats

### Player
- HP: 50/50 base
- Hunger: 100/100 (depletes 1 per 3 steps; at 0, lose 2 HP per step)
- Day counter: +1 every 30 steps
- Vaccines: shown as a separate counter (used in combat, very valuable)

### Pokémon team
- Max 4 in team
- Each: name, type, mhp/hp, atk, sprite, isZombie, cured (★ marker), bites, zombified
- Bite count: 0/3 → 3/3 (then turns)
- Bite count drops by 1 per day if Pokémon is rested (in team but unused in combat)

## Type chart

```
fire     → grass×2, water×0.5, rock×0.5, flying×1.5
water    → fire×2, grass×0.5, rock×2
grass    → water×2, fire×0.5, rock×2, flying×0.5, poison×0.5
normal   → rock×0.5, ghost×0
flying   → grass×2, rock×0.5
poison   → grass×2, rock×0.5
rock     → flying×2, fire×2, grass×0.5
ghost    → normal×0, ghost×2
human    → (no relations — neutral against everything)
```

## Combat formula

```
base_dmg = atk + random(0..4) - 2
effective_dmg = max(1, round(base_dmg × type_effectiveness))
if crit:  effective_dmg = round(effective_dmg × 1.7)
```

**Crit chance:** 8% for player attacks, 5% for enemy attacks.

**Visual feedback per hit:**
- Floating damage number (color by type: white=normal, orange=super, gray=weak, yellow=crit, green=heal)
- Hit flash on target (brightness + red sepia for 0.25s)
- Screen shake on stage (0.3s)
- Effectiveness banner ("SUPER EFFECTIVE!" / "CRITICAL HIT!" / "not effective…" / "NO EFFECT")

## Vaccine mechanic (capture)

When you use a vaccine in combat:

```
hp_pct = enemy.hp / enemy.mhp
success_rate = 
    boss      → max(0, 0.5 - hp_pct)   (must be very low HP)
    normal    → (1 - hp_pct) × 0.9 + 0.1
    introBoss → CANNOT VACCINATE (achieve only)
    human     → CANNOT VACCINATE (yet — placeholder for the human vaccine arc)
```

If success: the zombie returns to a healthy Pokémon, joins the team if space (max 4), gets the cured (★) marker, slight stat buff vs original wild form.

## Bite mechanic (v0.3)

Every enemy attack on a Pokémon adds **+1 bite mark**. Visible on the team panel and combat info card.

- 1/3 bites: visible warning, no effect yet
- 2/3 bites: log alert, "%name% has bites: 2/3 — careful!"
- 3/3 bites: **zombification triggers**. The Pokémon attacks the player one last turn, then flees combat. It joins the wild zombie pool — you can later encounter it as a wild zombie and vaccinate to recover.

Bite count drops by 1 per day if the Pokémon is rested (in team but unused).

**Anti-Bite item** (rare loot): erases 1 bite mark instantly.

## Hordes (v0.3)

### Detection
Manhattan distance ≤ 5 → horde sees player.

### Movement
- Player turn → moves 1 tile
- Then horde turn → moves 1 tile toward player (Manhattan greedy)
- **1/3 chance enraged**: horde moves **2 tiles per turn** (visually marked with red pulsing border)

### Hide
- Hide tiles in zones (`b` in map = tall bushes, drawn green and bushy)
- On a hide tile, press **A** to hide
- 3-turn invisibility, horde wanders away
- Each hide spot only works **once per horde**

### Caught
- 3-zombie sequence battle, no flee
- Reward: 50% chance of rare item drop
- Bites accumulate faster (consecutive fights)

### Horde data
```js
G.hordes = {
  bourg_palette: [],
  kanto: [
    {id:'h1', x:7, y:3, enraged:false, defeated:false},
    {id:'h2', x:11, y:5, enraged:true, defeated:false}
  ]
};
G.hordeChase = {hordeId, zoneId, hideTurnsLeft};  // null if not chasing
```

## Tiles

| Char | Name | Pass | Encounter rate | Loot type | Notes |
|---|---|---|---|---|---|
| `.` | grass | ✓ | 22% | — | base |
| `F` | forest | ✓ | 40% | — | dangerous |
| `R` | road | ✓ | 8% | — | safe-ish |
| `C` | city | ✓ | 42% | city | dense zombies |
| `H` | hospital | ✓ | 25% | hospital | medkits, vaccines |
| `X` | store | ✓ | 10% | store | food |
| `M` | mountain | ✗ | — | — | wall |
| `W` | water | ✗ | — | — | wall |
| `L` | lab | ✗ | — | — | barricaded for now |
| `h` | house exterior | ✗ | — | — | locked (Mom's house) |
| `d` | house door | ✓ | — | — | enter interior |
| `E` | house exit | ✓ | — | — | leave interior |
| `_` | indoor floor | ✓ | 0% | — | inside houses |
| `#` | wall | ✗ | — | — | indoor walls |
| `B` | bed | ✓ | — | — | sleep if house clean |
| `b` | hide bush | ✓ | 0% | — | hide from hordes |
| `G` | gate | ✓ | — | — | zone transition |

## Loot tables

| Source | Probability | Possible drops |
|---|---|---|
| Hospital tile | 65% | 50% medkit (1-2), 35% vaccine, 15% food |
| Store tile | 65% | 100% food (2-4) |
| City tile | 65% | 60% food, 30% medkit, 10% vaccine |
| Combat win | 30% | 50% food, 30% medkit, 20% vaccine |
| Horde win | 50% | mostly medkits/vaccines |
| Dead body in house | 100% | 50% medkit, 30% food, 20% vaccine |

## Sleep / Rest (v0.3 changes)

**v0.2 (current):** "Rest" button always available — restores HP, costs 20 hunger, +1 day, 35% chance of night encounter.

**v0.3 (target):** Rest button **removed**. Sleep is only available at **bed tiles inside cleared houses**.
- Bed shows animated "ZzZ" overlay only when usable
- House is "clean" when: no zombies left + all loot taken
- Sleep effect: HP +40% (vs +25% for rest), bite count -1 on each Pokémon, +1 day, -20 hunger, 35% night encounter chance

## Save / Load

### Auto-save
After every meaningful action (move with loot, combat end, dialog finish, quest objective). Stored in `localStorage.pokedead_save` as JSON.

### Manual file save
From in-game menu → "Save to file" → downloads `pokedead_<name>_jour<N>.save` (just a JSON file).

### Import
From launcher → "Import Save" → file picker → loads JSON.

### Continue button
Enabled in launcher only if `localStorage.pokedead_save` exists.

## Quest system

### Data shape
```js
QUEST_DEFS = {
  q_id: { type: 'main' | 'side', objectives: ['oid1', 'oid2', ...] }
};

G.quests[qid] = {
  active: bool,
  completed: bool,
  objDone: { oid: bool, ... }
};
```

### Translation lookup
```
quest.{qid}.title         → 'The Awakening'
quest.{qid}.desc          → 'Chen has something to tell you...'
quest.{qid}.{oid}         → 'Talk to Prof. Chen'
```

### NPC quest markers
- `!` orange = main quest available
- `?` yellow = side quest available
- `?` gray = side quest in progress, completable now
- nothing = no quest, NPC may have flavor dialogue

## Encounter scaling

The current map gates difficulty by tier:
```js
tier = min(3, 1 + floor(steps / 25))
```

Available zombies: tier 1 (early) → tier 4 (late). Boss is excluded from random pool.

Tier 1: Z-Rattata, Z-Pidgey, Z-Zubat, Z-Human-Man, Z-Human-Woman
Tier 2: Z-Geodude, Z-Grimer
Tier 3: Z-Haunter
Tier 4: Z-Tauros (boss, intentional encounter only)

## Player movement

### Keyboard
- Arrow keys / WASD: move
- I: open inventory
- T: open team
- Esc: open menu
- Space / Enter: action button (interact with adjacent NPC, advance dialog)

### D-pad / mobile
- 4 arrows + center "A" button (action / interact)

### NPC interaction
- Walking onto an NPC tile triggers dialog (Pokémon-style)
- "A" button interacts with adjacent NPC if no facing direction
