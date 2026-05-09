<h1 align="center">BrainCore</h1>

<p align="center">
  <strong>The memory layer that lets your AI coding agent say "I don't know."</strong>
</p>

<p align="center">
  <em>Local-first · 0.95 R@5 · 4ms p95 retrieval · Strict abstain by design</em>
</p>

<p align="center">
  <a href="https://getbraincore.com">Website</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="https://github.com/vbcherepanov/total-agent-memory">Open-source companion</a>
</p>

<p align="center">
  <a href="https://getbraincore.com"><img alt="Website" src="https://img.shields.io/badge/site-getbraincore.com-10b981?style=flat-square"></a>
  <a href="#"><img alt="Status" src="https://img.shields.io/badge/status-Private%20Beta-blueviolet?style=flat-square"></a>
  <a href="#"><img alt="License" src="https://img.shields.io/badge/license-Apache--2.0-blue?style=flat-square"></a>
  <a href="#"><img alt="Made with Go" src="https://img.shields.io/badge/Go-1.25-00ADD8?logo=go&style=flat-square"></a>
  <a href="#"><img alt="MCP" src="https://img.shields.io/badge/MCP-stdio-1f6feb?style=flat-square"></a>
  <a href="https://x.com/BestProgerVR"><img alt="X / Twitter" src="https://img.shields.io/badge/follow-%40BestProgerVR-1DA1F2?logo=x&style=flat-square"></a>
</p>

---

## Why this exists

You ship code with an AI coding agent. You watch it confidently rewrite a function based on a chunk that belongs to a branch deleted two months ago. Cosine similarity high. Top-1 retrieval. Honest stitch into the prompt. Patch against code from a different reality.

That's not memory failing. That's **search masquerading as memory** — RAG with cosine instead of BM25.

**BrainCore is the layer that closes that gap.** Every fact your agent uses to generate code passes through a strict-mode gate before it lands in the prompt — and if no fact survives, the agent says *"I don't know"* instead of inventing one.

---

## What BrainCore actually does

BrainCore is a **local-first cognitive memory** that sits between your IDE / coding agent and your codebase. Every fact your agent uses to generate code passes through a **strict-mode gate** before it lands in the prompt — and if no fact survives, the agent says *"I don't know"* instead of inventing one.

```mermaid
flowchart LR
    A[Your AI agent<br/>Claude Code · Codex · Cursor] -->|MCP stdio| B[BrainCore]
    B --> C[Atomic Knowledge Units<br/>w/ lifecycle + provenance]
    B --> D[Decision Graph<br/>problem → choice → outcome]
    B --> E[Source-Code Truth<br/>AST · go.mod · package.json]
    C & D & E -->|gate| F{Strict Mode}
    F -->|all pass| G[Trusted context<br/>injected into prompt]
    F -->|any fail| H[abstain → brain task<br/>'I need evidence for X']
    style F fill:#10b981,stroke:#0a8060,color:#fff
    style H fill:#ef4444,stroke:#991b1b,color:#fff
    style G fill:#1f6feb,stroke:#0d4ba0,color:#fff
```

Result: an agent that *refuses* to write code based on a deleted file, a deprecated decision, or a hypothetical fact you never confirmed.

---

## The headline numbers

<table>
<tr>
  <td align="center"><strong>0.95 R@5</strong><br/><sub>retrieval recall<br/>at gate threshold 0.85</sub></td>
  <td align="center"><strong>4 ms p95</strong><br/><sub>retrieval latency<br/>across atomic + graph</sub></td>
  <td align="center"><strong>0%</strong><br/><sub>confidently-wrong<br/>actions in bench</sub></td>
  <td align="center"><strong>30%</strong><br/><sub>honest abstain rate<br/>(traded for the 0% above)</sub></td>
