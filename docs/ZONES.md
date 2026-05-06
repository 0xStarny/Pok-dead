# POKÉDEAD — Complete Zone Reference

All zone IDs, connections, entry coords, and house mappings. Use this to navigate the world graph.

---

## Overworld zone graph

```
bourg_palette ──(north)── route_1 ──── jadielle ──── route_2 ──── argenta
                                                                       │
                                                                   mt_moon
                                                                       │
cinnabar ──── route_21 ──── safrania ──── route_7 ──── celadopole ──── route_3 ──── azuria ──── route_4 ──── lavandia ──── route_8 ──╮
                                                                                                                                      │
                                                                                                                            (connects back)
```

Simpler linear view: `bourg_palette → route_1 → jadielle → route_2 → argenta → mt_moon → route_3 → azuria → route_4 → lavandia → route_8 → celadopole → route_7 → safrania → route_21 → cinnabar`

---

## Overworld zones

| Zone ID | Name | Size | Entry from | Gates to |
|---|---|---|---|---|
| `bourg_palette` | Bourg Palette | 12×10 | — | route_1 (north), route_21 (south) |
| `route_1` | Route 1 | 10×8 | bourg_palette | jadielle (north), bourg_palette (south) |
| `jadielle` | Jadielle | 14×10 | route_1 | route_2 (north), route_1 (south) |
| `route_2` | Forêt de Jade | 10×8 | jadielle | argenta (north), jadielle (south) |
| `argenta` | Argenta | 12×10 | route_2 | mt_moon (north), route_2 (south) |
| `mt_moon` | Mont Selenite | 10×10 | argenta | route_3 (north), argenta (south) |
| `route_3` | Route 3 | 10×8 | mt_moon | azuria (north), mt_moon (south) |
| `azuria` | Azuria | 14×10 | route_3 | route_4 (north), route_3 (south) |
| `route_4` | Route 4 | 10×8 | azuria | lavandia (north), azuria (south) |
| `lavandia` | Lavandia | 12×10 | route_4 | route_8 (north), route_4 (south) |
| `route_8` | Route 8 | 10×8 | lavandia | celadopole (north), lavandia (south) |
| `celadopole` | Céladopole | 14×10 | route_8 | route_7 (north), route_8 (south) |
| `route_7` | Route 7 | 10×8 | celadopole | safrania (north), celadopole (south) |
| `safrania` | Safrania | 14×10 | route_7 | route_21 (south) |
| `route_21` | Route 21 | 10×8 | safrania | cinnabar (north), safrania (south) |
| `cinnabar` | Cinnabar | 10×8 | route_21 | route_21 (south) |

---

## Interior zones — city buildings

### Bourg Palette

| House ID | Zone | Entry | Notes |
|---|---|---|---|
| `h_bourg_player` | `h_player` | [3,6] | **LOCKED** until `G.flags.motherSaved` |
| `h_bourg_lab` | `h_chen_lab` | [5,8] | Chen's lab — auto-dialog on first entry |
| `h_bourg_bertrand` | `h_bertrand` | [3,4] | Bertrand's house |

### Jadielle

| Tile coord | House ID | Zone | Notes |
|---|---|---|---|
| `3,4` | `h_jad_1` | `h_house_b` | |
| `11,4` | `h_jad_2` | `h_house_a` | |
| `1,2` | `h_jad_lab` | `h_lab` | |
| `11,2` | `h_jad_pc` | `h_pcenter_dmg` | PokéCenter (damaged) |
| `2,7` | `h_jad_ps` | `h_pshop_ok` | PokéMart (intact) |

### Argenta

| Tile coord | House ID | Zone | Notes |
|---|---|---|---|
| `3,3` | `h_arg_1` | `h_medium` | |
| `9,3` | `h_arg_2` | `h_ruined` | |
| `3,7` | `h_arg_pc` | `h_pcenter_dmg` | PokéCenter (damaged) |
| `3,6` | `h_arg_ps` | `h_pshop_dmg` | PokéMart (damaged) |

### Azuria

| Tile coord | House ID | Zone | Notes |
|---|---|---|---|
| `3,3` | `h_az_1` | `h_medium` | |
| `3,7` | `h_az_pc` | `h_pcenter_ok` | PokéCenter (intact — best loot) |
| `3,5` | `h_az_ps` | `h_pshop_dmg` | PokéMart (damaged) |

### Lavandia

| Tile coord | House ID | Zone | Notes |
|---|---|---|---|
| `1,4` | `h_lav_1` | `h_ruined` | |
| `7,4` | `h_lav_2` | `h_house_a` | |
| `1,2` | `h_lav_lab` | `h_lab` | |
| `9,6` | `h_lav_pc` | `h_pcenter_dead` | PokéCenter (looted — nothing) |
| `3,5` | `h_lav_ps` | `h_pshop_dead` | PokéMart (looted — nothing) |

### Céladopole

| Tile coord | House ID | Zone | Notes |
|---|---|---|---|
| `3,2` | `h_cel_1` | `h_house_b` | |
| `9,2` | `h_cel_2` | `h_shop_in` | |
| `3,7` | `h_cel_pc` | `h_pcenter_dmg` | PokéCenter (damaged) |
| `2,6` | `h_cel_ps` | `h_pshop_ok` | PokéMart (intact) |
| `1,1` | `h_cel_imm` | `h_immeuble_g` | Apartment building (multi-floor) |

### Safrania

