---
title: "GEMM Evolution: CUDA suite with 13 kernels"
date: 2025-11-29 10:00:00 +0000
layout: post
summary: >
  Progressive CUDA GEMM suite implementing 13 kernels, from naive and anti-patterns to shared/register tiling,
  float4/vectorized kernels, autotuned tile sizes, warp-level WMMA, and cp.async double-buffered pipelines.
  Includes cuBLAS references. The unified runner records GFLOPS, GB/s, arithmetic intensity (AI), and occupancy
  across matrix sizes (1K–8K), and generates roofline plots plus Markdown performance tables.
---


GEMM Evolution is a progressive CUDA suite of 13 GEMM kernels. It demonstrates a sequence of optimizations
and design patterns (naive → tiled → vectorized → WMMA → cp.async double buffering) and provides a
unified runner for automated benchmarking and visualization.

## Highlights

- **Best custom kernel throughput:** `08_double_buffered_c` demonstrates the highest custom-kernel GFLOPS at large sizes — compute-bound (arithmetic intensity ≫ roofline ridge).
- **Library reference:** `cuBLAS` achieves very high GFLOPS at N = 8192, serving as a practical upper bound for comparison.
- **Occupancy trends:** Warp-level WMMA and large thread-blocks can reduce occupancy; double buffering helps balance resources and utilization.
- **Bandwidth scaling:** Observed GB/s often decreases with matrix size while AI increases; at large matrices reuse and compute dominate performance.

<p style="max-width:100%;overflow:hidden;">
  <img src="{{ site.baseurl }}/assets/roofline.png" alt="GEMM Evolution: Roofline" style="width:100%;height:auto;display:block;margin:12px 0;" />
</p>

You can see the full project on GitHub: [GemmEvolution](https://github.com/MattJBorowski1991/GemmEvolution).