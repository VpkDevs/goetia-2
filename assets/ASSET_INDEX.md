# DEMONICON — Master Asset Index

Every entity in the game that could conceivably need art, with two independent
scores and a verdict.

- **PG** = Procgen score (1–10). How good this looks generated/code-driven.
- **RA** = Real-Asset score (1–10). How important it is to ditch procgen.
- **Invariant:** no row has both ≥7. Both-low = it barely matters (`skip`).
- **Tier** = engine work required. See `ENGINE_REALITY.md`.
  A = works today · B = small UI-texture change · C = full material pipeline · D = outside engine.

Names below are pulled from `demonicon/data/*.ron`. They are the real IDs.

---

## 0. Executive summary

| Bucket | Rows | Verdict |
|---|---|---|
| In-world 3D (enemies, props, rooms, player) | 45 | **Stay procedural.** Engine can't texture them; the flat look already ships at 764 fps. |
| 2D UI icons (skills, sigils, statuses, items, rarity) | 59 | **Go real.** Tier B change unlocks the biggest perceived-polish jump in the project. |
| Bestiary / codex portraits | 16 | **Go real** if you ever add a codex screen. Otherwise defer. |
| VFX, particles, lighting | 12 | **Stay procedural.** This is the engine's superpower. |
| Audio | 6 | **Mixed** — real SFX + real music beat the synth. |
| Marketing / store (Tier D) | 14 | **Go real. Start here.** Zero engine work, highest commercial return. |

**If you only do one batch:** the 14 marketing assets (§10).
**If you only do two:** add the 59 UI icons (§4–§7).

---

## 1. Player & characters

| # | Entity | Tier | PG | RA | Verdict |
|---|---|---|---|---|---|
| 1.1 | Player avatar (in-world) — spike + orb, hex-emissive | C | 9 | 2 | **Procgen.** Silhouette reads at iso distance; emissive marks the player as brightest persistent object (Pillar 3). Don't touch. |
| 1.2 | Player portrait / bust (menus, future codex) | B | 2 | 7 | **Real.** No procgen path exists. Only needed if you add a character screen. |
| 1.3 | Player death/dissolve state | C | 9 | 1 | **Procgen.** Dissolve shader already does it. |
| 1.4 | Classes / subclasses | — | — | — | **N/A — the game has no class system.** Builds *are* the class: 8 skills × 20 sigils × 9 contracts. Nothing to draw. If you ever add archetype icons for a build-preset UI, treat as §4. |
| 1.5 | NPCs | — | — | — | **N/A — the game has zero NPCs.** The Court is 72 empty seats. Roadmap only (see §12). |

---

## 2. Enemies — in-world representation (Tier C)

All 13 are `MeshBuilder` primitives driven by `EnemyShape` + vertex wobble.
Uniform verdict: **procgen**. The engine cannot texture these, and shape+motion+
emissive is the stated identity system ("no rigs exist").

| # | ID | Name | Shape | Realm | PG | RA | Verdict |
|---|---|---|---|---|---|---|---|
| 2.1 | `index_wraith` | INDEX-WRAITH | Spike | Vassago | 9 | 2 | Procgen |
| 2.2 | `ledger_golem` | LEDGER GOLEM | Golem | Vassago | 9 | 2 | Procgen |
| 2.3 | `card_swarm` | CARD SWARM | Orb | Vassago | 8 | 3 | Procgen — could use a swarm-of-cards particle flourish |
| 2.4 | `vault_sentinel` | VAULT SENTINEL | Golem | Vassago | 9 | 2 | Procgen |
| 2.5 | `schism_knight` | SCHISM KNIGHT | Spike | Andras | 9 | 2 | Procgen |
| 2.6 | `frenzy_hound` | FRENZY HOUND | Spike | Andras | 8 | 3 | Procgen — quadruped reads weakly as a spike; consider a custom `MeshBuilder` chain |
| 2.7 | `discord_herald` | DISCORD HERALD | Column | Andras | 9 | 2 | Procgen |
| 2.8 | `blight_clergy` | BLIGHT-CLERGY | Column | Buer | 9 | 2 | Procgen |
| 2.9 | `spore_wheel` | SPORE WHEEL | Wheel | Buer | 10 | 1 | Procgen — rolling prism is *ideal* procgen; best-in-class already |
| 2.10 | `bloom_tender` | BLOOM TENDER | Column | Buer | 9 | 2 | Procgen |
| 2.11 | `boss_vassago` | VASSAGO, FINDER OF HIDDEN THINGS | Orb | Vassago | 8 | 3 | Procgen mesh — but see §3 for portrait |
| 2.12 | `boss_andras` | ANDRAS, SOWER OF DISCORD | Spike | Andras | 8 | 3 | Procgen mesh — see §3 |
| 2.13 | `boss_buer` | BUER, THE HEALER-WHEEL | Wheel | Buer | 8 | 3 | Procgen mesh — see §3 |