| Tile coord | House ID | Zone | Notes |
|---|---|---|---|
| `4,3` | `h_saf_1` | `h_house_b` | |
| `9,3` | `h_saf_2` | `h_house_a` | |
| `1,2` | `h_saf_lab` | `h_lab` | |
| `9,7` | `h_saf_pc` | `h_pcenter_dead` | PokéCenter (looted) |
| `3,6` | `h_saf_ps` | `h_pshop_dead` | PokéMart (looted) |
| `1,1` | `h_saf_imm` | `h_immeuble_g` | Apartment building (multi-floor) |

### Cinnabar

| Tile coord | House ID | Zone | Notes |
|---|---|---|---|
| `1,2` | `h_cin_lab` | `h_cinnabar_lab` | **Win condition** — auto-dialog on entry, synthesizes vaccine |

---

## Interior zone catalogue

| Zone ID | Size | Entry | Type | Notes |
|---|---|---|---|---|
| `h_player` | 8×8 | [3,6] | house | Player's home — barricaded until motherSaved |
| `h_chen_lab` | 10×10 | [5,8] | lab | Auto-dialog on first entry |
| `h_bertrand` | 8×6 | [3,4] | house | Bertrand's place |
| `h_small` | 8×6 | [3,4] | house | Small house variant |
| `h_medium` | 10×8 | [4,6] | house | Medium house with shelves |
| `h_lab` | 10×8 | [4,6] | lab | Generic city lab with lab_shelf |
| `h_cinnabar_lab` | 12×10 | [6,8] | lab | Endgame lab — triggers win condition |
| `h_house_a` | 10×6 | [4,4] | house | 2-bed house |
| `h_house_b` | 8×8 | [3,6] | house | L-shaped layout |
| `h_house_c` | 10×8 | [4,6] | house | Larger house, 2 beds + shelf room |
| `h_appart` | 8×8 | [3,6] | apartment | Compact apartment |
| `h_ruined` | 8×6 | [3,4] | house | Destroyed — barely anything left |
| `h_shop_in` | 10×6 | [4,4] | store | Corner shop interior |
| `h_pcenter_ok` | 12×8 | [6,6] | PokéCenter | Intact — full equipment + shelves |
| `h_pcenter_dmg` | 12×8 | [6,6] | PokéCenter | Damaged — partial loot |
| `h_pcenter_dead` | 10×6 | [4,4] | PokéCenter | Looted — nothing |
| `h_pshop_ok` | 10×8 | [4,6] | PokéMart | Intact — full shelves |
| `h_pshop_dmg` | 10×6 | [4,4] | PokéMart | Damaged — partial shelves |
| `h_pshop_dead` | 8×6 | [3,4] | PokéMart | Looted — nothing |
| `h_immeuble_g` | 10×8 | [4,6] | apartment | Ground floor — has `t` (stairs) tile |
| `h_immeuble_1` | 10×8 | [3,6] | apartment | Floor 1 — `parent: h_immeuble_g` |

---

## Encounter pools by zone

| Zone | Encounters |
|---|---|
| bourg_palette | z_rattata, z_pidgey |
| route_1 | z_rattata, z_pidgey (×2) |
| jadielle | z_rattata, z_pidgey, z_zubat |
| route_2 | z_rattata, z_pidgey, z_zubat |
| argenta | z_geodude, z_zubat, z_grimer |
| mt_moon | z_zubat, z_geodude, z_grimer (×2) |
| route_3 | z_pidgey, z_geodude, z_grimer |
| azuria | z_grimer, z_haunter (×2), z_zubat, **z_human** |
| route_4 | z_pidgey, z_zubat, z_grimer, z_haunter |
| lavandia | z_haunter, z_haunter_feral, z_grimer, z_zubat, **z_human** (×2) |
| route_8 | z_haunter, z_haunter_feral, z_grimer, z_geodude |
| celadopole | z_haunter_feral, z_tauros_roam, z_grimer, z_haunter, **z_human** (×2) |
| route_7 | z_tauros_roam, z_haunter_feral, z_grimer |
| safrania | z_tauros_roam, z_haunter_feral (×2), z_grimer, **z_human** (×3) |
| route_21 | z_tauros_roam (×2), z_haunter_feral |
| cinnabar | (none) |

---

## Enemy data

| Key | Name | Type | HP | ATK | Notes |
|---|---|---|---|---|---|
| `z_chen` | Pr. Chen (infecté) | normal | 42 | 9 | `isHuman`, `introBoss` — intro boss |
| `z_human` | Survivant infecté | normal | 20 | 6 | `isHuman` — vaccine blocked |
| `z_rattata` | Z-Rattata | normal | 24 | 8 | tier 1 |
| `z_pidgey` | Z-Roucool | vol | 28 | 9 | tier 1 |
| `z_zubat` | Z-Nosferapti | poison | 26 | 10 | tier 1 |
| `z_geodude` | Z-Racaillou | roche | 46 | 13 | tier 2 |
| `z_grimer` | Z-Tadmorv | poison | 60 | 15 | tier 2 |
| `z_haunter` | Z-Spectrum | spectre | 55 | 18 | tier 3 |
| `z_haunter_feral` | Z-Spectrum Sauvage | spectre | 70 | 22 | tier 3 |
| `z_tauros_roam` | Z-Tauros | normal | 85 | 19 | tier 3 |
| `z_tauros` | Z-Tauros (Boss) | normal | 120 | 24 | `boss` |
