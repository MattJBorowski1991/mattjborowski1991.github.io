---
title: "Long Scoreboard Warp Stalls in Radix"
date: 2026-02-25 10:00:00 +0000
layout: post
summary: "Optimized a custom radix-sort pipeline using Nsight Compute. Profiling Warp State Statistics and uncoalesced global accesses reduced long-scoreboard stalls by over 50%, illustrating GPU memory-hierarchy trade-offs and optimization trade-offs."
---

Optimized a custom radix-sort pipeline using Nsight Compute. Profiling Warp State Statistics and uncoalesced global accesses reduced long-scoreboard stalls by over 50%, illustrating GPU memory-hierarchy trade-offs and optimization trade-offs.

## Highlights

- **Root cause:** Uncoalesced global loads/stores triggered 11+ cycle stalls per warp (Warp State Statistics).
- **Fixes attempted:** Moving hot inputs to `SRAM` reduced long-scoreboard stalls by ~50% but surfaced new
	bottlenecks (barriers, short-scoreboards) that require synchronization and algorithmic attention.
- **Trade-offs:** The prefix kernel incurred ~20% slowdown due to sparse writes — this needs an algorithmic
	redesign to recover performance while preserving overall correctness.
- **Outcome:** SRAM utilization improved and the radix kernel remained stable; the work highlights the
	iterative, trade-off-driven nature of CUDA performance tuning.

<p style="max-width:100%;overflow:hidden;">
	<img src="{{ site.baseurl }}/assets/run2_radix_v2_long_scoreboard.png" alt="Radix sort long-scoreboard profile" style="width:100%;height:auto;display:block;margin:12px 0;" />
</p>

See the kernel code on GitHub: [radix_v2.cu](https://github.com/MattJBorowski1991/RadixSortNCU/blob/master/kernels/radix_v2.cu) and the full analysis: [run2 results](https://github.com/MattJBorowski1991/RadixSortNCU/blob/master/prof/results/run2.md).