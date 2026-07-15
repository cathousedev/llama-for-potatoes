# llama.cpp Settings for Hardware That Has No Business Running LLMs

A lot of local AI posts assume you're running multiple 4090s. This is for the rest of us.

These are my working `llama-server` presets, tuned and tested on a 4 year old business laptop with no discrete GPU, just Vulkan on an integrated Intel iGPU. If you've got similar specs and assumed local LLMs weren't realistic for you, hopefully this proves otherwise.

## Hardware

- **Laptop:** Lenovo ThinkPad T14 Gen 2
- **CPU:** Intel Core i5-1145G7
- **RAM:** 15GB
- **GPU:** Intel Xe (integrated), via Vulkan backend
- **OS:** CachyOS

## Usage

This file only contains *settings*, so you'll need to download the actual GGUF model (and draft model, where listed) files yourself first. Each section has a Hugging Face link in a comment above it pointing to the model source. Update the `model` (and `model-draft`) paths in the ini to match wherever you save them on your own machine.

Once you've got the files in place, save `models.ini` somewhere, then run:

```bash
llama-server --models-preset /path/to/models.ini
```

Each `[Section]` is a selectable preset, pick whichever model fits your task and hit go.

## Favorites / Benchmarks

| Model | Speed | Notes |
|---|---|---|
| LFM 2.5 8B-A1B | ~12.8 tok/s | Fastest of the bunch |
| Qwen 2.5 Coder 1.5B | ~12.5 tok/s | Code completion |
| Granite 4.0 Tiny (7B-A1B) | ~11.7 tok/s | Long context (20k) |
| Mellum 2 12B-A2B | ~7.7 tok/s | ~2.5B active params (MoE), great for coding |
| Gemma 4 E2B | ~8.0 tok/s | |
| Gemma 4 E4B | ~6.8 tok/s | Solid general-purpose |
| Qwen 3.5 4B | ~5.6 tok/s | Built-in MTP draft head |

## What's included

- **Gemma 4 E2B / E4B**: with MTP speculative decoding drafts for a speed boost
- **Granite 4.0 Tiny (7B-A1B)**: long-context option (20k ctx)
- **Mellum 2 12B-A2B**: only ~2.5B active parameters (MoE), and great for coding tasks
- **LFM 2.5 8B-A1B**: fastest of the bunch
- **Qwen 3.5 4B**: uses its built-in MTP draft head for speculative decoding (no separate draft model needed)
- **Qwen 2.5 Coder 1.5B**: small, fast, tuned for code completion (low temp, short context)

## A couple of notes if you're adapting this for your own machine

- `reasoning-budget` + `reasoning-budget-message` cap how long a model "thinks" before forcing an answer, handy on slower hardware where unbounded reasoning can eat your whole context.
- Model paths are hardcoded to my filesystem (`/home/ava/LLMs/...`), swap these for wherever you keep your GGUFs.
- `ctx-size` is scaled per-model based on what my RAM can actually handle at that model's size, adjust down if you have less than 15GB.

## Feedback

Suggestions welcome, open an issue/PR or email me at ava@cathousedev.com.
