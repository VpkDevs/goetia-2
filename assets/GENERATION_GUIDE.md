# GENERATION GUIDE — "I want all real assets"

Literal, stepwise. Two tracks: **A (local, recommended)** and **B (cloud free
tiers)**. Do Track A if you're willing to install one program — it's free,
unlimited, and you own the whole pipeline.

⚠️ **Credit amounts and free-tier rules change constantly and my information has
a January 2026 cutoff.** Where I say "you get N credits," verify on arrival. The
*tools*, *URLs*, and *licenses* are stable; the *quotas* are not.

⚠️ **Before generating anything for a paid release**, read the license table in
`PLATFORMS.md`. Short version: **FLUX.1-schnell and SDXL are safe to sell.
FLUX.1-dev is NOT.**

---

# TRACK A — Local generation (free, unlimited, recommended)

Your **RX 6800 XT (16 GB)** runs SDXL and FLUX comfortably.

### Step 1 — Install a local generator

**Easiest (AMD-native, GUI):**
1. Go to `https://www.amuse-ai.com/`
2. Download the Windows installer. It is free and built by/for AMD.
3. Install, launch. It downloads models on first run.
4. In model selection, pick **FLUX.1-schnell** (Apache 2.0 — safe to sell) or
   an **SDXL** checkpoint.

**More powerful (node graph, best control):**
1. Go to `https://github.com/patientx/ComfyUI-Zluda`
2. Follow its README exactly — it is the maintained AMD/ZLUDA fork of ComfyUI.
3. Install ZLUDA per its instructions, then run `comfyui.bat`.
4. Download a checkpoint from `https://civitai.com` (filter: SDXL, license
   "commercial use allowed") into `ComfyUI/models/checkpoints/`.

### Step 2 — Configure once, reuse for every batch

| Setting | Value | Why |
|---|---|---|
| Resolution | **1024×1024** | SDXL/FLUX native; downscale later |
| Steps | 30 (SDXL) / 4 (FLUX-schnell) | schnell is a 4-step model — don't over-step it |
| CFG | 6–7 (SDXL) / 1.0 (FLUX-schnell) | schnell ignores high CFG |
| Sampler | DPM++ 2M Karras | good default |
| **Seed** | **fix it, write it down** | Same seed + same style block = a consistent family |
| Batch count | 4 | generate 4 per prompt, keep the best |

**The single most important habit:** put the `[ICON]` style block from
`prompts/00-STYLE.txt` in **every** prompt, byte-identical, and keep the seed
fixed within a batch. That is what makes 96 images look like one game instead of
96 unrelated pictures.

### Step 3 — Generate, in this order

Work through the packs in priority order. Each line in a pack = one prompt.

| Order | Pack | Count | Why this order |
|---|---|---|---|
| 1 | `08-marketing.txt` | 12 | Zero engine work; highest return; start here |
| 2 | `01-skills.txt` | 8 | Biggest in-game readability win |
| 3 | `03-statuses-damage.txt` | 10 | Read under combat pressure |
| 4 | `04-goetics.txt` | 12 | The game's trophies |
| 5 | `06-portraits-seals.txt` | 6 | High reuse |
| 6 | `07-contracts.txt` | 9 | Thematic payoff |
| 7 | `02-sigils.txt` | 20 | Cheap as contact sheets |
| 8 | `05-gear-currency.txt` | 17 | Finishes the set |

For each prompt: paste prompt + `[ICON]` block → paste `[ICON NEGATIVE]` into the
negative field → generate 4 → save the best into `assets/incoming/`.

### Step 4 — The wordmark is a special case

Diffusion models are bad at text. **Do not** generate the DEMONICON logo locally.
1. Go to `https://ideogram.ai` — sign up free. Ideogram is the best free model
   for rendering legible text.
2. Use prompt #1 from `08-marketing.txt`. Generate several.
3. For a **scalable vector** version: go to `https://www.recraft.ai`, sign up
   free, use its vector/SVG mode with the same prompt. Recraft outputs true SVG.
4. Or vectorize the raster yourself: Inkscape → `Path ▸ Trace Bitmap` → save SVG.

---

# TRACK B — Cloud free tiers (no install)

Rotate across platforms as quotas run out. All have free tiers; **verify current
limits when you arrive.**

| # | Platform | URL | Best used for |
|---|---|---|---|
| 1 | **Ideogram** | `ideogram.ai` | The wordmark and anything with text |
| 2 | **Recraft** | `recraft.ai` | Logo/icons as **vector SVG** |
| 3 | **Bing Image Creator** | `bing.com/images/create` | DALL·E 3 quality key art; free daily boosts |
| 4 | **Adobe Firefly** | `firefly.adobe.com` | Commercially-safe marketing art; free monthly credits |
| 5 | **Leonardo.ai** | `leonardo.ai` | Game-asset presets, icon sets; free daily tokens |
| 6 | **Google AI Studio** | `aistudio.google.com` | Fast iteration, free tier |
| 7 | **Krea** | `krea.ai` | Realtime style exploration |
| 8 | **Playground** | `playground.com` | Bulk drafts |
| 9 | **Civitai** | `civitai.com` | On-site generation + finding SDXL finetunes |
| 10 | **Hugging Face Spaces** | `huggingface.co/spaces` | Free FLUX/SD3.5 demos, no signup for many |

