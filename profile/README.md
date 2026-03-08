# Cortex

A self-hosted, privacy-first personal AI assistant built to run entirely on hardware I control.

No cloud LLM calls. No personal data leaving the infrastructure. Everything local.

---

## What it is

Cortex is a long-running personal project to build an AI assistant that:

- Runs local LLMs on Apple Silicon via MLX-LM
- Learns about me over time through persistent memory (vector + graph)
- Integrates deeply with a homelab (Home Assistant, Pi-hole, Traefik, Docker)
- Can plan and execute multi-step tasks autonomously
- Is accessible via Matrix with full E2E encryption and cross-signing verification

The goal is an assistant that feels like an extension of how I think and work — not a cloud product I'm renting access to.

## Repos

| Repo | What it is |
|------|-----------|
| [`core`](../core) | AI backend — LangGraph agent, mem0 memory, MCP tools, WebSocket API |
| [`cortex_matrix_bot`](../cortex_matrix_bot) | Matrix interface — E2EE, streaming responses, in-room SAS verification |
| [`telegram_bot`](../telegram_bot) | Telegram interface — streaming tokens, background runs |
| [`deep_research_agent`](../deep_research_agent) | Iterative web research subprocess — multi-step search and synthesis |
| [`home_orchestration`](../home_orchestration) | Home Assistant control subprocess — natural-language smart home commands |
| [`kb`](../kb) | Research and design documentation — vision, architecture decisions, phase specs |

## Stack

| Layer | Choice |
|-------|--------|
| Inference | MLX-LM on Apple Silicon |
| LLM | Qwen2.5 14B (default), swappable via `MODEL_NAME` |
| Orchestration | LangGraph |
| Memory | Qdrant (vector/episodic) + SQLite (checkpointer) + Neo4j (graph, optional) |
| Interfaces | Matrix (primary) · Telegram |
| Tools | MCP servers · SearXNG · Home Assistant · custom Python tools |
| Web search | SearXNG (self-hosted) |
| Networking | Tailscale |
| Hardware | Mac Mini M4 Pro 64 GB |

## Architecture

```
┌─────────────────────────────────────┐
│              core/                   │
│  LangGraph agent · mem0 · Qdrant    │
│  MCP tools · SQLite · runs registry │
│                                      │
│  ws://localhost:8765 ◄───────────────┼── cortex_matrix_bot/
│                      ◄───────────────┼── telegram_bot/
└─────────────────────────────────────┘
        │
        ├── deep_research_agent/   (subprocess)
        └── home_orchestration/    (subprocess)
```

`core` exposes a streaming WebSocket API. Interface bots connect to it as thin transport layers — all agent logic, memory, and tools stay in `core`.
