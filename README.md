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
  - [🔄 Agent Loop](#agent-loop)
  - [🗺️ Planning & Task Decomposition](#planning--task-decomposition)
  - [📦 Context Delivery & Compaction](#context-delivery--compaction)
  - [🔧 Tool Design](#tool-design)
  - [🔌 Skills & MCP](#skills--mcp)
  - [🛡️ Permissions & Authorization](#permissions--authorization)
  - [🧠 Memory & State](#memory--state)
  - [⚙️ Task Runners & Orchestration](#task-runners--orchestration)
  - [✔️ Verification & CI Integration](#verification--ci-integration)
  - [👁️ Observability & Tracing](#observability--tracing)
  - [🧑‍💼 Human-in-the-Loop](#human-in-the-loop)
- [🔍 Reference Implementations](#reference-implementations)
  - [🎓 Tutorials & Educational](#tutorials--educational)
  - [🏭 Generators & Meta-Harnesses](#generators--meta-harnesses)
  - [🧪 Demo Harnesses](#demo-harnesses)
  - [🗂️ Adjacent Collections](#adjacent-collections)
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
- [Agent Development Kit: Making it easy to build multi-agent applications](https://developers.googleblog.com/en/agent-development-kit-easy-to-build-multi-agent-applications/) — Google's announcement and design rationale for ADK: explains the multi-agent topology, tool registration model, and eval pipeline that shaped their framework. Complements the Anthropic/OpenAI framing with Google's production perspective.
- [Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) — Martin Fowler's synthesis of what harness engineering practice looks like: three interlocking systems — context engineering (curating what the agent knows), architectural constraints (deterministic linters and structural tests), and entropy management (periodic agents that repair documentation drift). The "humans on the loop" framing — harness engineers who design and maintain agent environments rather than inspecting individual outputs — is the clearest conceptual map of what the discipline actually entails.

---

## Design Primitives

Harness components organized by the problem they solve, not by vendor.

### Agent Loop

- [ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629) — The foundational paper defining the Thought/Action/Observation loop structure that underlies virtually every agent harness. Required reading for understanding why the loop is structured the way it is and where each harness component maps onto the reasoning-acting cycle.
- [Unrolling the Codex Agent Loop](https://openai.com/index/unrolling-the-codex-agent-loop/) — The canonical decomposition of what happens inside one agent loop iteration: observe, plan, act, verify.
- [LangGraph — Low Level Concepts](https://langchain-ai.github.io/langgraph/concepts/low_level/) — Models the agent loop explicitly as a directed graph with typed state, conditional edges, and checkpointing. The most concrete engineering treatment of loop control flow: how to implement termination conditions, branch on tool results, and persist mid-loop state for resumption.
- [Unlocking the Codex Harness: How We Built the App Server](https://openai.com/index/unlocking-the-codex-harness/) — OpenAI's engineering deep-dive into the Item/Turn/Thread protocol (JSON-RPC/JSONL over stdio) that exposes the Codex harness to every client surface. The most direct first-party account of why approval flows, streaming diffs, and thread persistence demand a purpose-built protocol — and why MCP's tool-oriented model proved insufficient for these requirements.
- [Extended Thinking — Claude API Docs](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking) — The harness-critical reference for integrating extended thinking into agent loops: `budget_tokens` controls reasoning depth per turn, thinking blocks **must be preserved** when passing tool results back (omitting them silently breaks multi-step reasoning), and thinking mode cannot change mid-turn. Essential before wiring extended thinking into any tool-use loop.
- [Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/) — LangChain's case study showing harness-only changes moved their coding agent from rank 30 to top 5 on Terminal Bench 2.0 with no model swap: structured verification loops, context injection (directory maps + time budget warnings), loop-detection middleware, and a "reasoning sandwich" concentrating maximum thinking at planning and verification phases. The most concrete published demonstration that harness design is the primary performance lever, not model capability.

### Planning & Task Decomposition

- [Run Long-Horizon Tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex/) — Introduces milestone-based planning artifacts (Plan.md, Implement.md) as harness-level state.
- [Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps) — Multi-session planning, progress tracking, and the role of persistent planning documents.
- [Plan-and-Execute Agents](https://blog.langchain.com/plan-and-execute-agents/) — The canonical engineering write-up separating planning from execution as distinct harness layers: a planner LLM generates the step list once; an executor agent works through it, replanning only when needed. Defines the pattern that most modern task-decomposition harnesses follow.
- [microsoft/TaskWeaver](https://github.com/microsoft/TaskWeaver) — Code-first task decomposition framework with a planner/executor split and a plugin system for injecting domain knowledge into the planning layer. The most complete reference implementation of plan-then-execute with stateful task tracking. ![Stars](https://img.shields.io/github/stars/microsoft/TaskWeaver?style=flat-square&label=★&color=yellow)
- [LATS: Language Agent Tree Search](https://arxiv.org/abs/2310.04406) — Unifies reasoning, acting, and planning via Monte Carlo Tree Search over agent trajectories. Directly informs harness design: external tool feedback as tree-search signals, trajectory backtracking on failure, and depth-bounded exploration make this the most actionable planning research for harnesses with real environment interaction.

### Context Delivery & Compaction

- [Harness Engineering](https://openai.com/index/harness-engineering/) — How to structure context windows for agents: what to include, what to exclude, and how context shape affects agent behavior.
- [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic's systematic guide to managing the full context state—system prompts, tools, MCP, and message history—as a finite, curated resource. Reframes harness design as "what configuration of context produces the desired behavior?" rather than just prompt wording.
- [Compaction — Claude API Docs](https://platform.claude.com/docs/en/build-with-claude/compaction) — Anthropic's reference for server-side context compaction: automatically summarizes older context when approaching the window limit. Reduced token consumption by 84% in a 100-turn web search eval while allowing agents to complete workflows that would otherwise hit context limits.
- [LLMLingua](https://github.com/microsoft/LLMLingua) — Microsoft Research's prompt compression toolkit (up to 20x compression, minimal performance loss) that can be embedded as a preprocessing step in the context delivery layer. LLMLingua-2 adds 3–6x speed gains, making it viable for latency-sensitive agent loops. ![Stars](https://img.shields.io/github/stars/microsoft/LLMLingua?style=flat-square&label=★&color=yellow)
- [Prompt Caching — Claude API Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching) — The most effective harness-level cost lever: cache repeated system prompts, tool definitions, and long documents across requests. Explains where to place `cache_control` breakpoints for maximum reuse across multi-turn agent sessions.

### Tool Design

- [Writing Effective Tools for Agents](https://www.anthropic.com/engineering/writing-effective-tools-for-agents) — Tool naming, schema design, error messages, and return value conventions that make agents more reliable.
- [Tool Use — Claude API Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview) — Authoritative reference for client vs. server tool execution models, strict schema enforcement, and tool_result error signaling. The distinction between client-side and server-side tool execution is a foundational harness architecture decision.
- [Function Calling — OpenAI Docs](https://platform.openai.com/docs/guides/function-calling) — Defines the de facto industry-standard JSON Schema conventions for tool definitions and parallel function calling. Essential reading before designing a tool interface that needs to work across multiple models.
- [Tool Annotations as Risk Vocabulary](https://blog.modelcontextprotocol.io/posts/2026-03-16-tool-annotations/) — The MCP team's definitive post on the four tool annotation hints (`readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`) as inputs to harness permission decisions, not enforced contracts. The "lethal trifecta" — private data access + untrusted content exposure + external communication — is the most actionable framing for why single-tool safety analysis misses the risk that emerges from tool *combinations*.
- [outlines](https://github.com/dottxt-ai/outlines) — Constrains token sampling via regex/CFG/JSON Schema at the decoding layer, guaranteeing structured output without model fine-tuning. The right solution when you need OpenAI Structured Outputs-equivalent reliability from a locally deployed or open-weight model. ![Stars](https://img.shields.io/github/stars/dottxt-ai/outlines?style=flat-square&label=★&color=yellow)
- [instructor](https://python.useinstructor.com/) — Maps Pydantic models directly to structured LLM extraction with built-in retry and validation-error feedback loops. Turns tool call output parsing from ad-hoc JSON handling into type-safe data models, eliminating an entire class of harness parsing bugs. ![Stars](https://img.shields.io/github/stars/instructor-ai/instructor?style=flat-square&label=★&color=yellow)

### Skills & MCP

- [Model Context Protocol](https://modelcontextprotocol.io/introduction) — Anthropic's open protocol for connecting agents to external tools, data sources, and services in a standardized way.
- [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) — Anthropic's official reference MCP server implementations (GitHub, Slack, Postgres, Puppeteer, etc.). The authoritative source for understanding correct MCP server structure before building your own. ![Stars](https://img.shields.io/github/stars/modelcontextprotocol/servers?style=flat-square&label=★&color=yellow)
- [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) — Browser automation via accessibility tree snapshots rather than screenshots, dramatically reducing token cost. The canonical example of structured tool output design in an MCP server. ![Stars](https://img.shields.io/github/stars/microsoft/playwright-mcp?style=flat-square&label=★&color=yellow)
- [A2A Protocol](https://github.com/a2aproject/A2A) — Google's open Agent-to-Agent protocol: JSON-RPC over HTTP(S)/SSE with Agent Card service discovery and a task/message/artifact communication model. The emerging standard for cross-framework agent interoperability in multi-agent harnesses. ![Stars](https://img.shields.io/github/stars/a2aproject/A2A?style=flat-square&label=★&color=yellow)
- [MCP Inspector](https://github.com/modelcontextprotocol/inspector) — Interactive debugging UI for MCP servers: inspect tool definitions, send test calls, and validate responses without wiring up a full agent. The essential development tool for anyone building or integrating MCP servers into a harness. ![Stars](https://img.shields.io/github/stars/modelcontextprotocol/inspector?style=flat-square&label=★&color=yellow)
- [Shell + Skills + Compaction: Tips for Long-Running Agents](https://developers.openai.com/blog/skills-shell-tips) — OpenAI's engineering guide to three production harness primitives: versioned Skill bundles (SKILL.md manifest; routing accuracy improved 73%→85% by adding negative examples), a managed shell container for durable tool execution, and server-side compaction via explicit `/responses/compact` endpoint. The most concrete first-party documentation of skills-based routing and compaction published in 2026.
- [Composio](https://github.com/ComposioHQ/composio) — Wraps 250+ SaaS APIs (GitHub, Slack, Linear, Notion, etc.) as agent-ready actions with managed OAuth, so tool integration becomes a one-line import rather than a custom harness component per service. The fastest path from "the agent needs to call an external API" to a production-grade, authenticated tool. ![Stars](https://img.shields.io/github/stars/ComposioHQ/composio?style=flat-square&label=★&color=yellow)

### Permissions & Authorization

- [Beyond Permission Prompts](https://www.anthropic.com/engineering/beyond-permission-prompts) — Structured authorization patterns for agents: how to give agents the right permissions without relying on prompt-level trust.
- [OWASP LLM06:2025 — Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/) — OWASP's authoritative definition of the "excessive agency" risk: over-provisioned functions, unnecessary permissions, and missing approval mechanisms. The standard checklist for auditing harness permission scope against principle of least privilege.
- [Claude Code Auto Mode: A Safer Way to Skip Permissions](https://www.anthropic.com/engineering/claude-code-auto-mode) — Anthropic's engineering post on replacing approval fatigue (users approve 93% of prompts, making approvals meaningless) with a two-stage classifier: fast single-token gate first, chain-of-thought reasoning only on flagged actions. The design decisions — stripping assistant messages to prevent the agent from rationalizing dangerous actions, deny-and-continue recovery instead of halt — are the reference design for safe-by-default headless agent permissions.
- [Claude Agent SDK — Configure Permissions](https://platform.claude.com/docs/en/agent-sdk/permissions) — The most concrete reference for harness permission architecture: five-layer evaluation order (hooks → deny rules → permission mode → allow rules → canUseTool), `allowedTools`/`disallowedTools` declarative scoping, and four permission modes including `dontAsk` (deny-by-default for headless agents). The subagent inheritance warning for `bypassPermissions` alone is worth reading before any multi-agent deployment.

### Memory & State

- [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — Covers in-context, external, and procedural memory patterns as harness-level concerns.
- [Letta (MemGPT)](https://github.com/letta-ai/letta) — The reference architecture for stateful agents: three-tier memory (core / archival / recall) maps directly to harness state management design. Their [agent loop redesign post](https://www.letta.com/blog/letta-v1-agent) is the most thorough public analysis of how memory structure shapes the harness. ![Stars](https://img.shields.io/github/stars/letta-ai/letta?style=flat-square&label=★&color=yellow)
- [mem0](https://github.com/mem0ai/mem0) — Drop-in universal memory layer (YC-backed, AWS Agent SDK's exclusive memory provider) that handles cross-session retention without custom harness-level state management code. Lowest integration cost for production-grade persistent memory. ![Stars](https://img.shields.io/github/stars/mem0ai/mem0?style=flat-square&label=★&color=yellow)
- [Zep](https://github.com/getzep/zep) — Purpose-built agent memory store with automatic conversation summarization, entity extraction, and semantic search over session history. Solves long-session context overflow at the memory layer rather than forcing the harness to manage trimming manually. ![Stars](https://img.shields.io/github/stars/getzep/zep?style=flat-square&label=★&color=yellow)

### Task Runners & Orchestration

- [Harness Engineering](https://openai.com/index/harness-engineering/) — How task runners fit into the harness: queueing, parallelism, and progress reporting.
- [Building a C Compiler with a Team of Parallel Claudes](https://www.anthropic.com/engineering/building-c-compiler) — Anthropic's account of coordinating 16 Claude instances in parallel on a shared git repo without a central orchestrator: agents claim tasks via files in `current_tasks/`, git forces collision resolution naturally, and a continuous restart loop spawns fresh sessions that resume where predecessors left off. Key harness lesson: verbose test output pollutes agent context — the feedback loop must emit only a few summary lines, log detail to file.
- [LiteLLM](https://github.com/BerriAI/litellm) — Unified proxy and SDK that routes to 100+ LLM providers behind a single OpenAI-compatible interface, with a Router handling retry/fallback across deployments, per-project cost and rate-limit tracking, and OTEL callback integrations. The right infrastructure layer when your harness needs provider resilience (automatic failover on 429/500 errors), budget guardrails, or the ability to swap models without touching orchestration code. ![Stars](https://img.shields.io/github/stars/BerriAI/litellm?style=flat-square&label=★&color=yellow)
- [LangGraph](https://github.com/langchain-ai/langgraph) — Graph-based state machine framework for multi-agent harnesses: models supervisor/subagent topologies, error-recovery branches, and checkpoint persistence as first-class primitives. The most widely adopted harness orchestration layer in production. ![Stars](https://img.shields.io/github/stars/langchain-ai/langgraph?style=flat-square&label=★&color=yellow)
- [OpenAI Agents SDK](https://github.com/openai/openai-agents-python) — Lightweight multi-agent framework built around handoffs and guardrails; the production successor to Swarm. Complements LangGraph for harnesses where delegation patterns are simpler than full graph orchestration. ![Stars](https://img.shields.io/github/stars/openai/openai-agents-python?style=flat-square&label=★&color=yellow)
- [Google ADK](https://github.com/google/adk-python) — Google's code-first agent framework with built-in multi-agent orchestration, tool registration, session state, and eval pipeline. Its `Runner` and `AgentTool` patterns are the reference implementation for wrapping sub-agents as tools in a larger harness. ![Stars](https://img.shields.io/github/stars/google/adk-python?style=flat-square&label=★&color=yellow)
- [AutoGen](https://github.com/microsoft/autogen) — Microsoft's multi-agent conversation framework with a complete AgentChat layer covering agent loop, tool integration, termination conditions, and human-in-the-loop. The most comprehensive open-source reference for large-scale multi-agent harness design. ![Stars](https://img.shields.io/github/stars/microsoft/autogen?style=flat-square&label=★&color=yellow)
- [CrewAI](https://github.com/crewAIInc/crewAI) — Dual-layer harness orchestration: Crew handles autonomous agent delegation, Flow provides event-driven deterministic control (branching + shared Pydantic state). The clearest open-source example of mixing autonomous and scripted execution in the same harness. ![Stars](https://img.shields.io/github/stars/crewAIInc/crewAI?style=flat-square&label=★&color=yellow)
- [PydanticAI](https://github.com/pydantic/pydantic-ai) — Type-safe agent framework where tool definitions, parameters, and return values are Pydantic models. Shifts "agent output doesn't match expected structure" from a runtime bug to a type-check failure; its `RunContext` dependency injection pattern is the reference design for passing session-scoped objects through the harness without global state. ![Stars](https://img.shields.io/github/stars/pydantic/pydantic-ai?style=flat-square&label=★&color=yellow)

### Verification & CI Integration

- [Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) — How to build verification into the harness loop, not just as a post-hoc eval.
- [promptfoo](https://github.com/promptfoo/promptfoo) — YAML-driven LLM testing framework with LLM-as-judge, assertion DSL, and native CI integration. The most practical tool for adding agent output regression tests to a PR pipeline without writing a test harness from scratch. ![Stars](https://img.shields.io/github/stars/promptfoo/promptfoo?style=flat-square&label=★&color=yellow)
- [AgentBench](https://github.com/THUDM/AgentBench) — Multi-environment agent benchmark (OS, DB, web, code) with a structured eval pipeline. Worth studying for its environment isolation design and task definition format when building custom eval environments for your harness. ![Stars](https://img.shields.io/github/stars/THUDM/AgentBench?style=flat-square&label=★&color=yellow)

### Observability & Tracing

- [OpenLLMetry](https://github.com/traceloop/openllmetry) — OpenTelemetry-based instrumentation for LLM calls and agent steps: adds trace spans to every inference and tool call without modifying business logic. The cleanest way to bring the existing OTEL ecosystem (Grafana, Datadog, Jaeger) to a harness. ![Stars](https://img.shields.io/github/stars/traceloop/openllmetry?style=flat-square&label=★&color=yellow)
- [Arize Phoenix](https://github.com/Arize-ai/phoenix) — Self-hostable trace UI and eval runtime for agent workflows. Lets harness engineers audit and replay every reasoning step and tool call offline, without sending data to a third-party cloud. ![Stars](https://img.shields.io/github/stars/Arize-ai/phoenix?style=flat-square&label=★&color=yellow)
- [Langfuse](https://github.com/langfuse/langfuse) — The most widely adopted self-hostable LLM observability platform: traces every agent step, manages prompt versions, and runs evals in one tool. Preferred over cloud-only alternatives when data residency or cost control is a constraint. ![Stars](https://img.shields.io/github/stars/langfuse/langfuse?style=flat-square&label=★&color=yellow)
- [Weights & Biases Weave](https://github.com/wandb/weave) — W&B's tracing and eval layer purpose-built for agent workflows: automatic call graph capture, dataset versioning, and LLM-as-judge evals that integrate directly with the wandb experiment tracking ecosystem. ![Stars](https://img.shields.io/github/stars/wandb/weave?style=flat-square&label=★&color=yellow)
- [OTel GenAI Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/) — OpenTelemetry's standard attribute names for GenAI spans (`gen_ai.system`, `gen_ai.request.model`, etc.). The naming baseline that makes harness traces portable across any OTEL-compatible backend.

### Human-in-the-Loop

- [LangGraph — Human-in-the-Loop Concepts](https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop/) — Systematic treatment of interrupt, breakpoint, and approve patterns: how to pause an agent mid-loop, persist state, and resume after human review. Directly addresses the harness engineering challenge of inserting human gates into long-running workflows.
- [AutoGen — Human-in-the-Loop](https://microsoft.github.io/autogen/0.2/docs/tutorial/human-in-the-loop/) — Explains `human_input_mode` (NEVER / TERMINATE / ALWAYS) and the UserProxyAgent as an approval gate. The most concrete implementation reference for adding human review nodes to a multi-agent conversation harness.
- [Claude Agent SDK — Handle Approvals and User Input](https://platform.claude.com/docs/en/agent-sdk/user-input) — The most complete implementation reference for HITL mechanics: `canUseTool` callback pauses execution at every tool request with allow/deny/approve-with-changes/suggest-alternative response shapes; `AskUserQuestion` surfaces structured clarifications mid-task; streaming input enables mid-execution redirects. The "approve with changes" pattern — modifying tool input before execution — is the reference design for safe-by-default harnesses that don't simply block or permit.
- [Humans and Agents in Software Engineering Loops](https://martinfowler.com/articles/exploring-gen-ai/humans-and-agents.html) — Martin Fowler defines three human-involvement postures — humans outside, in, or on the agent loop — and argues that "humans on the loop" (maintaining the harness rather than reviewing individual outputs) is the only approach that scales with agent throughput. The "agentic flywheel" section — where agents are directed to evaluate results and recommend harness improvements — is the clearest articulation of how HITL evolves from a gate into a feedback mechanism.

---

## Reference Implementations

Real repositories worth studying — each with a note on *why* it's worth your time.

### Tutorials & Educational

- [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks) — Anthropic's official notebook collection covering orchestrator-worker patterns, parallel tool calling, programmatic tool calling (PTC), context compaction, and Agent SDK examples. The `patterns/agents/` directory is the reference implementation of every orchestration pattern described in *Building Effective Agents*. ![Stars](https://img.shields.io/github/stars/anthropics/claude-cookbooks?style=flat-square&label=★&color=yellow)
- [huggingface/smolagents](https://github.com/huggingface/smolagents) — HuggingFace's deliberately minimal agent library (~1,000 lines of core code): the entire harness — tool validation, memory, monitoring, sandbox isolation (E2B, Docker, Pyodide) — is readable in an afternoon. The code-agent pattern (model writes Python that calls tools, eliminating JSON round-trips) is a concrete alternative loop design worth understanding. ![Stars](https://img.shields.io/github/stars/huggingface/smolagents?style=flat-square&label=★&color=yellow)
- [shareAI-lab/learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) — Step-by-step deconstruction of Claude Code as an agent harness (s01–s12). Best resource for understanding how agent loop, tool use, skills, context compaction, and task management compose in practice. ![Stars](https://img.shields.io/github/stars/shareAI-lab/learn-claude-code?style=flat-square&label=★&color=yellow)
- [AutoJunjie/awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness) — Curated list organized into Full Lifecycle Platforms, Task Runners, Agent Runtimes, Coding Agents. Close to this list's scope; good complementary reference. ![Stars](https://img.shields.io/github/stars/AutoJunjie/awesome-agent-harness?style=flat-square&label=★&color=yellow)
- [Skill Issue: Harness Engineering for Coding Agents](https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents) — Practitioners' guide covering all harness configuration points for coding agents: system prompts, MCP tool selection, skills for progressive disclosure, sub-agents as context firewalls, hooks for deterministic control, and back-pressure verification. The central argument — that most agent failures are configuration problems, not model limitations — and the heuristic to minimize tool exposure (too many MCP servers bloat context) make this the most actionable single-page synthesis of what harness engineering means in practice for a coding agent.

### Generators & Meta-Harnesses

- [Claude Agent SDK](https://platform.claude.com/docs/en/agent-sdk/overview) — Anthropic's official SDK that exposes Claude Code's entire harness as a programmable API: built-in tool execution loop, `PreToolUse`/`PostToolUse` hooks for interception, subagent definitions, `allowedTools` permission control, and session resumption. The highest-leverage starting point for building a production harness — you inherit the entire tool execution layer rather than implementing it. ![Stars](https://img.shields.io/github/stars/anthropics/claude-agent-sdk-python?style=flat-square&label=★&color=yellow)
- [revfactory/harness](https://github.com/revfactory/harness) — A meta-skill that generates domain-specific agent teams and the skills they use. Good example of harness-as-code, where the harness itself is produced by an agent. ![Stars](https://img.shields.io/github/stars/revfactory/harness?style=flat-square&label=★&color=yellow)

### Demo Harnesses

- [Anthropic Computer Use Demo](https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo) — Anthropic's reference harness for the screenshot-action loop: defines the `screenshot`, `bash`, and `text_editor` tool interface that makes desktop/browser control work. Essential reading before building any harness where the agent's primary sensory input is a rendered screen rather than structured API responses. ![Stars](https://img.shields.io/github/stars/anthropics/anthropic-quickstarts?style=flat-square&label=★&color=yellow)
- [coleam00/your-claude-engineer](https://github.com/coleam00/your-claude-engineer) — Agent harness with Slack, GitHub, and Linear integrations. Useful reference for how real-world tool wiring works inside a harness. ![Stars](https://img.shields.io/github/stars/coleam00/your-claude-engineer?style=flat-square&label=★&color=yellow)
- [OpenHands](https://github.com/OpenHands/OpenHands) — The most architecturally complete open-source coding agent: Runtime/Sandbox isolation, EventStream message bus, and Agent Controller are a three-layer harness design worth studying for production deployments. ![Stars](https://img.shields.io/github/stars/OpenHands/OpenHands?style=flat-square&label=★&color=yellow)
- [browser-use](https://github.com/browser-use/browser-use) — Minimal browser-automation agent harness with clean separation of tool registration, DOM state injection, action loop, and error recovery. Small codebase, clear structure — the best "minimal viable harness" reference for understanding core loop mechanics. ![Stars](https://img.shields.io/github/stars/browser-use/browser-use?style=flat-square&label=★&color=yellow)
- [SWE-agent](https://github.com/SWE-agent/SWE-agent) — Coding agent whose Agent-Computer Interface (ACI) — purpose-built file viewer, search, and editor tools with explicit state constraints and error feedback — is the reference design for adapting a tool interface to a specific task domain rather than using generic bash. ![Stars](https://img.shields.io/github/stars/SWE-agent/SWE-agent?style=flat-square&label=★&color=yellow)
- [Aider](https://github.com/Aider-AI/aider) — AI pair-programmer harness with an Architect mode that splits planning (one LLM) from coding (another), and git-aware tooling that uses version control as the undo mechanism instead of custom state rollback. The best reference for multi-file editing tool design and planner/coder layer separation. ![Stars](https://img.shields.io/github/stars/Aider-AI/aider?style=flat-square&label=★&color=yellow)

### Adjacent Collections

- [jiji262/awesome-harness-engineering](https://github.com/jiji262/awesome-harness-engineering) — Focuses on platform delivery governance, IDP, GitOps, and AI-native engineering. Overlaps with this list on the platform engineering side; more Harness-the-company oriented. ![Stars](https://img.shields.io/github/stars/jiji262/awesome-harness-engineering?style=flat-square&label=★&color=yellow)

---

## Security, Sandbox & Permissions

- [Beyond Permission Prompts](https://www.anthropic.com/engineering/beyond-permission-prompts) — The authoritative resource on moving from prompt-level permission grants to structured authorization in the harness.
- [Model Context Protocol — Authorization](https://modelcontextprotocol.io/specification/2025-11-05/basic/authorization) — MCP's specification for OAuth-based authorization flows when agents access external services.
- [AI Harness Scorecard](https://github.com/anthropics/ai-harness-scorecard) — Scores repositories on AI harness safeguards. Useful checklist for auditing your own harness's security posture.
- [E2B](https://github.com/e2b-dev/E2B) — Firecracker microVM sandboxes purpose-built for agent tool loops: ~150ms cold start, Python/JS SDKs, open source. The clearest reference implementation of "code execution as a harness primitive" rather than a CI system bolted on. ![Stars](https://img.shields.io/github/stars/e2b-dev/E2B?style=flat-square&label=★&color=yellow)
- [tldrsec/prompt-injection-defenses](https://github.com/tldrsec/prompt-injection-defenses) — The most complete catalog of practical prompt injection defenses (input validation, tool output sanitization, canary tokens, etc.). Functions as a design checklist for hardening trust boundaries in any agent harness. ![Stars](https://img.shields.io/github/stars/tldrsec/prompt-injection-defenses?style=flat-square&label=★&color=yellow)
- [Prompt Injection — Simon Willison's Series](https://simonwillison.net/series/prompt-injection/) — The most thorough public writing on why indirect prompt injection is uniquely dangerous for agent harnesses: agents actively consume untrusted external content (emails, web pages, tool outputs) that can hijack their actions. Essential for understanding the attack surface before designing trust boundaries.
- [OWASP LLM01:2025 — Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) — OWASP's authoritative classification of direct and indirect prompt injection risks. Complements the tldrsec defense catalog: use this to define the threat model, use tldrsec to select countermeasures.
- [Daytona](https://github.com/daytonaio/daytona) — OCI-container sandboxes with sub-90ms startup, built-in Git operations, LSP support, and indefinite state persistence. Complements E2B for harnesses that need long-lived working directories across multiple agent sessions rather than ephemeral code execution. ![Stars](https://img.shields.io/github/stars/daytonaio/daytona?style=flat-square&label=★&color=yellow)
- [NeMo Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) — NVIDIA's programmable guardrails toolkit: define input, dialog, retrieval, execution, and output rails that intercept the agent loop at five distinct layers using the Colang DSL. The execution rail layer specifically governs what tools the LLM can invoke and what their inputs/outputs may contain — the reference for behavioral-level enforcement when static allow/deny lists are insufficient. ![Stars](https://img.shields.io/github/stars/NVIDIA-NeMo/Guardrails?style=flat-square&label=★&color=yellow)

---

## Evals & Verification

- [Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) — Anthropic's comprehensive guide to agent evaluation: trajectory evals, outcome evals, and how to build eval harnesses that are themselves reliable.
- [DeepEval](https://github.com/confident-ai/deepeval) — The most complete open-source LLM/agent eval framework: 20+ built-in metrics (hallucination, answer relevancy, RAGAs, tool correctness), pytest integration, and a CI-friendly runner. Removes the need to hand-roll eval infrastructure when you need structured, repeatable agent quality gates. ![Stars](https://img.shields.io/github/stars/confident-ai/deepeval?style=flat-square&label=★&color=yellow)
- [SWE-bench](https://www.swebench.com) — The canonical benchmark for coding agents. Essential reference for understanding what "verified working" means for harness outputs.
- [Inspect AI](https://github.com/UKGovernmentBEIS/inspect_ai) — UK AI Security Institute's eval framework with native support for evaluating external agents (Claude Code, Codex CLI) as black-box targets, plus built-in bash/python/web browsing tools. Built for safety-grade rigor; the right foundation for harness-level eval infrastructure. ![Stars](https://img.shields.io/github/stars/UKGovernmentBEIS/inspect_ai?style=flat-square&label=★&color=yellow)
- [Quantifying Infrastructure Noise in Agentic Coding Evals](https://www.anthropic.com/engineering/infrastructure-noise) — Anthropic's empirical study showing container resource configuration alone produces 6+ percentage point benchmark swings — often exceeding model-to-model gaps. The 3x threshold finding is the key practical result: scores are stable up to 3x specified resources, but above that agents shift strategy entirely (lean tools vs. heavy dependencies), meaning tight and generous resource limits measure fundamentally different behaviors. Essential reading before interpreting any agentic eval.
- [tau-bench](https://github.com/sierra-research/tau-bench) — Benchmarks agent behavior in three-way user-tool-policy interactions — the failure mode SWE-bench doesn't cover. Useful for validating that a harness correctly enforces business rules across multi-turn, stateful conversations. ![Stars](https://img.shields.io/github/stars/sierra-research/tau-bench?style=flat-square&label=★&color=yellow)

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
- [awesome-mcp-servers](https://github.com/appcypher/awesome-mcp-servers) — Comprehensive list of MCP servers for extending agents with external capabilities. ![Stars](https://img.shields.io/github/stars/appcypher/awesome-mcp-servers?style=flat-square&label=★&color=yellow)
- [awesome-ai-agents](https://github.com/e2b-dev/awesome-ai-agents) — Curated list of AI agents and agent frameworks, organized by use case. Useful for surveying the landscape of what harnesses are being built around. ![Stars](https://img.shields.io/github/stars/e2b-dev/awesome-ai-agents?style=flat-square&label=★&color=yellow)
- [awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) — Collection of production LLM applications with source code across RAG, multi-agent, and tool-use patterns. Good reference for how harness primitives combine in real applications. ![Stars](https://img.shields.io/github/stars/Shubhamsaboo/awesome-llm-apps?style=flat-square&label=★&color=yellow)

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

**What belongs here:** Resources that address a specific harness engineering problem (context, tools, planning, permissions, memory, verification, sandboxing). Each addition should include a 1–2 sentence note explaining *why* it's worth including — this is an opinionated list, not a directory.

**What doesn't belong here:** General AI/ML papers, model benchmarks unrelated to agent harnesses, tutorials on using specific models, product marketing.

---

## License

[CC0](LICENSE) — public domain dedication.
