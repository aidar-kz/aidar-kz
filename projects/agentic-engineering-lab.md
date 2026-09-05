# Agentic Engineering Lab

A sanitized case study of the self-hosted development environment I use to experiment with multi-model agent orchestration, coding-agent delegation, local inference, and recovery/fallback behavior.

> This document intentionally omits credentials, private hostnames, tokens, client code, and machine-specific secrets. The underlying operational repositories remain private.

## Goal

Build a practical development workflow where a higher-level agent can maintain project context, decompose ambiguous work, and delegate bounded implementation tasks to specialized coding agents, while retaining the option to use local models for lower-cost or privacy-sensitive work.

## Components

- **OpenClaw** — persistent agent/runtime layer and tool/skill orchestration
- **Hermes** — high-level conversational/planning workflow experimentation
- **Codex** — implementation-focused coding agent
- **Ollama / local inference** — self-hosted models for selected tasks
- **Cloud model providers** — stronger models for planning, review, or difficult tasks
- **Linux + Docker + Proxmox** — infrastructure and isolation
- **GPU inference** — local model execution on an NVIDIA GPU

## Workflow

```text
User objective
    ↓
Planner / high-capability agent
    ↓
Repository + environment inspection
    ↓
Task decomposition
    ↓
Specialized coding agent (for example Codex)
    ↓
Diff / runtime / test inspection
    ↓
Concrete diagnostics fed back into the workflow
    ↓
Reusable skill, tool, configuration, or documented pattern
```

The key design principle is to treat agent failures as engineering failures to diagnose, not as a reason to blindly re-prompt.

## Problems I have worked through

### Model routing and fallbacks

Configured primary and fallback models across local and hosted providers. A useful routing layer needs to handle unavailable models, rate limits, quota/billing failures, and different capability/cost profiles without losing the intent of the task.

### Context and compaction

Long-running agent sessions eventually hit context limits. I have investigated stalled compaction, loss of strategically important context, and the difference between keeping a conversation alive versus deliberately starting a clean execution context.

### Tool and provider failures

I regularly inspect logs and configuration when a tool call, provider authentication flow, or model invocation fails. The goal is to identify the failing layer—agent, tool, provider, network, or model—before changing prompts.

### Local inference behavior

I operate local models on a separate GPU host and have debugged cases where inference unexpectedly split between CPU/GPU or behaved differently when called through an agent runtime versus directly.

### Planning vs implementation

I increasingly separate high-level planning from implementation. A strong planning model keeps the architecture and constraints in view, while a code-focused agent receives a bounded task and concrete acceptance criteria.

## What I learned

Useful agentic engineering is mostly systems engineering around the model:

- context quality
- task boundaries
- tool design
- observability
- validation
- recovery behavior
- fallback policy
- cost/latency trade-offs

A powerful model inside a weak workflow is still unreliable. A well-bounded workflow makes model capability much more useful.

## Current direction

I am continuing to turn repeated solutions into reusable skills/tools and to make evaluation more explicit: whether a planner chose the right action, whether a worker completed the task, and whether the result was validated against the real repository/runtime state.

## Related prototype

I also maintain an **AI Employee** prototype: a SvelteKit application that turns a user goal into a structured execution plan using a local Qwen model. The next steps are repository inspection, worker delegation, retries, execution traces, and model routing.
