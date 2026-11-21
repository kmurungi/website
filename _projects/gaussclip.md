---
layout: page
title: GaussCLIP
description: Searchable native 3D Gaussian chunks → grounded QA from a frozen LLM and retrieved caption evidence (no VLM fine-tuning).
img: assets/img/gaussclip_preview.png
importance: 1
permalink: /projects/gaussclip/
---

## Overview

GaussCLIP indexes a 3D Gaussian Splat scene as **searchable native chunks**. A text question retrieves the relevant regions; a **frozen instruct LLM** answers from the retrieved caption evidence—or abstains when that evidence is withheld. There is **no VLM fine-tuning** and no baking language into every splat. Demos and metrics use **Replica MCMC** indoor 3DGS scenes from SceneSplat-7K / GaussianWorld.


<p class="text-muted mb-3"><strong>Scene:</strong> <code>frl_apartment_0</code> (Replica)</p>

{% include figure.liquid path="assets/img/gaussclip/qa-bicycle.png" class="img-fluid rounded z-depth-1 mb-4" %}

**Q:** *Is there a bicycle near the stairs?*

**Evidence (top retrieved captions):**

- *Bicycle near stairs:* blue bicycle parked near the wooden stairs on the light wood flooring.
- *Plant and stairs area:* potted plant near stairs and wood-paneled walls in living space.

**With retrieval:** **Yes** — blue bicycle parked near the wooden stairs on the light wood flooring.

**Without retrieval:** Abstains — not certain from evidence whether a bicycle is near the stairs.

<div class="row mt-4">
  <div class="col-sm-6 mb-3">
    {% include figure.liquid path="assets/img/gaussclip/qa-vase.png" class="img-fluid rounded z-depth-1" %}
    <div class="caption">
      <em>What is on the wooden table?</em> — with retrieval: two blue lamps; without: abstains.
    </div>
  </div>
  <div class="col-sm-6 mb-3">
    {% include figure.liquid path="assets/img/gaussclip/qa-plant.png" class="img-fluid rounded z-depth-1" %}
    <div class="caption">
      <em>Where are the potted plants?</em> — with retrieval: stairwell, bottom near the white door; without: abstains.
    </div>
  </div>
</div>

## Results

Chunk ↔ text retrieval on held-out Replica MCMC chunks (**N = 507**; chance R@1 = 1/507 ≈ **0.2%**):

| Pool | N | R@1 | R@3 | Chance R@1 |
|---|---:|---:|---:|---:|
| Full validation | 507 | ~5–6% | ~13% | ~0.2% |
| Small qualitative pool | ~56 | ~23–25% | higher | ~2% |

On held-out **Replica MCMC** chunks, R@1 and R@3 sit clearly above chance. When the right caption reaches the evidence pack, the frozen LLM answers spatial questions it otherwise refuses.

## Notes

- Wrong top‑k retrieval can still yield confident wrong answers (text evidence only today).
- **Next:** add gsplat **crops** to a multimodal VLM so answers can use vision as well as captions.

