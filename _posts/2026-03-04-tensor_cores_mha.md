---
title: "Tensor Cores + Multi-Head Attention"
date: 2026-03-04 10:00:00 +0000
layout: post
summary: >
  I profiled the standard fused Flash Attention kernel against a Tensor Core–optimized version.
  Result: 30.7% runtime speedup (8.33ms → 5.77ms) using `WMMA` for `Q@K^T` and `P@V`, where one warp owns a
  `16×d` chunk of `Q` and processes a single `16×16×16` tile at a time.
---


I profiled the standard fused Flash Attention kernel against a Tensor Core–optimized implementation and measured a 30.7% runtime speedup (8.33ms → 5.77ms). The implementation uses `WMMA` for `Q@K^T` and `P@V`; each warp owns a `16×d` chunk of `Q` and processes `16×16×16` tiles serially.

## Highlights

- **Speedup:** 30.7% runtime improvement (8.33ms → 5.77ms).
- **Approach:** `WMMA` for `Q@K^T` and `P@V`; one warp processes `16×16×16` tiles for a `16×d` Q chunk.
- **Efficiency:** Throughput (memory/compute) dropped from 67.83% to 46.05% while runtime improved — indicating fewer idle cycles and higher work-per-clock.
- **Challenge:** Low occupancy (~16.7%) due to `Br=64` row SRAM pressure on the L4 GPU; this forces serial tile processing per warp, trading parallelism for memory fit.
- **Next steps:** Try `8×8×32` tiles to improve occupancy and add multi-warp d-splits to increase theoretical active warps per scheduler.

<p style="max-width:100%;overflow:hidden;">
  <video controls playsinline style="width:100%;height:auto;display:block;margin:12px 0;">
    <source src="{{ site.baseurl }}/assets/fa_tc_v1_warp_work.mp4" type="video/mp4">
    Your browser does not support the video tag. <a href="{{ site.baseurl }}/assets/fa_tc_v1_warp_work.mp4">Download the MP4</a>.
  </video>
</p>

See the kernel code on my GitHub [here](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/mha_kernels/fa_tc_v1a.cu) and the full analysis [here](https://github.com/MattJBorowski1991/QuantizedMHA/blob/main/profiles/md/run3a/ncu_high_level.md).