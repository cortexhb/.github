# Cortex

A self-hosted, privacy-first personal AI assistant built to run entirely on hardware I control.

No cloud LLM calls. No personal data leaving the infrastructure. Everything local.

---

## What it is

Cortex is a long-running personal project to build an AI assistant that:

- Runs local LLMs on Apple Silicon via MLX-LM
- Learns about me over time through persistent memory (vector + structured)
- Integrates deeply with a homelab (Home Assistant, Pi-hole, Traefik, Docker)
- Can plan and execute multi-step tasks autonomously
- Reads its own codebase and proposes improvements to itself

The goal is an assistant that feels like an extension of how I think and work — not a cloud product I'm renting access to.

## Repos

| Repo | What it is |
|------|-----------|
| [`cortex_kb`](../cortex_kb) | Research and design documentation — vision, architecture decisions, phase specs |
| `cortex` | Core assistant codebase (Python) — coming in Phase 0 |

## Stack

| Layer | Choice |
|-------|--------|
| Inference | MLX-LM on Apple Silicon |
| LLM | Qwen2.5 72B (Phase 1), QwQ-32B for reasoning (Phase 5) |
| Orchestration | Custom Python — no frameworks |
| Memory | Qdrant (vector / episodic) + SQLite (explicit facts) |
| Interface | Telegram → Matrix |
| Web search | SearXNG (self-hosted) |
| Networking | Tailscale |
| Hardware | MacBook Pro M1 (Phase 0 prototype) → Mac Mini M4 Pro 64GB (Phase 1+) |

## Status

Currently in the research and design phase. Phase 0 (prototype on existing hardware) is the next concrete milestone.

See [`cortex_kb/docs/vision.md`](../cortex_kb/docs/vision.md) for the full roadmap.
