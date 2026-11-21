---
layout: page
title: gaussclip
description: Render-free 3D Gaussian chunk–text alignment for open-vocab retrieval and grounded scene QA
img: assets/img/gaussclip_preview.png
importance: 1
permalink: /projects/gaussclip/
---

<p class="mb-4">
  <a class="btn btn-sm btn-outline-primary me-2" href="https://github.com/kmurungi/3d_scene_understanding" target="_blank" rel="noopener">GitHub Repository</a>
  <a class="btn btn-sm btn-outline-secondary" href="{{ '/assets/img/gaussclip/demo-storyboard.png' | relative_url }}">Demo storyboard</a>
</p>

{% include figure.liquid path="assets/img/gaussclip/demo-storyboard.png" class="img-fluid rounded z-depth-1" %}
<div class="caption">
  Demo overview on <code>frl_apartment_0</code>: retrieve top‑k chunks → pack caption evidence → frozen LLM answers (or abstains without retrieval).
</div>

## Overview

gaussclip turns a 3D Gaussian Splatting scene into a text-queryable index. A small encoder maps native Gaussian chunks into CLIP space; a frozen instruct LLM then answers spatial questions from retrieved caption evidence—without fine-tuning the VLM or baking language features into every splat. The product is a reusable **3D search index** for grounded QA, not another per-scene language field.

## System & Architecture

{% include figure.liquid path="assets/img/gaussclip/pipeline-overview.png" class="img-fluid rounded z-depth-1" %}
<div class="caption">
  Pipeline: Morton chunks → offline captions → contrastive <code>f_3D</code> → embedding bank → text retrieve → frozen LLM.
</div>

1. **Chunk** each 3DGS scene with Morton-ordered spatial boxes.
2. **Train** a small transformer `f_3D` into 512‑d CLIP space against a frozen CLIP text tower (InfoNCE). Labels come from offline full-scene crops + captioning—not from the encoder at query time.
3. **Index** every chunk into a scene embedding bank.
4. **Retrieve** top‑k by embedding the question with the same CLIP text tower.
5. **Answer** with a frozen instruct LLM on question + caption evidence only (no VLM fine-tuning).

{% include figure.liquid path="assets/img/gaussclip/inference-paths.png" class="img-fluid rounded z-depth-1" %}
<div class="caption">
  Inference paths: retrieve → evidence → frozen model vs. question alone (ablation).
</div>

## Benchmarks & Results

Chunk ↔ text retrieval on held-out Replica MCMC chunks (**N = 507**; chance R@1 = 1/507 ≈ **0.2%**):

| Pool | N | R@1 | R@3 | Chance R@1 |
|---|---:|---:|---:|---:|
| Full validation | 507 | ~5–6% | ~13% | ~0.2% |
| Small qualitative pool | ~56 | ~23–25% | higher | ~2% |

Read R@K **vs chance**. When the right caption lands in the evidence pack, frozen LLM QA succeeds on spatial questions it otherwise refuses.

{% include figure.liquid path="assets/img/gaussclip/qa-bicycle.png" class="img-fluid rounded z-depth-1" %}
<div class="caption">
  <em>Is there a bicycle near the stairs?</em> — with retrieval: yes; without: abstains.
</div>

{% include figure.liquid path="assets/img/gaussclip/qa-vase.png" class="img-fluid rounded z-depth-1" %}
<div class="caption">
  <em>What is on the wooden table?</em> — with retrieval: blue vase; without: abstains.
</div>

{% include figure.liquid path="assets/img/gaussclip/qa-plant.png" class="img-fluid rounded z-depth-1" %}
<div class="caption">
  <em>Where are the potted plants?</em> — with retrieval: stairwell / white door; without: abstains.
</div>

## Engineering Notes & Next Steps

- **Caption noise** — wrong top‑k → wrong answers; evidence is text-only today.
- **Not SOTA** vs LangSplat / SceneSplat language-field benchmarks—that is not the claim.
- **Encoder is search**, not a 3D VLM vision backbone.
- **Next:** pack gsplat crops into a multimodal VLM alongside captions so answers can use vision as well as text.
