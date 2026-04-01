---
title: "Warp work distribution in Flash Attention"
date: 2026-03-04 10:00:00 +0000
layout: post
summary: >
  Flash Attention with 8×32×16 WMMA tiles along with d-axis warp work split enable more warps per block in Flash Attention and hike occupancy 100% (8→16 warps per scheduler) for Br=64. SRAM pressure, however, bars padding, leading to bank conflicts. A detailed analysis with Nsight Compute on parallelism trade-offs.
---

  Flash Attention with 8×32×16 WMMA tiles along with d-axis warp work split enable more warps per block in Flash Attention and hike occupancy 100% (8→16 warps per scheduler) for Br=64. SRAM pressure, however, bars padding, leading to bank conflicts. A detailed analysis with Nsight Compute on parallelism trade-offs.

## Highlights

- **Occupancy Increase:** Increased warp occupancy by splitting work along the d-dimension (8×32×16 WMMA tiles vs 16×16×16), doubling active warps for Br=64 but exposing hidden costs.
- **SRAM Pressure:** Observed major SRAM pressure: accumulation buffers (~32 KB each) push per-block SRAM near limits, causing overflows and silent failures when padding increases.
- **Bank Conflicts:** Doubling warps reduced compute throughput due to shared-memory bank conflicts (≈3‑way) that serialized accesses and reduced issued work.
- **Scheduler Bottlenecks:** Scheduler and warp-state stats show more active warps but only ~35% increase in eligible warps and >120% rise in warp cycles per instruction (Stall Barrier, MIO Throttle).
- **Validation Run:** Validation with Br=32 (fa_tc_v2a, PAD=16) eliminated bank conflicts and lowered latency (~7.5 ms), confirming conflict-driven slowdown at Br=64.
- **Next:** Root cause fixes and next steps: guard race in accumulation (fixed), and prioritize mitigating bank conflicts and reducing per-warp/shared allocations (re-indexing, padding, swizzling, or preserving single-warp work).


<p style="max-width:100%;overflow:hidden;">
  <video controls playsinline style="width:100%;height:auto;display:block;margin:12px 0;">
    <source src="{{ site.baseurl }}/assets/fa_tc_v1_warp_work.mp4" type="video/mp4">
    Your browser does not support the video tag. <a href="{{ site.baseurl }}/assets/fa_tc_v2a_warp_work.mp4">Download the MP4</a>.
  </video>
</p>

See the kernel code on my GitHub [here](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/mha_kernels/fa_tc_v1a.cu) and the full analysis [here](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/profiles/md/run4/ncu_details.md).