**Cheap procgen upgrade (no art needed):** give each enemy a bespoke
`MeshBuilder` composition instead of sharing 5 shapes. `spike().merged(orb())
.twisted().jittered()` chains are free and would double silhouette variety.

---

## 3. Boss & bestiary portraits (Tier B / D)

These are 2D. They do **not** replace the in-world mesh — they'd appear in a
codex, a boss-intro card, or marketing.

| # | Subject | PG | RA | Verdict |
|---|---|---|---|---|
| 3.1 | VASSAGO portrait — crimson, archive, veiled ledgers | 1 | 8 | **Real.** Prompt pack `06`. |
| 3.2 | ANDRAS portrait — sickly gold, broken cathedral, schism | 1 | 8 | **Real.** Pack `06`. |
| 3.3 | BUER portrait — spectral cyan, wheel-garden, blight/bloom | 1 | 8 | **Real.** Pack `06`. |
| 3.4–3.13 | 10 regular-enemy bestiary illustrations | 1 | 5 | **Real, but defer** — only worth it once a codex screen exists. |
| 3.14 | Boss-intro banner frame (shared chrome) | 4 | 6 | Real-ish. Could be a procedural UI rect for now. |
| 3.15 | Three realm "seal" emblems (heraldry per demon) | 3 | 8 | **Real.** High reuse: loading screens, Court seats, store page, contracts. Pack `07`. |
| 3.16 | Bestiary screen background | 5 | 5 | Skip until the screen exists. |

---

## 4. Skills — icons (Tier B)

8 skills, currently rendered as **truncated text** in the HUD skill bar
(`name.split(' ').next_back()`, 9 chars). Icons are the single biggest
readability + polish win in the game.

| # | ID | Name | Damage | PG | RA | Verdict |
|---|---|---|---|---|---|---|
| 4.1 | `hellbolt` | BOLT OF THE PYRE | Hellfire | 3 | 8 | **Real icon.** Pack `01`. |
| 4.2 | `voidnova` | HOLLOWING | Void | 3 | 8 | **Real icon.** |
| 4.3 | `ritual` | RITE OF THE DRAWN CIRCLE | Hex | 3 | 8 | **Real icon.** |
| 4.4 | `shades` | SUMMON INDEX-SHADES | Physical | 3 | 8 | **Real icon.** |
| 4.5 | `hexbeam` | SENTENCE OF UNMAKING | Hex | 3 | 8 | **Real icon.** |
| 4.6 | `ripdash` | RIP THROUGH | Physical | 3 | 8 | **Real icon.** |
| 4.7 | `unravel` | CURSE OF UNRAVELLING | Hex | 3 | 8 | **Real icon.** |
| 4.8 | `idol` | OBSIDIAN IDOL | Hellfire | 3 | 8 | **Real icon.** |

Spec: 512×512 source → 128×128 in atlas. Must read at **48 px** (HUD slot is
~74 px wide with padding). Tint-able: author mostly-monochrome + let the damage
type color it, or bake the type color in. Recommend **baked color** so damage
type is instantly legible.

---

## 5. Sigils — icons (Tier B)

20 sigils, currently shown as gold pips + a name string with `"SIGIL OF "`
stripped. Icons here are lower priority than skills (they appear in the menu,
not the combat HUD) but they're the same production run.

