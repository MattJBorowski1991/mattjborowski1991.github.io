---
title: "Unfused Multi-Head Attention vs Flash Attention"
date: 2026-02-10 10:00:00 +0000
layout: post
summary: >
  Profiling analysis of unfused MHA (`mma` + `softmax` + `mma`) compared to a Flash
  Attention kernel where one warp owns four rows of `Q`. Nsight Compute highlights
  three primary bottlenecks: bank conflicts, low occupancy, and sub‑optimal grid
  utilization.
---


This post compares an unfused MHA implementation (separate mma, softmax, mma) with a
fused Flash Attention kernel that assigns four rows of Q per warp. Nsight Compute reveals
several performance issues that compound and limit end-to-end throughput.

## Highlights

- **Bank conflicts:** ~68.6% of shared-memory stores show bank conflicts, reducing achieved
  throughput to ~45.9% versus a theoretical 82–90% — eliminating conflicts could yield
  a ~53% improvement in throughput.
- **Occupancy:** Achieved occupancy ≈ 37% vs theoretical 66.7%; register pressure (~64 regs/thread)
  prevents more blocks from running concurrently — reducing register use could recover ~44%.
- **Grid utilization:** Only 64 blocks were launched, which does not fully saturate 58 SMs given
  current per-block resource usage; increasing concurrency or reducing per-block footprint
  would improve utilization.

These three issues compound: fixing any single one yields limited gains (~35%), so a combined
approach is required for significant end-to-end improvement.

## Architectural insight

The unfused baseline achieves higher occupancy (≈87–100%) by using more, simpler blocks. The
custom fused kernel consolidates work and reduces overhead but introduces memory-access
patterns and register pressure that hurt occupancy — a common fusion tradeoff.

See the kernels on GitHub: [unfused.cu](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/mha_kernels/unfused.cu), [fa.cu](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/mha_kernels/fa.cu), and the full analysis: [ncu_details.md](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/profiles/md/run1/ncu_details.md).