<h1 align="center">Inline Research</h1>

<h3 align="center">Version control for AI filmmaking</h3>

<p align="center">
We build <strong>Inline Studio</strong>, a free and open source canvas for AI filmmaking where every
render is kept as a versioned take, so you never lose the good version.
</p>

<p align="center">
  <a href="https://inlinestudio.art"><img alt="Website" src="https://img.shields.io/badge/Website-inlinestudio.art-111111?style=for-the-badge"></a>
  <a href="https://discord.gg/cSUS88VdY9"><img alt="Discord" src="https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white&style=for-the-badge"></a>
  <a href="https://huggingface.co/inlineresearch"><img alt="Hugging Face" src="https://img.shields.io/badge/Hugging%20Face-inlineresearch-FFD21E?style=for-the-badge"></a>
  <a href="https://civitai.com/user/inlineresearch"><img alt="Civitai" src="https://img.shields.io/badge/Civitai-inlineresearch-1971C2?style=for-the-badge"></a>
  <a href="https://www.gnu.org/licenses/gpl-3.0.html"><img alt="License GPL-3.0" src="https://img.shields.io/badge/License-GPLv3-blue?style=for-the-badge"></a>
</p>

---

## The problem we work on

You generate forty variations of a shot. One of them is perfect. Three hours later you can't find it,
can't remember the settings, and can't reproduce it. So you re-roll and settle for something worse.

Inline Studio fixes that at the data model. A frame isn't a file, it's a slot with a history.
Generating again adds a take, it never overwrites. You pick a hero take and it flows downstream to
everything wired after it. You build the whole film on one board, moodboard through final cut.

> "This is the part of open source you can't fake. Someone wanted a tool that didn't exist, built it
> on us, and gave it to everyone."
>
> Robin Huang, Cofounder, ComfyUI ([original post](https://www.linkedin.com/posts/robinjhuang_someone-built-version-control-for-ai-filmmaking-activity-7475278781914681344-ut9E/))

## Repositories

| Repo | What it is |
| --- | --- |
| [Inline-Studio](https://github.com/inlineresearch/Inline-Studio) | The app. Node canvas, take history, timeline, LoRA trainer, and the Inline Core engine that powers it. Start here. |
| [Inline-Registry](https://github.com/inlineresearch/Inline-Registry) | The published extension index the app's Available tab reads. |
| [Inline-Studio-Extension-Guide](https://github.com/inlineresearch/Inline-Studio-Extension-Guide) | The reference extension, for anyone writing custom nodes. |

Releases and changelog: [Inline-Studio/releases](https://github.com/inlineresearch/Inline-Studio/releases).
Packages: [inline-core](https://pypi.org/project/inline-core/) and
[inline-studio-frontend](https://pypi.org/project/inline-studio-frontend/) on PyPI.

## Will it run on your machine?

Worth answering before you install anything.

| Your hardware | What you get |
| --- | --- |
| No GPU at all | API Nodes reach hosted models. Nothing to download, works today. |
| 8 to 16 GB | Z-Image Turbo runs locally. 1024px fits in roughly 11.5 GB at Guidance 0. |
| 24 GB | FLUX.2 klein 4B stays resident at bf16, and quantized builds go further. |
| 40 GB and up | Krea 2 at full quality, and LoRA training at 1024px. |

Training is cheaper than generating, so a Krea 2 LoRA trains at 512px inside 12 GB on a card that
can't generate with the model at all. Full numbers are in the
[app README](https://github.com/inlineresearch/Inline-Studio#benchmark-results).

## Train your own LoRA

Training is part of the app, not a separate toolchain you go and learn. The **Trainer** is a second
canvas: a dataset node, a Train LoRA node, a live loss graph and a resources readout, wired together.
Press Start and watch it run. Hyperparameters sit in a side panel so the node itself stays a status
surface, with a step counter, the trainer's streaming logs, and a progress bar.

![The Inline Studio Trainer canvas, with a dataset node, a Train LoRA node running, live logs and a loss curve](https://raw.githubusercontent.com/inlineresearch/Inline-Studio/main/screenshots/lora-trainer.png)

- **Three architectures.** Z-Image, Krea 2 and FLUX.2 klein. You train on the undistilled base, then
  generate with the fast distilled checkpoint, and the LoRA carries over unchanged so you keep the
  8-step render speed. If you only hold a Turbo checkpoint, a training adapter is fused in for the
  run and dropped when the LoRA is saved.
- **It fits a 16 GB card.** Z-Image trains at 512px in about 13 GB and at 1024px in about 15 GB.
  Krea 2 with a 4-bit frozen base trains at 512px in about 12 GB, measured on a Tesla T4. A LoRA
  trained at 512px still applies at any generation resolution.
- **Captions are optional.** Caption the dataset locally with the built-in captioner, edit captions
  by hand, or switch them off and rely on the trigger word, which works well for a single subject.
- **Stop and resume.** Stopping flushes a checkpoint holding the adapter weights, optimizer and RNG
  state, and step count, so resuming picks up at the exact step. Runs interrupted by a crash recover
  on their own.
- **Straight into a render.** The finished `.safetensors` lands in `models/loras/` and appears in the
  LoRA loader node right away, so you can wire it into a generate node and try it without leaving the
  app.

Nothing is downloaded behind your back. Training reuses the model files you already have, and if one
is missing the run stops and names it.

[Full walkthrough](https://inlinestudio.art/lora-training), with a worked example, measured VRAM
numbers, and the settings we'd start from.

## Models and datasets

Trained on that canvas, published openly, dataset included so you can reproduce the run.

- [skin-lora-krea-2-raw](https://huggingface.co/inlineresearch/skin-lora-krea-2-raw), a skin texture
  LoRA for Krea 2 RAW
- [krea2-skin-lora](https://huggingface.co/datasets/inlineresearch/krea2-skin-lora), the 26 image and
  caption pairs it trained on
- The same LoRA on Civitai:
  [Krea 2 Realistic Skin Texture](https://civitai.com/models/2808600/krea-2-realistic-skin-texture)

Everything else we publish lands on [Hugging Face](https://huggingface.co/inlineresearch) and
[Civitai](https://civitai.com/user/inlineresearch).

## Get started

- [Getting started guide](https://inlinestudio.art/getting-started), install to first render
- [Train your own LoRA](https://inlinestudio.art/lora-training)
- [Projects](https://inlinestudio.art/projects), films made in the app
- [Discord](https://discord.gg/cSUS88VdY9), where we answer questions and take feature requests

## Contact

[team@inlinestudio.art](mailto:team@inlinestudio.art) ·
[Privacy policy](https://inlinestudio.art/privacy)

Inline Studio is licensed [GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html).
Copyright Aesthisia Datacenters Private Limited.
