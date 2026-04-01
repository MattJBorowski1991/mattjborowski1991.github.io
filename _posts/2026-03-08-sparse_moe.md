---
title: "Capacity-aware Sparse MoE"
date: 2026-03-08 10:00:00 +0000
layout: post
summary: >
The first profiling run for Spare MoE shows fused kernels vastly outperform unfused: the unfused workflow spends ~2034 ms dominated by WMMA up_proj/Swiglu/down_proj kernels, while the fused baseline variant runs in ~54 ms and the capacity aware version in ~37 ms. Capacity-aware per-expert buffering substantially improves compute/memory balance (≈46% speedup vs baseline) by reducing redundant DRAM↔shared copies and improving locality. 
---

SparseMoE run1 profiling shows fused kernels vastly outperform unfused: the unfused workflow spends ~2034 ms dominated by WMMA up_proj/Swiglu/down_proj kernels, while the fused baseline variant runs in ~54 ms and the capacity aware version in ~37 ms. Capacity-aware per-expert buffering substantially improves compute/memory balance (≈46% speedup vs baseline) by reducing redundant DRAM↔shared copies and improving locality. 

Nsight Compute points to uncoalesced cp.async.ca.shared.global copies and WMMA load paths as primary hotspots, and flags a redundant __syncthreads() after cp.async.wait_group that contributes ~58% of warp stalls. Next steps are to remove unnecessary barriers, tighten per-expert buffering, and focus WMMA kernels for further gains. 

See the full analysis on my GitHub [here](https://github.com/MattJBorowski1991/SparseMoE/blob/master/prof/md/run1/ncu_details.md), and the kernels: [unfused.cu](https://github.com/MattJBorowski1991/SparseMoE/blob/master/kernels/unfused.cu), [baseline.cu](https://github.com/MattJBorowski1991/SparseMoE/blob/master/kernels/baseline.cu) and [capacity.cu](https://github.com/MattJBorowski1991/SparseMoE/blob/master/kernels/capacity.cu).

## Highlights

- **Performance:** Fused (capacity) dramatically outperforms the unfused workflow.
- **Capacity:** Per‑expert buffering reduces redundant DRAM↔shared copies and evens load.
- **Hotspots:** cp.async.ca.shared.global copies and WMMA load paths are the primary sources of stalls.
- **Synchronization:** Redundant __syncthreads() after cp.async.wait_group drives ~58% of warp stalls.
- **Memory ↔ Compute:** capacity shifts the kernel from DRAM‑bound toward better compute/cache utilization.
- **Next Steps:** Remove the redundant barrier, tighten per‑expert sizing, and optimize WMMA kernels/padding.

<p style="max-width:100%;overflow:hidden">
	<img src="{{ site.baseurl }}/assets/capacity_source_code_uncoalesced_shared_access_2.jpg" alt="SparseMoE - uncoalesced access" style="width:100%;height:auto;display:block;margin:12px 0" />
</p>