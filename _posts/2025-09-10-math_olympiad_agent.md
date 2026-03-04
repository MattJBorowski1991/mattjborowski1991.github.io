---
title: "Math Olympiad Agent: Self-Verification for IMO Problems"
date: 2025-09-10 10:00:00 +0000
layout: post
summary: >
	This project implements an AI agent for solving International Mathematical Olympiad (IMO)-level problems,
	inspired by the self-verification pipeline described in the paper "Gemini 2.5 Pro Capable of Winning Gold at IMO 2025"
	by Yichen Huang and Lin F. Yang. The agent uses a multi-stage iterative process with rigorous verification to
	generate and refine solutions. It is built with LangGraph for workflow orchestration and supports the A2A
	(Agent-to-Agent) protocol for potential multi-agent collaboration.
---


This project implements an AI agent for solving International Mathematical Olympiad (IMO)-level problems. It adapts the self-verification pipeline from the paper "Gemini 2.5 Pro Capable of Winning Gold at IMO 2025" (Yichen Huang and Lin F. Yang) and uses a multi-stage iterative process with automated verification. Built with LangGraph for workflow orchestration, the agent supports the A2A (Agent-to-Agent) protocol for multi-agent collaboration and is extensible to different LLM backends.

## 🚀 Highlights

- **Cutting‑Edge Inspiration:** Direct adaptation of the Gemini 2.5 Pro IMO pipeline (5/6 problems solved in the 2025 evaluation).
- **Rigorous Proofs:** Produces logically sound solutions rather than just final answers.
- **Extensible Design:** Compatible with multiple LLMs (Gemini, OpenAI-compatible, etc.) and supports the A2A protocol.
- **Open-Source Reimplementation:** Faithfully recreates the paper's method while leaving room for experimentation.

<p style="max-width:100%;overflow:hidden;">
  <img src="{{ site.baseurl }}/assets/math_olympiad_agent_pipeline.png" alt="Math Olympiad Agent: Pipeline" style="width:100%;height:auto;display:block;margin:12px 0;" />
</p>

You can see the full project on GitHub: [AgentMathOlympiadMedalist](https://github.com/MattJBorowski1991/AgentMathOlympiadMedalist).