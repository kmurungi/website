---
layout: page
title: Custom 3D Gaussian Splatting (CUDA)
description: Bare-metal C++/CUDA differentiable 3DGS engine achieving 114 FPS @ 720p with custom tile rasterization and EWA kernels
img: assets/img/cuda_3dgs_preview.gif
importance: 2
permalink: /projects/cuda-3dgs/
---

<p class="mb-4">
  <a class="btn btn-sm btn-outline-primary me-2" href="https://github.com/kmurungi" target="_blank" rel="noopener">GitHub Repository</a>
  <a class="btn btn-sm btn-outline-secondary" href="#">Demo video (coming soon)</a>
</p>

{% include figure.liquid path="assets/img/cuda_3dgs_preview.gif" class="img-fluid rounded z-depth-1" %}
<div class="caption">
  Preview placeholder — swap in a renderer GIF/video showing live 3DGS playback.
</div>

## Overview

A bare-metal C++/CUDA differentiable 3D Gaussian Splatting engine built for real-time performance and systems control: custom tile rasterization, EWA projection, and degree‑3 spherical harmonics, exposed to PyTorch as a C++ extension. The focus is GPU throughput and memory layout—not wrapping an existing rasterizer—targeting **114 FPS @ 720p** and **56 FPS @ 1080p**.

## System & Architecture

1. **Forward kernels** — EWA projection of anisotropic 3D Gaussians, degree‑3 SH evaluation, and tile-based alpha blending with sorted per-tile splat lists.
2. **Backward kernels** — matching gradients through projection, SH, and alpha compositing for end-to-end training.
3. **Memory & occupancy** — buffer layouts and shared-memory choices tuned for coalescing and SM occupancy under dense splat counts.
4. **PyTorch integration** — C++ extension + autograd hooks so training/inference loops call the CUDA engine without leaving the Python stack for bookkeeping.

## Benchmarks & Results

| Resolution | Throughput | Notes |
|---|---:|---|
| 720p | **114 FPS** | Custom tile rasterization + EWA kernels |
| 1080p | **56 FPS** | Same engine, higher pixel load |

*(Replace with your final benchmark table vs. reference implementations when ready.)*

## Engineering Notes & Next Steps

- Trade off numerical fidelity vs. occupancy in shared-memory tile queues and alpha blending order.
- Keep host↔device transfers off the hot path; pack splat attributes for coalesced loads.
- PyTorch extension packaging and ABI stability matter as much as kernel speed for day-to-day training.
- **Next:** publish repo + side-by-side FPS vs. reference 3DGS CUDA baselines; add a short demo GIF of live navigation.
