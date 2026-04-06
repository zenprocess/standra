# standra.ai

**Open standards and tooling for AI-assisted engineering.**

The dispatch pipeline that makes AI coding agents reliable, measurable, and cheap to run at scale. Composable open-source components — pick the ones you need.

```
   TASK          CONTEXT         DISPATCH         VERIFY          LEARN
 ┌──────┐      ┌──────┐       ┌──────┐         ┌──────┐        ┌──────┐
 │ Spec │ ───▶ │Filter│  ───▶ │ Run  │   ───▶  │ Gate │  ───▶  │Mine  │
 │  +AC │      │+Rules│       │+Trace│         │+Tests│        │+Tune │
 └──────┘      └──────┘       └──────┘         └──────┘        └──────┘
   CACP         Sieeve         Switchyard       Switchyard      Afterburn
                +Axiom         +PawBench
                                +ServingCard
```

---

## The thesis

Most AI coding tools optimize a single agent doing a single task. That's the wrong unit of work.

The real bottleneck is **orchestration**: dispatching dozens of agents in parallel, giving each just the context it needs, verifying their output before merge, and learning from every dispatch to make the next one better.

`standra.ai` is the open layer for that. Each phase of the pipeline has a focused tool. Use them together or pick one.

---

![standra.ai ecosystem](diagrams/ecosystem.png)

---

## The pipeline

| Phase | What happens | Component | Repo |
|---|---|---|---|
| **Task** | Structured I/O — typed fields replace free-form prose. ~200 tokens vs ~2000. | **CACP** | [zenprocess/cacp](https://github.com/zenprocess/cacp) |
| **Context** | 5-tier submodular file assignment, task-aware retrieval, cache-aware optimization. | **Sieeve** | _open-source release pending_ |
| **Context (rules)** | Compact, tabular, LLM-native rule definition language. The format Sieeve compiles into. | **Axiom** | [zenprocess/axiom](https://github.com/zenprocess/axiom) |
| **Dispatch** | Tracker-agnostic, git-native multi-agent dispatcher. Pre-dispatch doctor, response gate, AC compliance, regression detection. | **Switchyard** _(commercial)_ | [switchyard.standra.ai](https://standra.ai/switchyard) |
| **Verify** | 4-dimensional benchmark harness — multi-turn, multi-agent, parallel dispatch with tool calling. | **PawBench** | [zenprocess/pawbench](https://github.com/zenprocess/pawbench) |
| **Verify (serving)** | Model registry for optimized LLM serving configurations. The "spec sheet" for self-hosted vLLM. | **ServingCard** | [zenprocess/servingcard](https://github.com/zenprocess/servingcard) |
| **Learn** | Mines Claude Code session history for friction, patterns, gaps. Evolves your skills via autonomous experiment loops. | **Afterburn** | [zenprocess/afterburn](https://github.com/zenprocess/afterburn) |

---

## The dispatch flow

![Dispatch pipeline](diagrams/dispatch-flow.png)

A single Switchyard dispatch end-to-end:

1. **Task** comes in (GitHub, Linear, GitLab — tracker-agnostic), wrapped in **CACP**
2. **Sieeve** compiles task-aware context (Axiom rules + 5-tier file assignment)
3. **Pre-dispatch doctor** rejects bad dispatches instantly (zero cost)
4. Agent executes in **isolated git worktree** (Claude or local Hermes/vLLM)
5. **Gate stack** runs: response gate (9 structural checks), test gate, AC compliance, regression detection, verify
6. On pass → **merge**. On block → **retry/partial**. On doctor fail → **rejected**
7. **Telemetry** is mined by **Afterburn** to improve tomorrow's dispatch

---

## Compression stack

Every layer is measured.

| Layer | Tool | Savings |
|---|---|---|
| System prompt | Axiom compiled.ctx | 5000 → 500 tokens |
| Protocol | CACP structured fields | 2000 → 200 tokens/response |
| File context | Sieeve 5-tier submodular | Task-aware: FULL/SKELETON/SUMMARY/MANIFEST/OMITTED |
| Single file | Manifest vs inline | 50–100x per file |
| Mid-trajectory | AgentDiet pruning | 40–60% context reduction |

**Validated:** 87% token savings at parity on 684 compliance runs.

---

## Why this matters

Most AI coding work today is single-shot, manual, and unmeasured. Token costs are opaque. Quality is self-reported. There's no feedback loop.

`standra.ai` makes the whole pipeline:
- **Measurable** — every dispatch produces structured telemetry
- **Cheap** — context compilation is the difference between $0.50 and $5.00 per dispatch
- **Reliable** — gates catch false-positive "done" claims before they hit your branch
- **Self-improving** — Afterburn evolves your skills based on what actually worked

---

## Get involved

- ⭐ Star the components that interest you
- 🐛 File issues — every open-source project welcomes them
- 💬 Discussions are open
- 📧 [hello@standra.ai](mailto:hello@standra.ai)

For Switchyard (commercial dispatcher), see [standra.ai/switchyard](https://standra.ai/switchyard).

---

<p align="center">
  <em>standra.ai — Open Standards for the AI-Ready Enterprise</em>
</p>
