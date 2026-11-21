---
layout: page
title: Custom 3D Gaussian Splatting (CUDA)
description: Bare-metal C++/CUDA differentiable 3DGS engine achieving 114 FPS @ 720p with custom tile rasterization and EWA kernels
img: assets/img/cuda-3dgs/horse_gt_vs_render.jpg
importance: 2
permalink: /projects/cuda-3dgs/
---



Ground truth (left) vs. differentiable CUDA render (right) on held-out views — **100k** Gaussians, degree‑3 SH, custom tile rasterizer.

<div class="row mt-2">
  <div class="col-sm-6 mb-3">
    {% include figure.liquid path="assets/img/cuda-3dgs/horse_gt_vs_render.jpg" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Test view — full frame (see evaluation table below).</div>
  </div>
  <div class="col-sm-6 mb-3">
    {% include figure.liquid path="assets/img/cuda-3dgs/horse_gt_vs_render_2.jpg" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Second view — same GT / render split.</div>
  </div>
</div>

## Overview

A bare-metal C++/CUDA differentiable 3D Gaussian Splatting engine built for real-time performance and systems control: custom tile rasterization, EWA projection, and degree‑3 spherical harmonics, exposed to PyTorch as a C++ extension. The focus is GPU throughput and memory layout—not wrapping an existing rasterizer—targeting **114 FPS @ 720p** and **56 FPS @ 1080p**.

## System & Architecture

- **Forward path** — EWA projection, degree‑3 SH, tile-based alpha blending with sorted per-tile splat lists.
- **Backward path** — gradients through projection, SH, and compositing for end-to-end training.
- **Memory & occupancy** — layouts and shared memory tuned for coalescing and SM occupancy at high splat counts.
- **PyTorch extension** — autograd hooks so training loops call the CUDA engine directly.

## Evaluation

Reconstruction on **151** test views (100k Gaussians):

| Setting | Views | L1 ↓ | PSNR ↑ | SSIM ↑ |
|---|---:|---:|---:|---:|
| Full frame | 151 | 0.266530 | **10.336 dB** | 0.470 |
| Center crop (50%) | 151 | 0.143482 | **14.629 dB** | 0.581 |

Center-crop metrics emphasize the middle of the image where tile coverage is densest; full-frame numbers include border regions with fewer contributing splats.

## Benchmarks

**Device:** NVIDIA GeForce RTX 2060 SUPER · **Gaussians:** 100,000 · **Warmup:** 100 · **Timed iters:** 100

| Resolution | Forward (ms) | Backward (ms) | FPS |
|---:|---:|---:|---:|
| 1280×720 | 8.747 | 71.124 | **114.3** |
| 1920×1080 | 17.724 | 154.640 | **56.4** |

Throughput uses the custom tile rasterization + EWA forward/backward path exposed to PyTorch autograd (same engine quoted in the overview: **~114 FPS @ 720p**, **~56 FPS @ 1080p**).

## Engineering Notes & Next Steps

- Trade off numerical fidelity vs. occupancy in shared-memory tile queues and alpha blending order.
- Keep host↔device transfers off the hot path; pack splat attributes for coalesced loads.
- PyTorch extension packaging and ABI stability matter as much as kernel speed for day-to-day training.
- **Next:** side-by-side FPS vs. reference 3DGS CUDA baselines; improve border/crop fidelity to close the full-frame vs. center-crop gap.