| # | ID | Name | PG | RA |
|---|---|---|---|---|
| 5.1 | `echo_minor` | SIGIL OF THE SECOND VOICE | 3 | 7 |
| 5.2 | `echo_major` | SIGIL OF THE CHOIR | 3 | 7 |
| 5.3 | `brimstone` | SIGIL OF BRIMSTONE | 3 | 7 |
| 5.4 | `hexwright` | SIGIL OF THE HEXWRIGHT | 3 | 7 |
| 5.5 | `voidhollow` | SIGIL OF THE HOLLOW | 3 | 7 |
| 5.6 | `splinter` | SIGIL OF SPLINTERS | 3 | 7 |
| 5.7 | `lance` | SIGIL OF THE LANCE | 3 | 7 |
| 5.8 | `orbitals` | SIGIL OF ORBIT | 3 | 7 |
| 5.9 | `vast` | SIGIL OF THE VAST | 3 | 7 |
| 5.10 | `rapid` | SIGIL OF HASTE | 3 | 7 |
| 5.11 | `heavy` | SIGIL OF THE ANVIL | 3 | 7 |
| 5.12 | `swift` | SIGIL OF THE ARROW | 3 | 7 |
| 5.13 | `lingering` | SIGIL OF THE LONG HOUR | 3 | 7 |
| 5.14 | `arsonist` | SIGIL OF THE ARSONIST | 3 | 7 |
| 5.15 | `blighted` | SIGIL OF THE ROT | 3 | 7 |
| 5.16 | `petrifier` | SIGIL OF THE STONE EYE | 3 | 7 |
| 5.17 | `discordant` | SIGIL OF DISCORD | 3 | 7 |
| 5.18 | `detonator` | SIGIL OF THE DETONATOR | 3 | 7 |
| 5.19 | `gravemind` | SIGIL OF THE GRAVEMIND | 3 | 7 |
| 5.20 | `martyr` | SIGIL OF THE MARTYR | 3 | 7 |

**Verdict: real, batch 2.** These are *sigils* — abstract occult glyphs are
exactly what image models are good at, and they're cheap (one 4×5 sheet).
Consider generating as **white-on-black glyphs** and tinting in-engine, which
lets one asset serve socketed/unsocketed/hover states.

---

## 6. Contracts — seals (Tier B)

9 contracts, 3 per demon. Currently text-only in the PACTS pane.

| # | ID | Demon | Name | PG | RA |
|---|---|---|---|---|---|
| 6.1 | `appraisal` | Vassago | CONTRACT OF APPRAISAL | 3 | 7 |
| 6.2 | `veiled_pact` | Vassago | CONTRACT OF THE VEIL | 3 | 7 |
| 6.3 | `index_tax` | Vassago | CONTRACT OF THE INDEX | 3 | 7 |
| 6.4 | `discord_pact` | Andras | CONTRACT OF DISCORD | 3 | 7 |
| 6.5 | `schism` | Andras | CONTRACT OF SCHISM | 3 | 7 |
| 6.6 | `warfeast` | Andras | CONTRACT OF THE WAR-FEAST | 3 | 7 |
| 6.7 | `blight_communion` | Buer | CONTRACT OF BLIGHT-COMMUNION | 3 | 7 |
| 6.8 | `wheel_rider` | Buer | CONTRACT OF THE WHEEL-RIDER | 3 | 7 |
| 6.9 | `gardener` | Buer | CONTRACT OF THE GARDENER | 3 | 7 |

**Verdict: real, batch 3.** Wax-seal / sigillum motif, tinted per demon.
High thematic payoff for 9 images. Pack `07`.

---

## 7. Items, gear, rarity, currency (Tier B)

### 7a. Goetics — the 12 hand-designed uniques

These are the game's trophies. A named unique with no icon is a missed moment.

