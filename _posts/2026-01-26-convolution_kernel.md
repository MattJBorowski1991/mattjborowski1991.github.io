---
title: "Convolution Kernel profiling"
date: 2026-01-26 10:00:00 +0000
layout: post
summary: >
  Optimized a convolution kernel using shared memory, constant memory, and load widening. Latency
  decreased from 4.4 ms to 3.0 ms via Nsight Compute profiling and targeted bottleneck fixes.
---



I optimized a convolution kernel by removing shared-memory bottlenecks, moving suitable constants to `__constant__`, and applying load-widening techniques. These changes reduced warp-stalling and increased ALU utilization (approx. 5% → 50%), yielding the measured latency improvement.

## Highlights

- **Memory strategy:** Moved kernel constants to `__constant__` where beneficial (intra-warp broadcast).
- **Load widening:** `#pragma unroll` and autotuning further alleviated MIO throttle stalls.
- **Profiling:** Nsight Compute (NCU) for hotspots and SASS/PTX inspection for instruction mix.
- **Outcome:** Reduced stalls, higher ALU throughput, and small codegen shifts (more integer ops).

See the kernels: [ConvProfNCU/kernels](https://github.com/MattJBorowski1991/ConvProfNCU/tree/main/kernels) and the full analysis: [profiling_summary.md](https://github.com/MattJBorowski1991/ConvProfNCU/blob/main/profiling_summary.md).