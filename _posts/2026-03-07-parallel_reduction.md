---
title: "Twelve Parallel Reduction Kernels"
date: 2026-03-07 10:00:00 +0000
layout: post
summary: >
	CUDA benchmark suite implementing 12 parallel-reduction kernels with comprehensive Nsight Compute analysis and charts comparing latency, memory throughput, scheduler stats, and instruction/source counters. The results show bandwidth-bound behavior where the highest DRAM throughput (the atomic_global kernel) yields the best runtimes, while divergence and instruction overhead (e.g., interleaved_addr_divergent_branch) severely hurt performance. 
---

CUDA benchmark suite implementing 12 parallel-reduction kernels with comprehensive Nsight Compute analysis and charts comparing latency, memory throughput, scheduler stats, and instruction/source counters. The results show bandwidth-bound behavior where the highest DRAM throughput (the atomic_global kernel) yields the best runtimes, while divergence and instruction overhead (e.g., interleaved_addr_divergent_branch) severely hurt performance.

See the kernels on my GitHub [here](https://github.com/MattJBorowski1991/ParallelReduction/tree/master/kernels), the full analysis [here](https://github.com/MattJBorowski1991/ParallelReduction?tab=readme-ov-file) and an Nsight Compute comparison of the best and worst kernels [here](https://github.com/MattJBorowski1991/ParallelReduction/blob/master/profiling_best_vs_worst.md).

## Highlights

- **Key Result:** atomic_global beats interleaved_addr_divergent_branch by ~3.7× at large N (measured N≈1.07B).
- **DRAM Impact:** DRAM throughput is the dominant lever (example: memory throughput rises from ~33→110 GB/s in the faster variant).
- **Instruction Reduction:** Faster kernels issue far fewer instructions (issued/executed drop ≈90%), cutting overhead.
- **Divergence Cost:** Branch-heavy/uncoalesced designs explode instruction counts and drive stalls via warp divergence.
- **Occupancy Caveat:** Higher occupancy alone isn’t sufficient — lower occupancy can win if it increases effective DRAM throughput.
- **Reproducibility:** Full NCU reports, plots, build/profile scripts, and a PyTorch extension are included for easy validation and integration

<p style="max-width:100%;overflow:hidden">
	<img src="{{ site.baseurl }}/assets/parallel_reduction.png" alt="Parallel Reduction comparison" style="width:100%;height:auto;display:block;margin:12px 0" />
</p>