| # | ID | Name | Slot | PG | RA |
|---|---|---|---|---|---|
| 7.1 | `second_sorrow` | SECOND SORROW OF THE LEDGER | Relic | 2 | 8 |
| 7.2 | `crown_fires` | CROWN OF ALL FIRES | Armor | 2 | 8 |
| 7.3 | `patient_flame` | THE PATIENT FLAME | Ring | 2 | 8 |
| 7.4 | `open_vein` | THIRST OF THE OPEN VEIN | Ring | 2 | 8 |
| 7.5 | `basilisk` | BASILISK'S REGARD | Weapon | 2 | 8 |
| 7.6 | `standing_stones` | LITURGY OF STANDING STONES | Relic | 2 | 8 |
| 7.7 | `choir_voice` | VOICE OF THE CHOIR | Relic | 2 | 8 |
| 7.8 | `counting_mouth` | MOUTH THAT COUNTS | Weapon | 2 | 8 |
| 7.9 | `hunger_wheel` | HUNGER WHEEL | Armor | 2 | 8 |
| 7.10 | `beckoning_ledger` | BECKONING LEDGER | Armor | 2 | 8 |
| 7.11 | `tenth_echo` | TENTH ECHO | Weapon | 2 | 8 |
| 7.12 | `small_gods` | DEBT OF SMALL GODS | Ring | 2 | 8 |

**Verdict: real, batch 1 (highest item priority).** Pack `04`.

### 7b. Generic gear icons

Non-unique items are procedurally *named* (`ASHEN ATHAME`) from
`naming.ron` — 16 adjectives × 4 nouns per slot. You cannot hand-author art for
combinatorial names; you author **per-slot base icons** and let rarity color them.

| # | Asset | PG | RA | Verdict |
|---|---|---|---|---|
| 7.13 | Weapon base icon (ATHAME / SCOURGE / CENSER / BRAND) — 4 variants | 3 | 7 | **Real** — 4 images cover every generated weapon name. |
| 7.14 | Armor base icon (SHROUD / CUIRASS / VESTMENT / PALL) — 4 | 3 | 7 | **Real** — 4 images. |
| 7.15 | Relic base icon (IDOL / LEDGER / PHYLACTERY / SEAL) — 4 | 3 | 7 | **Real** — 4 images. |
| 7.16 | Ring base icon (BAND / SIGNET / COIL / KNOT) — 4 | 3 | 7 | **Real** — 4 images. |
| 7.17 | ~65 affixes | 8 | 1 | **Skip.** Affixes are text lines; they already render with meaningful colors. Never draw these. |

### 7c. Rarity treatment

4 rarities: Common (auto-shatters to dust), Magic, Rare, Goetic.

| # | Asset | PG | RA | Verdict |
|---|---|---|---|---|
| 7.18 | Rarity frames/borders ×4 | 6 | 5 | **Procgen first.** A colored `ui.rect` border is nearly as good and already possible. Real frames are a nice-to-have. |
| 7.19 | Rarity color ramp | 10 | 1 | **Procgen.** Already defined in `Rarity::color()`. Done. |
| 7.20 | Goetic "awakened" overlay (procedurally-named mutants) | 5 | 6 | Real-ish — one shared shimmer overlay covers all awakened items. |

### 7d. Currency & drops

| # | Asset | PG | RA | Verdict |
|---|---|---|---|---|
| 7.21 | **Dust** (the only currency) | 4 | 7 | **Real icon.** Appears in HUD + menus constantly. One image. |
| 7.22 | Loot beam (in-world) | 9 | 2 | **Procgen.** Already an emissive column mesh, rarity-tinted. Working well. |
| 7.23 | Ground drop marker | 8 | 2 | Procgen. |
| 7.24 | Loot pickup burst | 10 | 1 | Procgen — GPU particles. |

---

## 8. Vocabulary icons (Tier B)

### 8a. Damage types — 4

| # | Type | Color | PG | RA | Verdict |
|---|---|---|---|---|---|
| 8.1 | Physical | bone | 4 | 7 | **Real** — tiny, high-reuse. |
| 8.2 | Hellfire | brimstone orange | 4 | 7 | **Real** |
| 8.3 | Hex | occult violet | 4 | 7 | **Real** |
| 8.4 | Void | spectral cyan | 4 | 7 | **Real** |

### 8b. Statuses — 6 (currently colored squares + a stack number)