</tr>
<tr>
  <td align="center"><strong>11/11</strong><br/><sub>legacy migration tests<br/>green</sub></td>
  <td align="center"><strong>Local</strong><br/><sub>your code never leaves<br/>the box, period</sub></td>
  <td align="center"><strong>MCP</strong><br/><sub>plugs into Claude Code,<br/>Codex, Cursor, Cline</sub></td>
  <td align="center"><strong>Apache-2.0</strong><br/><sub>open companion:<br/>total-agent-memory</sub></td>
</tr>
</table>

---

## Cognitive functions BrainCore models

A "memory tool" stores text. A **cognitive layer** models how an agent should
*think* about that text. BrainCore implements the cognitive functions that
turn raw retrieval into a brain that can reason — and stay silent when it has to.

| Cognitive function | What BrainCore implements | What's broken without it |
|---|---|---|
| **Memory** | Atomic knowledge units with lifecycle: `staging → working → consolidated → archived` | Stale chunks parade as current truth |
| **Attention** | Strict-mode gate filters by source, confidence, temporal validity, contradiction | Top-k cosine returns everything, agent reads garbage |
| **Reasoning** | Causal decision chains (`problem → alternatives → decision → reasoning → outcome`) | Three flat fragments, model invents the missing logic |
| **Perception of code** | AST-based identity (Tree-sitter, 9 langs) — "this symbol", not "this string" | Patches written against deleted branches |
| **Negative learning** | Failures, regressions, rejected decisions are first-class entities | Agent ships the same bug it shipped 3 months ago |
| **Metacognition** | Self-model: competencies, blind spots, brain-tasks backlog | Agent doesn't know what it doesn't know |
| **Abstain** | "I don't know" as a first-class outcome, with an explicit reason | Confident hallucinations indistinguishable from real answers |