**Suggested rotation:** Ideogram → wordmark. Firefly/Bing → the 12 marketing
pieces. Leonardo → the 55 icons (it has icon-oriented presets). Civitai/HF →
overflow.

---

# POST-PROCESSING (both tracks)

### Step 5 — Strip backgrounds

Icons must be transparent PNGs, not black squares.

```bash
pip install rembg[cli]
cd assets/incoming
for f in *.png; do rembg i "$f" "../engine-ready/icons/$f"; done
```

Touch up leftover fringing in **Krita** (free) — pick the eraser, zoom to 400%.

### Step 6 — Slice contact sheets (if you generated 4-up sigil sheets)

```bash
magick sigils_sheet.png -crop 2x2@ +repage sigil_%d.png
```

### Step 7 — Resize to spec

```bash
cd assets/engine-ready/icons
# skills, goetics, gear, contracts -> 128px
magick mogrify -resize 128x128 -filter Lanczos skill_*.png goetic_*.png gear_*.png contract_*.png
# statuses and damage types -> 64px (they render tiny)
magick mogrify -resize 64x64 -filter Lanczos status_*.png dmg_*.png
# sigils -> 96px
magick mogrify -resize 96x96 -filter Lanczos sigil_*.png
```

**Squint test:** shrink each icon to 48 px and look at it. If you can't tell what
it is, regenerate with a bolder silhouette. This is the #1 failure mode.

### Step 8 — Pack an atlas

The engine draws UI from a single texture, so pack everything into one sheet.

1. Go to `https://free-tex-packer.com` (free, also downloadable/offline).
2. Drag in `assets/engine-ready/icons/*.png`.
3. Settings: **format JSON (hash)**, power-of-two **ON**, size **2048×2048**,
   padding **2**, allow trim **OFF** (trimming breaks fixed-slot UI math).
4. Export → `assets/engine-ready/icons.png` + `icons.json`.

### Step 9 — Screenshots: capture, don't generate

Do **not** AI-generate screenshots — it's dishonest marketing and the real game
already looks good.

```powershell
$env:PATH = "$HOME\scoop\apps\mingw\current\bin;$env:PATH"
cd C:\Users\MQ420_OL\DEV\goetia2
cargo run -p demonicon --release -- --demon 0 --tier 3 --seed 31337
```
Press **F1** to hide the debug overlay first. Capture with Win+Shift+S or
ShareX (free). Get 6: one per realm, one boss fight, one loaded inventory, one
big proc-chain explosion.

---

# THE BLOCKER: making the engine actually load these

**Everything above produces files the game currently cannot open.** See
`ENGINE_REALITY.md`. To use the icons you need one contained change:

### Step 10 — Tier B engine change (do this deliberately)

1. **Log it as a contract revision first.** `crates/` is frozen per
   `API_CONTRACT.md`; `AGENTS.md` says a genuine gap gets recorded in
   `demonicon/BUILD_NOTES.md`, not patched casually. Write the entry, then do it.

2. Add a PNG decoder to `crates/goetia_render/Cargo.toml`:
   ```toml
   image = { version = "0.25", default-features = false, features = ["png"] }
   ```

3. In `crates/goetia_render/src/ui.rs`:
   - change the atlas texture format `R8Unorm` → `Rgba8UnormSrgb`
   - add a loader that reads `icons.png` from disk beside the existing
     `build_atlas()` font path (keep the font atlas — add a second binding)
   - add `UiBatch::icon(pos, size, atlas_uv, tint)` mirroring the existing
     `rect`/`text` calls

4. Expose it through the facade and document it in `API_CONTRACT.md` (bump the
   version — this is a real interface change).

5. Load `icons.json` in `demonicon` to map `skill_hellbolt` → UV rect, and draw
   icons in the skill bar instead of truncated text.

6. Verify:
   ```powershell
   cargo fmt --all --check
   cargo clippy --workspace --all-targets -- -D warnings
   cargo test --workspace
   ```
   Then actually play it — `/playtest` — and confirm icons read at a glance
   during a fight.

---

# Total time & cost

| Phase | Time | Cost |
|---|---|---|
| Install local generator | 30–60 min once | $0 |
| Generate 12 marketing assets | 2–3 h | $0 |
| Generate ~84 icons | 4–6 h | $0 |
| Background-strip, resize, pack | 1–2 h | $0 |
| Tier B engine change | 3–5 h | $0 |
| **Total** | **~1 weekend** | **$0** |

# If you only have one hour

Do the wordmark (Ideogram) + key art (Bing/Firefly) + capture 6 real
screenshots. That's a store page. Everything else can wait.
