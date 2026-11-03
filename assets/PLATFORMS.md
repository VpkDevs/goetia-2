# Free Generation Platforms & Models

⚠️ **Free-tier amounts change constantly.** My knowledge cuts off January 2026.
Treat every credit number below as "check on arrival," not gospel. The *tools*
and *licenses* are stable; the *quotas* are not.

---

## 0. The headline: your GPU makes this free and unlimited

You have an **AMD Radeon RX 6800 XT (16 GB VRAM)**. That is comfortably enough to
run SDXL and FLUX locally at full quality. Local generation is:

- free forever, no credits, no queue, no rate limit
- private, offline, reproducible (fixed seeds → identical images)
- unrestricted on batch size — you can render all ~96 assets overnight

**AMD-on-Windows caveat:** most tutorials assume NVIDIA/CUDA. Your three good
paths, easiest first:

| Path | Effort | Notes |
|---|---|---|
| **Amuse AI** | lowest | AMD's own free local gen app. Installer, GUI, built for Radeon. Best starting point. |
| **ComfyUI-ZLUDA** | medium | Full ComfyUI power, CUDA-translation layer for AMD. Best quality/control ceiling. |
| **SD.Next / A1111 + DirectML** | medium | Works, generally slower than ZLUDA. |
| WSL2 + ROCm | high | Closest to native Linux perf; fiddly setup. |

---

## 1. Model licenses — read this before you sell anything

This game is **public and MIT/Apache licensed**, and you may sell it. Model
license matters.

| Model | License | Commercial use? |
|---|---|---|
| **FLUX.1 [schnell]** | Apache 2.0 | ✅ **Yes — safest high-quality option** |
| FLUX.1 [dev] | FLUX.1-dev Non-Commercial | ❌ **No.** Great quality, but you cannot ship its output in a paid game. |
| **SDXL 1.0** | CreativeML OpenRAIL++-M | ✅ Yes |
| SD 1.5 | CreativeML OpenRAIL-M | ✅ Yes |
| SD 3.5 | Stability Community License | ✅ Free under revenue threshold — verify current terms |
| **Adobe Firefly** | Adobe (licensed training data) | ✅ Yes — designed to be indemnified/commercially safe |
| DALL·E 3 (via Bing) | OpenAI terms | ✅ Generally yes — verify current terms |
| SDXL community finetunes (Juggernaut, DreamShaper…) | usually OpenRAIL++ | ✅ Usually — **check each model card** |

**Recommendation:** use **FLUX.1-schnell** or **SDXL** for anything shipping.
Keep FLUX.1-dev for internal mockups only, if at all.

---

## 2. Local software (free, unlimited)

| Tool | Use |
|---|---|
| **Amuse AI** | Easiest AMD-native local image gen. Start here. |
| **ComfyUI** (+ ZLUDA for AMD) | Node graph; best control, batch, upscale pipelines |
| **Fooocus** | Midjourney-like simplicity on SDXL |
| **InvokeAI** | Polished UI, good inpainting/canvas |
| **SD.Next / AUTOMATIC1111** | The classic web UIs |
| **Krita + AI Diffusion plugin** | Paint *and* generate in one app — ideal for icon touch-up |

## 3. Cloud free tiers (verify quotas on arrival)

| Platform | Strength | Why you'd use it |
|---|---|---|
| **Ideogram** | **Best text rendering** | The one to use for the DEMONICON wordmark |
| **Recraft** | **Vector/SVG output** | Logos and icons as true vectors — scalable, tiny |
| **Adobe Firefly** | Commercially safe | Marketing art you want zero license doubt on |
| **Bing Image Creator** | DALL·E 3, free daily | Strong prompt adherence for key art |
| **Google AI Studio** | Free tier, Gemini image models | Fast iteration |
| **Leonardo.ai** | Daily free credits, game-art presets | Icon sets, has dedicated game-asset modes |
| **Krea.ai** | Realtime canvas | Fast style exploration |
| **Playground** | Daily free images | Bulk drafts |
| **Civitai** | Model hub + on-site gen | Finding a finetune that matches your style |
| **Hugging Face Spaces** | Free demos of nearly every model | Trying FLUX/SD3.5 before installing |
| **Tensor.art / SeaArt / Mage.space** | More free-tier generators | Overflow when other quotas run dry |

## 4. 3D generation (you almost certainly don't need this)

Your engine has **no model importer and no UVs** — generated 3D models cannot be
loaded. Listed only for completeness / future use:

| Tool | Note |
|---|---|
| **Blender** | Free, and you have a Blender MCP connected. Could *export procedural meshes* for reference. |
| Meshy / Tripo3D / Rodin | Text/image→3D, free tiers |
| **TRELLIS / Hunyuan3D** | Open-source, run locally |

**If you ever want 3D in this engine**, the realistic path is not generated
models — it's extending `MeshBuilder` with more primitives, which is free.

## 5. Audio (game currently synthesizes everything at runtime)

| Tool | Use |
|---|---|
| **sfxr / Bfxr / ChipTone** | Free forever, instant retro SFX — perfect fit for this game |
| **Freesound.org** | Huge CC-licensed library (check each license) |
| **ElevenLabs SFX** | Free tier, text→sound effect |
| **Suno / Udio** | Free tiers, music generation — check commercial terms carefully |
| **Audacity / LMMS / Cakewalk** | Free editing/DAW |

## 6. Post-processing & pipeline (all free)

| Tool | Use |
|---|---|
| **Krita / GIMP** | Raster editing, alpha cleanup |
| **Inkscape** | Vector — logo finalization |
| **rembg** | One-command background removal (essential for icons) |
| **Upscayl / Real-ESRGAN** | Free upscaling |
| **IOPaint (LaMa)** | Remove artifacts/watermarks |
| **ImageMagick** | Batch resize/convert/atlas assembly from the CLI |
| **free-tex-packer** | Sprite-atlas packing (you'll need this for Tier B) |
| **msdf-atlas-gen / BMFont / fontbm** | Bitmap/MSDF font atlas generation |
| **Aseprite** (paid, or free via **LibreSprite**) | Pixel-perfect icon work |

## 7. Recommended stack for *this* project

```
Local:   Amuse AI (or ComfyUI-ZLUDA)  +  FLUX.1-schnell / SDXL
Text:    Ideogram          → the DEMONICON wordmark
Vector:  Recraft           → logo + app icon as SVG
Cleanup: rembg → Krita → ImageMagick
Pack:    free-tex-packer   → engine-ready/icons.png + .json
Audio:   Bfxr (SFX), keep the runtime synth for now
```

Total cost: **$0**. Total quota anxiety: **none**, because the bulk runs locally.
