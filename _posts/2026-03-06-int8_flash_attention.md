---
title: "Quantized (Int8) Flash Attention: smaller blocks = higher occupancy"
date: 2026-03-06 10:00:00 +0000
layout: post
summary: >
	Compared Br=32 vs Br=64 for my custom int8 Flash Attention kernel. Top-line:
	Br=32 = 9.06 ms vs Br=64 = 12.17 ms → ≈25% faster
	Memory throughput: ≈69% vs ≈50% → ~38% higher; L2 throughput ≈2.1× higher
	Compute & memory % of peak both rose ≈50% → ≈69% (better utilization)

	Why: Br=64 uses more shared memory/registers per block, reducing resident blocks/SM and amplifying load imbalance and unavoidable per-warp accumulation barriers (guarded by if(warp_tile_col_id==0]). Br=32 fits more blocks/SM, boosts L2 locality, and yields better real-world throughput.
---

Compared Br=32 vs Br=64 for my custom int8 Flash Attention kernel. Top-line:
Br=32 = 9.06 ms vs Br=64 = 12.17 ms → ≈25% faster
Memory throughput: ≈69% vs ≈50% → ~38% higher; L2 throughput ≈2.1× higher
Compute & memory % of peak both rose ≈50% → ≈69% (better utilization)

Why: Br=64 uses more shared memory/registers per block, reducing resident blocks/SM and amplifying load imbalance and unavoidable per-warp accumulation barriers (guarded by if(warp_tile_col_id==0]). Br=32 fits more blocks/SM, boosts L2 locality, and yields better real-world throughput.

See the kernel code on my GitHub [here](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/mha_kernels/fa_tc_int8_a.cu), the full analysis [here](https://github.com/MattJBorowski1991/QuantizedMHA/blob/ef703c80463daea02902b0059cad5d8f4648667a/profiles/md/run6/ncu_Br32_vs_Br64_details.md) and the summary of how I performed quantization [here](https://github.com/MattJBorowski1991/QuantizedMHA/blob/ef703c80463daea02902b0059cad5d8f4648667a/profiles/md/run6/int8_notes.md) .

## Highlights

- **Occupancy:** Br=32 increases block/SM residency and L2 locality, improving real throughput.
- **Resource Pressure:** Br=64’s higher shared-memory/register use reduces resident blocks and causes imbalance.
- **Synchronization:** Per-warp accumulation guard (if(warp_tile_col_id==0)) forces unavoidable barriers and stalls.
- **Bank Conflicts:** Splitting work across warps creates shared-memory bank hotspots that serialize accesses.
- **Compile Tradeoff:** Using -maxrregcount=40 raised residency but induced register spilling and extra memory traffic.
- **Next:** Port Int8 to fa_tc_v1 (single-warp) and focus on padding/re-indexing or swizzled accesses to remove conflicts.

<p style="max-width:100%;overflow:hidden">
	<img src="{{ site.baseurl }}/assets/int8a_throughput.png" alt="Int8 Flash Attention" style="width:100%;height:auto;display:block;margin:12px 0" />
</p>