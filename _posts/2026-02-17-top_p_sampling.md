---
title: "Performance Analysis: Top-P Sampling in Large Language Models"
date: 2026-02-17 10:00:00 +0000
layout: post
summary: "Whenever you interact with ChatGPT, Gemini, or any modern LLM, the final inference step determining response speed is top-p (nucleus) sampling - the algorithm that selects tokens from the model's vocabulary.

I've conducted CUDA kernel profiling analysis of this critical sampling operation. The results reveal a clear performance bottleneck."
---


Whenever you interact with ChatGPT, Gemini, or any modern LLM, the final inference step determining response speed is top-p (nucleus) sampling - the algorithm that selects tokens from the model's vocabulary.

I've conducted CUDA kernel profiling analysis of this critical sampling operation. The results reveal a clear performance bottleneck. 

📊 Radix sort dominates execution time (71-83% of total processing) and becomes increasingly problematic with larger vocabularies, reaching up to 83% of total time at 131K vocabulary size.

This bottleneck presents a significant optimization opportunity for improving LLM inference latency. 

<p style="max-width:100%;overflow:hidden">
	<img src="{{ site.baseurl }}/assets/top-p-profile.png" alt="Top-P sampling profiling" style="width:100%;height:auto;display:block;margin:12px 0" />
</p>

You can see the full analysis and code: <a href="https://github.com/MattJBorowski1991/RadixSortNCU/blob/master/top_p/README.md" target="_blank" rel="noopener">here</a>.