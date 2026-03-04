---
title: "Radix Sort & Top-P Sampling Kernels in LLMs"
date: 2026-02-25 10:00:00 +0000
layout: post
summary: "Optimization of a custom Radix Sort pipeline using Nsight Compute. Diving into profiling bottlenecks like Warp State Statistics and uncoalesced global accesses, I reduced Long Scoreboard Stalls by over 50%—a great lesson in GPU memory hierarchies and trade-offs."
---


Optimization of a custom Radix Sort pipeline using Nsight Compute. Diving into profiling bottlenecks like Warp State Statistics and uncoalesced global accesses, I reduced Long Scoreboard Stalls by over 50%—a great lesson in GPU memory hierarchies and trade-offs. 

Root causes uncovered: 
Global-to-global load/stores triggering 11+ cycle stalls per warp revealed the cost of poor coalescing.

Optimization experiments: 
Shifting inputs to SRAM cut stalls in half but exposed new bottlenecks (barriers, short scoreboards)—highlighting that fixes often reveal deeper issues.

Results & reflections: Radix kernel held steady, prefix kernel took a 20% hit (sparse writes need algorithmic redesign), yet SRAM utilization improved. This reinforced CUDA optimization as a delicate balance of locality, coalescing, and iterative testing. 

<p style="max-width:100%;overflow:hidden">
	<img src="{{ site.baseurl }}/assets/run2_radix_v2_long_scoreboard.png" alt="Top-P sampling profiling" style="width:100%;height:auto;display:block;margin:12px 0" />
</p>

See the kernel code on my GitHub [here](https://github.com/MattJBorowski1991/RadixSortNCU/blob/master/kernels/radix_v2.cu) and the full analysis [here](https://github.com/MattJBorowski1991/RadixSortNCU/blob/master/prof/results/run2.md).