| # | Status | PG | RA | Verdict |
|---|---|---|---|---|
| 8.5 | `ignite` — burns, spreads on death | 4 | 8 | **Real.** Status icons are read mid-combat under pressure; a colored square is the weakest UI in the game. |
| 8.6 | `hexmark` — amplifies next non-physical hit | 4 | 8 | **Real** |
| 8.7 | `discord` — enemies attack each other | 4 | 8 | **Real** |
| 8.8 | `blight` — stacking DoT, detonates at cap | 4 | 8 | **Real** |
| 8.9 | `petrify` — stone, shatters under physical | 4 | 8 | **Real** |
| 8.10 | `consecrate` — flips heal/harm on ground | 4 | 8 | **Real** |

### 8c. Triggers — 8

| # | Assets | PG | RA | Verdict |
|---|---|---|---|---|
| 8.11 | 8 trigger icons (`on_kill`, `on_crit`, `on_status_apply`, `on_status_detonate`, `nth_cast`, `on_dodge`, `on_loot_pickup`, `on_low_life`) | 5 | 5 | **Defer.** Triggers appear inside reaction text lines, which already read fine. Only worth it if you build a visual build-planner screen. |

---

## 9. Environment, realms, props, VFX

### 9a. Realm rooms — 21 templates (7 per realm), Tier C

| # | Realm | Templates | PG | RA | Verdict |
|---|---|---|---|---|---|
| 9.1 | Vassago | `rotunda`, `stack_ns`, `stack_ew`, `gallery`, `vault`, `junction`, `spiral_wing` | 8 | 2 | **Procgen.** Grammar-assembled; floors+pillars are instanced primitives. |
| 9.2 | Andras | `nave`, `artery_ns`, `artery_ew`, `collapse_arena`, `breach`, `transept`, `pulpit` | 8 | 2 | **Procgen.** |
| 9.3 | Buer | `hub_dais`, `ring_garden`, `stem_ns`, `stem_ew`, `petal`, `arbor`, `bloom_court` | 8 | 2 | **Procgen.** |

### 9b. Props (Tier C)

| # | Prop | PG | RA | Verdict |
|---|---|---|---|---|
| 9.4 | Portal (entry/exit) | 9 | 2 | Procgen — prism + orb + emissive + particles. |
| 9.5 | Vassago shrine | 9 | 2 | Procgen |
| 9.6 | Corruption altar | 9 | 2 | Procgen |
| 9.7 | Court throne/seat ×72 | 9 | 2 | Procgen — instanced; 3 lit, 69 dark. |
| 9.8 | Pillars / columns | 9 | 1 | Procgen — `twisted().tapered().jittered()` is excellent here. |
| 9.9 | Ground / floor planes | 8 | 2 | Procgen |
| 9.10 | Zone discs (ritual circles, telegraphs) | 8 | 4 | Procgen — **but** a real ritual-circle decal texture would look great if Tier B lands. |
| 9.11 | Corpses | 9 | 1 | Procgen — dissolve shader. |
| 9.12 | Loot beam | 9 | 1 | Procgen |

### 9c. VFX — the engine's strongest suit

| # | Effect | PG | RA | Verdict |
|---|---|---|---|---|
| 9.13 | All GPU particles (1M budget) | 10 | 1 | **Procgen. Never replace.** |
| 9.14 | Bloom / emissive glow | 10 | 1 | Procgen |
| 9.15 | Hitstop / screenshake | 10 | 1 | Procgen (engine feel primitives) |
| 9.16 | Dissolve, wobble, vertex motion | 10 | 1 | Procgen |
| 9.17 | Damage numbers | 7 | 4 | Procgen (uses font atlas). Upgrade only if you replace the font. |
| 9.18 | Particle sprite textures (soft dot, spark, smoke) | 7 | 5 | **Optional Tier B.** Currently math-shaded circles; real sprites add subtlety. Low priority. |

---

## 10. Marketing & store — Tier D ⭐ START HERE

Zero engine work. Highest commercial return. Nothing here touches the codebase.

