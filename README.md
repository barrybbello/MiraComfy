# 🪞 MiraComfy

> ComfyUI on Google Colab — modernized for the **free tier** (Tesla T4 · 15 GB VRAM).

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/barrybbello/MiraComfy/blob/main/ComfyUIonColab.ipynb)

One notebook: mount Drive → install ComfyUI → download models → get a public URL.

## Quick start

1. Open [`ComfyUIonColab.ipynb`](ComfyUIonColab.ipynb) in Colab (badge above).
2. **Runtime ▸ Change runtime type ▸ Hardware accelerator: T4 GPU** (free tier).
3. Run the cells top-to-bottom. The **Start** cell prints a `https://…trycloudflare.com` URL — that's your ComfyUI.

## Secrets (optional)

| Secret | Needed for | Where |
|---|---|---|
| `CIVITAI_API_TOKEN` | civitai.com downloads (most now require login) | Colab sidebar 🔑 → *Secrets* → enable notebook access |
| `HF_TOKEN` | gated Hugging Face models | same |

Defaults use verified, anonymously-downloadable links, so **no secrets are required** for the default setup.

## What the 2026 refresh changed

- **No more CUDA wheel roulette.** Colab ships a CUDA-enabled PyTorch (Python 3.12, torch 2.8+); Setup reuses it instead of pulling 2023-era `cu117/cu118/cu121` extra indexes that downgraded or broke torch. `xformers` is gone too — modern ComfyUI uses PyTorch SDPA attention, which performs well on T4.
- **Upstream URLs updated.** ComfyUI → [`Comfy-Org/ComfyUI`](https://github.com/Comfy-Org/ComfyUI), ComfyUI-Manager → [`Comfy-Org/ComfyUI-Manager`](https://github.com/Comfy-Org/ComfyUI-Manager) (old URLs redirect, but clones now target the canonical repos).
- **Dead model links fixed.** `runwayml/stable-diffusion-v1-5` was taken down in 2024 — defaults now point to the official `Comfy-Org` SD 1.5 archive (fp16, 2.1 GB), with optional SDXL base and VAEs, all verified anonymous downloads.
- **Hybrid persistence.** The app installs to fast local disk while `models/` and `output/` are symlinked to Google Drive — multi-GB checkpoints and generated images survive runtime recycles without the pain of running the whole app from Drive.
- **One smart launcher.** Cloudflare quick tunnel (recommended) or localtunnel fallback, double-start protection, and a stop/restart cell.
- **Bug fixes from the old notebook:** secrets lookup no longer crashes when unset; Civitai tokens append correctly (`?` vs `&`); custom-node installs respect the actual workspace (were hardcoded to `/content`); the broken clone-or-update check (`[ ! -d WORKSPACE ]`) replaced with real logic; resumable downloads that don't redownload completed files.

## Free-tier notes

- T4: SD 1.5 is quick; SDXL runs well (~1 min per 1024² image); bigger architectures only make sense quantized (GGUF/FP8).
- Idle GPU runtimes get recycled — everything outside Drive is wiped, which is why models/outputs default to Drive persistence.
- The tunnel URL is public while the server runs and changes on every restart. Don't share it.
- Full troubleshooting table is in the notebook's last cell.
