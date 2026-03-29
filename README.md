<div align="center">
  <h1>Awesome Harness Engineering</h1>
  <p>Curated resources, patterns, and templates for building reliable AI agent harnesses.</p>
  <p>
    <a href="https://awesome.re"><img src="https://awesome.re/badge.svg" alt="Awesome"></a>
    <a href="LICENSE"><img src="https://img.shields.io/badge/License-CC0-lightgrey.svg" alt="License: CC0"></a>
  </p>
  <p>
    <a href="https://zdoc.ai/de/ai-boost/awesome-harness-engineering">Deutsch</a> |
    <a href="https://zdoc.ai/en/ai-boost/awesome-harness-engineering">English</a> |
    <a href="https://zdoc.ai/es/ai-boost/awesome-harness-engineering">Español</a> |
    <a href="https://zdoc.ai/fr/ai-boost/awesome-harness-engineering">Français</a> |
    <a href="https://zdoc.ai/ja/ai-boost/awesome-harness-engineering">日本語</a> |
    <a href="https://zdoc.ai/ko/ai-boost/awesome-harness-engineering">한국어</a> |
    <a href="https://zdoc.ai/pt/ai-boost/awesome-harness-engineering">Português</a> |
    <a href="https://zdoc.ai/ru/ai-boost/awesome-harness-engineering">Русский</a> |
    <a href="https://zdoc.ai/zh/ai-boost/awesome-harness-engineering">中文</a>
  </p>
</div>

**Harness engineering** is the discipline of designing the scaffolding — context delivery, tool interfaces, planning artifacts, verification loops, memory systems, and sandboxes — that surrounds an AI agent and determines whether it succeeds or fails on real tasks.

This list focuses on the *harness*, not the model. Every component here exists because the model can't do it alone — and the best harnesses are designed knowing those components will become unnecessary as models improve.

---

## Contents