| # | Asset | Spec | PG | RA | Verdict |
|---|---|---|---|---|---|
| 10.1 | **Logo / wordmark "DEMONICON"** | vector + 1024² PNG | 1 | 10 | **Real. Do first.** Everything else derives from it. |
| 10.2 | Steam capsule — main | 616×353 | 1 | 9 | **Real** |
| 10.3 | Steam capsule — small | 462×174 | 1 | 9 | **Real** |
| 10.4 | Steam capsule — header | 460×215 | 1 | 9 | **Real** |
| 10.5 | Steam library hero | 3840×1240 | 1 | 9 | **Real** |
| 10.6 | Steam page background | 1438×810 | 2 | 8 | **Real** |
| 10.7 | Key art (the one big illustration) | 2560×1440 | 1 | 10 | **Real. Highest single-asset value.** |
| 10.8 | App / window icon | 256², 64², 32², 16² | 2 | 9 | **Real** |
| 10.9 | itch.io cover | 630×500 | 1 | 8 | **Real** |
| 10.10 | Social banner (X/Bluesky/Discord) | 1500×500 | 1 | 8 | **Real** |
| 10.11 | Screenshots ×6 | 1920×1080 | 9 | 3 | **Procgen — capture in-game!** The game already looks good; take real screenshots. Free. |
| 10.12 | Trailer (60s) | 1080p | 7 | 6 | Capture gameplay + real title cards. Hybrid. |
| 10.13 | Press kit one-sheet | PDF | 3 | 7 | Real-ish (layout work, not illustration). |
| 10.14 | Three realm splash cards | 1920×1080 ×3 | 2 | 8 | **Real** — reuse the §3.15 seals. |

---

## 11. UI chrome & typography

| # | Asset | Tier | PG | RA | Verdict |
|---|---|---|---|---|---|
| 11.1 | Font — currently hand-authored 5×7 bitmap, uppercase-only | A | 6 | 6 | **Borderline.** It's crisp and on-theme, but uppercase-only and blocky. A real bitmap/MSDF font is the single biggest text-quality win. Both 6 = your call. |
| 11.2 | Panel backgrounds | B | 7 | 3 | **Procgen.** `ui.rect` with alpha already works. |
| 11.3 | Health / boss bars | B | 8 | 2 | **Procgen.** Already good. |
| 11.4 | Skill-slot frames | B | 6 | 5 | Procgen now; real frames if Tier B lands. |
| 11.5 | Sigil socket pips | B | 7 | 3 | Procgen |
| 11.6 | Cursor / crosshair | B | 3 | 7 | **Real.** OS cursor over an iso ARPG is weak; a custom reticle is cheap and improves aim feel. |
| 11.7 | Menu tab chrome | B | 7 | 3 | Procgen |
| 11.8 | Loading screen | B | 3 | 6 | Real-ish — reuse realm splash (§10.14). |

---

## 12. Roadmap / not-yet-in-game

Listed so the index is complete. **Do not generate art for these yet** — the
content doesn't exist.

| # | Thing | Note |
|---|---|---|
| 12.1 | The other 69 demons | Seats are modeled and dark. Each future demon = 1 realm + ~4 enemies + 1 boss + 3 contracts + seal. |
| 12.2 | Professions (demonology, smithing, taming…) | Roadmap in the Shot-2 appendix. |
| 12.3 | NPCs / questgivers | None exist. |
| 12.4 | Pets / mounts / cosmetics | None exist. |
| 12.5 | Achievements icons | Only if you ship to Steam. ~20–30 icons, Tier D. |

---

## 13. Totals

| Category | Real assets worth generating |
|---|---|
| Marketing (§10) | **12** (11 gen + screenshots captured) |
| Skill icons (§4) | 8 |
| Sigil icons (§5) | 20 |
| Contract seals (§6) | 9 |
| Goetic icons (§7a) | 12 |
| Gear base icons (§7b) | 16 |
| Damage + status icons (§8) | 10 |
| Realm seals + boss portraits (§3) | 6 |
| Currency, cursor, misc (§7d, §11) | 3 |
| **TOTAL** | **≈96 images** |

At ~4 images per generation batch, that's ~24 batches — comfortably inside one
weekend of local generation, or a few days of free-tier hopping.

Everything else in this document: **leave procedural.**