These map 1-to-1 onto the [seven architectural principles](#the-seven-principles-braincore-is-built-on) below — every cognitive function has a Postgres schema and a Go module behind it, not a prompt-engineering trick.

---

## The seven principles BrainCore is built on

The full deep-dive is in [Part 2 of the series](#articles). Headlines:

| # | Principle | What it kills |
|---|-----------|---------------|
| 1 | **Atomic Knowledge Units with lifecycle** — `staging → working → consolidated → archived` | Stale chunks parading as current truth |
| 2 | **Strict Mode + right to abstain** — no fact → no answer | "Confident hallucinations" disguised as accuracy |
| 3 | **Causal decision chains** — `problem → alternatives → decision → reasoning → outcome` | Decisions reduced to three flat fragments by the chunker |
| 4 | **AST-based code identity** — symbols, not text | Patches written against deleted branches |
| 5 | **Internal git versioning of memory** — every fact has a commit | "When did we change our mind?" being unanswerable |
| 6 | **Negative memory + rule engine** — what *failed* is first-class | Repeating the same regression you fixed three months ago |
| 7 | **Self-model** — competencies, blind spots, brain-tasks backlog | Agent that pretends to know what it doesn't |

*Each principle in BrainCore corresponds to an explicit Postgres schema + a Go module — not a prompt-engineering trick.*

---

## How a query flows through

```mermaid
sequenceDiagram
    autonumber
    participant Agent as AI Agent (Claude/Codex/Cursor)
    participant MCP as braincore-mcp
    participant API as BrainCore API
    participant Mem as Atomic + Graph + Code
    participant Gate as Strict Mode

    Agent->>MCP: memory_recall("why did we pick JWT?")
    MCP->>API: POST /v1/recall
    API->>Mem: parallel fan-out (semantic + graph + AST)
    Mem-->>API: candidate facts (10ms)
    API->>Gate: source? confidence? temporal? contradiction?
    Gate-->>API: 4 passed, 6 dropped (1 stale, 2 unsourced, 3 contradicted)
    API-->>MCP: { facts: [...], abstained_for: [...] }
    MCP-->>Agent: structured context, NOT raw chunks
    Note over Agent,Gate: If 0 facts pass — abstain + brain task,<br/>NOT a confident hallucination.
```

The agent receives **decisions and atomic facts**, not chunks. The structure carries the metadata your prompt needs to *reason*, not just *recite*.

---

## Versus other memory tools

|  | BrainCore | Mem0 / Letta / Zep | Generic vector RAG (Qdrant + bge) |
|---|---|---|---|
| **Local-first by default** | Yes — your code never leaves the box | Hybrid / cloud-first | Self-host or cloud |
| **Strict abstain mechanism** | Yes — first-class `abstain` outcome | No — always returns top-k | No — always returns top-k |
| **Causal decision chains** | Yes — explicit schema | Partial / flat | No |
| **Negative memory (failures)** | Yes — rule engine | No | No |
| **AST-based code identity** | Yes — Tree-sitter + symbols | No | No |
| **Self-model + brain tasks** | Yes — backlog of unresolved Qs | No | No |
| **Privacy-conscious deploy** | Single Go binary, native Ollama, optional DeepSeek fallback | Cloud-native | DIY |
| **MCP integration** | First-class, ships `braincore-mcp` | Varies | Bring-your-own |

We don't claim to have invented any single principle. We claim that *all seven have to work in one system at the same time* — and that **a system where only five of seven actually work continues to lie to the user with a confident face**. There's only one way to see this — try assembling all seven into one codebase and watch what happens. That codebase is BrainCore.

---

## Architecture at a glance

```
┌─────────────────────────────────────────────────────────────────┐
│  Your machine — local-first by default                          │
│                                                                 │
│   ┌────────────┐    ┌────────────────────────────────────────┐  │
│   │ Claude Code│    │           BrainCore Daemon            │  │
│   │ Codex CLI  │MCP │                                       │  │
│   │ Cursor     │◀──▶│  ┌─────────┐  ┌─────────┐  ┌────────┐ │  │
│   │ Cline      │stdio│  │ API     │  │ Worker  │  │ Brain  │ │  │
│   └────────────┘    │  │ :8765   │  │ NATS    │  │ Events │ │  │
│                     │  └────┬────┘  └────┬────┘  └───┬────┘ │  │
│                     │       │            │           │      │  │
│                     │  ┌────▼────────────▼───────────▼────┐ │  │
│                     │  │  Postgres 18 + pgvector + RLS    │ │  │
│                     │  │  Redis 7  ·  NATS 2 JetStream    │ │  │
│                     │  └─────────────┬────────────────────┘ │  │
│                     │                │                      │  │
│                     │  ┌─────────────▼──────────────────┐   │  │
│                     │  │ Ollama (host) — bge-m3 1024d   │   │  │
│                     │  │            qwen2.5-coder:7b    │   │  │
│                     │  │   (DeepSeek cloud fallback,    │   │  │
│                     │  │    per-tenant key, encrypted)  │   │  │
│                     │  └────────────────────────────────┘   │  │
│                     └───────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

**Stack:** Go 1.25 · Postgres 18 (pgvector) · Redis 7 · NATS 2 JetStream · Ollama (bge-m3 + qwen2.5-coder:7b) · Next.js dashboard. Six binaries, all built from one repo: `braincore-saas`, `worker`, `migrate`, `seed-demo`, `braincore-mcp`, plus the `install.sh` MCP wiring.

---

## Quick start

### Option A — Cloud (Private Beta)

BrainCore is currently in **Private Beta**, onboarding design partners by invitation only.

1. Visit [getbraincore.com](https://getbraincore.com) and request a seat — describe your stack and one painful AI bug you'd want the brain to catch.
2. We send back a signed install link + API key.
3. Install the MCP wrapper, restart your IDE, ship.

### Option B — Open-source companion

The local-only memory engine is open-sourced as **[`total-agent-memory`](https://github.com/vbcherepanov/total-agent-memory)** (Apache-2.0). Same atomic-knowledge / strict-mode / decision-graph core, minus the multi-tenant SaaS layer and the hosted MCP relay.

---

## What this is *not*

- **Not a vector DB.** We use pgvector for one of seven retrieval layers; cosine similarity is a feature, not the product.
- **Not a RAG framework.** RAG is Ctrl+F with embeddings. We treat it as a *primitive*, not a model.
- **Not a chatbot.** BrainCore doesn't generate answers. It *gates* the facts the answer is grounded in.
- **Not a cloud-only SaaS.** Local-first is the default. Your code never leaves the machine unless you explicitly opt into a cloud LLM fallback.

---

## FAQ

**Does BrainCore replace my existing AI agent?**
No. It plugs in via MCP — your agent stays Claude Code / Codex / Cursor / Cline. BrainCore is the layer that decides *what facts the agent is allowed to see*.

**Will my code be sent to a cloud?**
Only if you explicitly enable the DeepSeek fallback for embeddings/chat. Default is fully local Ollama (bge-m3 + qwen2.5-coder:7b on your hardware). The dashboard, the Postgres, the NATS — all on your box.

**What's the difference between BrainCore and `total-agent-memory`?**
`total-agent-memory` is the open-source single-tenant core. **BrainCore** adds: multi-tenant SaaS layer, design-partner onboarding, hosted MCP relay, billing, RLS hardening, audit log. Same memory model, different operational story.

**Will it work with my agent stack?**
If your agent speaks MCP stdio — yes. Verified: Claude Code, Codex CLI, Cursor, Cline (VS Code). Coming: Continue, Gemini CLI.

**Why "BrainCore" and not "AntivirusForAI"?**
The marketing positioning evolved. The internal moat is anti-hallucination via 8-layer factcheck (the 8th layer is *grounding against your real source code* — not against a vector index of comments about that code). The product name kept the cognitive metaphor; the value prop is "your agent finally gets to admit it doesn't know."

**Can I self-host the full SaaS stack?**
Talk to us — [hi@getbraincore.com](mailto:hi@getbraincore.com). Self-hosted plans land after Private Beta.

**Is the codebase open?**
The companion `total-agent-memory` is Apache-2.0 today. The full SaaS stack opens progressively as we exit Private Beta.

---

## Roadmap

- [x] Atomic Knowledge Units with full lifecycle
- [x] Strict-mode gate with explicit abstain
- [x] Causal decision-graph schema
- [x] AST-based code identity (Tree-sitter, 9 languages)
- [x] MCP stdio integration (Claude Code, Codex, Cursor, Cline)
- [x] Native Ollama + DeepSeek fallback
- [x] Multi-tenant Postgres with RLS + per-tenant API keys
- [ ] **Public Beta** — open signup at [getbraincore.com](https://getbraincore.com) (Q3 2026)
- [ ] **VS Code extension** — surface brain tasks in the editor sidebar
- [ ] **GitHub App** — block PRs that contradict committed decisions
- [ ] **Browser-side agent** — same brain across IDE + ChatGPT/Claude.ai
- [ ] **On-prem k8s helm chart** — Enterprise tier

---

## Founder

Built by [**Vitalii Cherepanov**](https://www.linkedin.com/in/progerinvr/) — 18 years of senior backend, 3 years debugging AI agents in production.

- [LinkedIn](https://www.linkedin.com/in/progerinvr/) — engineering posts, weekly
- [X / @BestProgerVR](https://x.com/BestProgerVR) — short takes on AI memory
- [GitHub / @vbcherepanov](https://github.com/vbcherepanov) — open-source companion + side projects
- [hi@getbraincore.com](mailto:hi@getbraincore.com) — design-partner intake

---

## Talk to us

If you've ever shipped a patch your AI wrote against deleted code — we're building this for you.

- **Private Beta intake:** [getbraincore.com](https://getbraincore.com) → "Become a design partner"
- **Email:** [hi@getbraincore.com](mailto:hi@getbraincore.com)
- **Issues / feature requests:** GitHub Issues on this repo
- **Live demo / 30-min call:** book via the website

> *"A good AI agent isn't the one that always answers. It's the one that never confidently does the wrong thing."*

---

<p align="center">
  <a href="https://getbraincore.com">
    <img alt="getbraincore.com" src="https://img.shields.io/badge/getbraincore.com-Private%20Beta-10b981?style=for-the-badge">
  </a>
</p>