- [📐 Foundations](#foundations)
- [🧩 Design Primitives](#design-primitives)
- [🔍 Reference Implementations](#reference-implementations)
- [🔒 Security, Sandbox & Permissions](#security-sandbox--permissions)
- [✅ Evals & Verification](#evals--verification)
- [📋 Templates](#templates)
- [📚 Related Awesome Lists](#related-awesome-lists)
- [🤝 Contributing](#contributing)

---

## Foundations

Canonical essays that define what harness engineering is and why it matters.

- [Harness Engineering](https://openai.com/index/harness-engineering/) — OpenAI's framing of harness engineering as a discipline: how to design the scaffolding that lets Codex and similar agents operate reliably in an agent-first world.
- [Unrolling the Codex Agent Loop](https://openai.com/index/unrolling-the-codex-agent-loop/) — OpenAI's detailed breakdown of the Codex agent loop, exposing each harness component and where it can be improved.
- [Run Long-Horizon Tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex/) — OpenAI's practice guide for long-horizon task planning: introduces Plan.md, Implement.md, Documentation.md as reusable harness artifacts.
- [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — Anthropic's foundational guide on agent architecture, covering when to use workflows vs. agents and how to compose primitives.
- [Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps) — Anthropic's engineering blog on designing harnesses for sustained, multi-session development tasks. Key insight: every harness component assumes the model can't do something; those assumptions expire.
- [Writing Effective Tools for Agents](https://www.anthropic.com/engineering/writing-effective-tools-for-agents) — Anthropic's guide on tool interface design: naming, schemas, error surfaces, and the principle that tool design is agent UX.
- [Beyond Permission Prompts](https://www.anthropic.com/engineering/beyond-permission-prompts) — Anthropic on building structured permission and authorization systems into agent harnesses instead of relying on natural-language permission text.
- [Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) — Anthropic's framework for evaluating agent behavior: what to measure, how to build eval harnesses, and why unit-test-style evals fail for agents.
- [What is an AI Agent?](https://www.anthropic.com/research/what-is-an-agent) — Anthropic's definitional piece, useful for anchoring harness design decisions to a clear model of what an agent actually is.

---

## Design Primitives

Harness components organized by the problem they solve, not by vendor.

### Agent Loop

- [Unrolling the Codex Agent Loop](https://openai.com/index/unrolling-the-codex-agent-loop/) — The canonical decomposition of what happens inside one agent loop iteration: observe, plan, act, verify.

### Planning & Task Decomposition

- [Run Long-Horizon Tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex/) — Introduces milestone-based planning artifacts (Plan.md, Implement.md) as harness-level state.
- [Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps) — Multi-session planning, progress tracking, and the role of persistent planning documents.

### Context Delivery & Compaction

- [Harness Engineering](https://openai.com/index/harness-engineering/) — How to structure context windows for agents: what to include, what to exclude, and how context shape affects agent behavior.

### Tool Design

- [Writing Effective Tools for Agents](https://www.anthropic.com/engineering/writing-effective-tools-for-agents) — Tool naming, schema design, error messages, and return value conventions that make agents more reliable.

### Skills & MCP

- [Model Context Protocol](https://modelcontextprotocol.io/introduction) — Anthropic's open protocol for connecting agents to external tools, data sources, and services in a standardized way.

### Permissions & Authorization

- [Beyond Permission Prompts](https://www.anthropic.com/engineering/beyond-permission-prompts) — Structured authorization patterns for agents: how to give agents the right permissions without relying on prompt-level trust.

### Memory & State

- [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — Covers in-context, external, and procedural memory patterns as harness-level concerns.

### Task Runners & Orchestration

- [Harness Engineering](https://openai.com/index/harness-engineering/) — How task runners fit into the harness: queueing, parallelism, and progress reporting.

### Verification & CI Integration

- [Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) — How to build verification into the harness loop, not just as a post-hoc eval.

---

## Reference Implementations

Real repositories worth studying — each with a note on *why* it's worth your time.

### Tutorials & Educational

- [shareAI-lab/learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) — Step-by-step deconstruction of Claude Code as an agent harness (s01–s12). Best resource for understanding how agent loop, tool use, skills, context compaction, and task management compose in practice. ![Stars](https://img.shields.io/github/stars/shareAI-lab/learn-claude-code?style=flat-square&label=★&color=yellow)
- [AutoJunjie/awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness) — Curated list organized into Full Lifecycle Platforms, Task Runners, Agent Runtimes, Coding Agents. Close to this list's scope; good complementary reference. ![Stars](https://img.shields.io/github/stars/AutoJunjie/awesome-agent-harness?style=flat-square&label=★&color=yellow)

### Generators & Meta-Harnesses

- [revfactory/harness](https://github.com/revfactory/harness) — A meta-skill that generates domain-specific agent teams and the skills they use. Good example of harness-as-code, where the harness itself is produced by an agent. ![Stars](https://img.shields.io/github/stars/revfactory/harness?style=flat-square&label=★&color=yellow)

### Demo Harnesses

- [coleam00/your-claude-engineer](https://github.com/coleam00/your-claude-engineer) — Agent harness with Slack, GitHub, and Linear integrations. Useful reference for how real-world tool wiring works inside a harness. ![Stars](https://img.shields.io/github/stars/coleam00/your-claude-engineer?style=flat-square&label=★&color=yellow)

### Adjacent Collections

- [jiji262/awesome-harness-engineering](https://github.com/jiji262/awesome-harness-engineering) — Focuses on platform delivery governance, IDP, GitOps, and AI-native engineering. Overlaps with this list on the platform engineering side; more Harness-the-company oriented. ![Stars](https://img.shields.io/github/stars/jiji262/awesome-harness-engineering?style=flat-square&label=★&color=yellow)

---

## Security, Sandbox & Permissions

- [Beyond Permission Prompts](https://www.anthropic.com/engineering/beyond-permission-prompts) — The authoritative resource on moving from prompt-level permission grants to structured authorization in the harness.
- [Model Context Protocol — Authorization](https://modelcontextprotocol.io/specification/2025-11-05/basic/authorization) — MCP's specification for OAuth-based authorization flows when agents access external services.
- [AI Harness Scorecard](https://github.com/anthropics/ai-harness-scorecard) — Scores repositories on AI harness safeguards. Useful checklist for auditing your own harness's security posture.

---

## Evals & Verification

- [Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) — Anthropic's comprehensive guide to agent evaluation: trajectory evals, outcome evals, and how to build eval harnesses that are themselves reliable.
- [SWE-bench](https://www.swebench.com) — The canonical benchmark for coding agents. Essential reference for understanding what "verified working" means for harness outputs.

---

## Templates

Reusable starting points for harness artifacts. Copy and adapt.

| Template | Purpose |
|---|---|
| [`templates/AGENTS.md`](templates/AGENTS.md) | Project-level agent instructions: conventions, constraints, tool permissions |
| [`templates/PLAN.md`](templates/PLAN.md) | Task planning artifact with milestones and verification gates |
| [`templates/IMPLEMENT.md`](templates/IMPLEMENT.md) | Implementation log: decisions, deviations, open questions |
| [`templates/HARNESS_CHECKLIST.md`](templates/HARNESS_CHECKLIST.md) | Review checklist before shipping a harness to production |

---

## Related Awesome Lists

Lists that cover adjacent territory — overlapping but not identical scope.

- [Awesome Context Engineering](https://github.com/Meirtz/Awesome-Context-Engineering) — Comprehensive survey on context engineering: prompt engineering, RAG, context window management, production AI systems.
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — Curated resources, tools, and workflows specifically for Claude Code users.
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers) — Comprehensive list of MCP servers for extending agents with external capabilities.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

**What belongs here:** Resources that address a specific harness engineering problem (context, tools, planning, permissions, memory, verification, sandboxing). Each addition should include a 1–2 sentence note explaining *why* it's worth including — this is an opinionated list, not a directory.

**What doesn't belong here:** General AI/ML papers, model benchmarks unrelated to agent harnesses, tutorials on using specific models, product marketing.

---

## License

[CC0](LICENSE) — public domain dedication.
