# DEMONICON — Asset Production Kit

Everything you need to produce art for this game, and an honest account of what
the game can currently *use*.

**Read `ENGINE_REALITY.md` before generating a single image.** It will save you
from producing 200 beautiful PNGs the game physically cannot load.

## Files

| File | What it's for |
|---|---|
| **`ENGINE_REALITY.md`** | What the engine can display today, and the exact work needed to unlock each asset class. Read first. |
| **`ASSET_INDEX.md`** | The master matrix. Every entity in the game that could need art, with a Procgen score and a Real-Asset score. |
| **`PLATFORMS.md`** | Every free model, tool, and platform for generating images / 3D / audio. Local-first (your RX 6800 XT is the hero here). |
| **`GENERATION_GUIDE.md`** | The literal stepwise "do exactly this" walkthrough for going all-real-assets. |
| **`prompts/`** | Copy-paste prompt packs, one file per batch. Style block is pre-applied. |
| **`incoming/`** | Drop raw downloads here. Gitignored — nothing here is authoritative. |
| **`engine-ready/`** | Final, processed, correctly-sized files the game actually loads. |

## The 30-second version

- The game's art direction is **procedural by design**: primitives + flat shading
  + emissive + bloom + GPU particles. That is not a placeholder. It is the look.
- The engine can load **no image files at all** right now. Zero image decoders
  are compiled in.
- Therefore the highest-value real assets are the ones that **live outside the
  engine**: logo, Steam capsule, key art, icon, trailer. Those need no engine
  work and are what actually sell the game.
- The second-highest value is **2D UI icons** (skills, sigils, statuses, items),
  which need one small, contained engine addition — the UI pipeline already
  samples a texture, it just needs RGBA + an atlas loader.
- In-world 3D texturing is the **worst** investment: it needs UVs, a material
  system, and it actively fights the art direction that already works.

## Scoring rules used in `ASSET_INDEX.md`

Two independent 1–10 scores per row:

- **PROCGEN (1–10)** — how good this can look generated//code-driven. 10 = procgen
  is genuinely the right answer and looks great. 1 = procgen output would be sad.
- **REAL (1–10)** — how important it is to abandon procgen and commission/generate
  a real handmade asset. 10 = ship-blocking, must be real. 1 = never bother.

**Invariant: no row has both scores ≥7.** If procgen nails it, a real asset is by
definition not important; if a real asset is essential, procgen by definition
can't cut it. Rows where *both* are low are legitimate — that means the thing
barely matters either way (`Verdict: skip`).

## Working rule

Generate nothing until you've read the **Verdict** column for that row. Most of
this game should stay procedural. The list of things that genuinely want a real
asset is about 40 items long, and 12 of them are marketing.
