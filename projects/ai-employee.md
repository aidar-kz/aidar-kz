# AI Employee — Local-First Agentic Planning Prototype

AI Employee is a small SvelteKit prototype for experimenting with explicit agent orchestration rather than hiding the workflow behind a large framework.

The source repository remains private while I review it for public release. This case study describes only the safe, portfolio-relevant architecture and behavior.

## Goal

Turn an ambiguous user goal into a structured execution plan using a self-hosted model, then progressively extend that planner into a system capable of repository inspection, worker delegation, validation, retries, and model routing.

## Current capabilities

- accepts a natural-language task
- sends the task to a locally hosted Qwen 2.5 7B model
- uses an OpenAI-compatible llama.cpp endpoint
- returns a structured execution plan
- surfaces risks, affected systems/files, recommended worker, and estimated complexity
- keeps model endpoint and selection in environment variables

## Architecture

```text
Browser
  ↓
SvelteKit UI
  ↓
SvelteKit server route
  ↓
OpenAI-compatible local inference endpoint
  ↓
Qwen 2.5 7B
```

The application is intentionally decoupled from the physical model host: it only needs an OpenAI-compatible HTTP endpoint, so the model can be local or hosted elsewhere.

## Stack

- Svelte 5
- SvelteKit 2
- TypeScript
- Vite
- Tailwind CSS
- llama.cpp-compatible API
- Qwen 2.5 7B

## Why this project exists

The interesting part of agentic software is not simply generating text or code. I use this prototype to explore questions such as:

- does the planner have enough context to choose the correct next action?
- can a large goal be decomposed into bounded implementation tasks?
- how should a planner choose a worker/model?
- how should failures be represented and fed back into the loop?
- what should be validated automatically before a task is considered complete?
- when should a repeated solution become a reusable tool or skill?

## Roadmap

- schema-validated structured output
- repository inspection and context collection
- specialized worker delegation
- test/check execution with failure feedback
- retry and recovery policies
- model routing and fallbacks
- persistent execution traces
- planner evaluation fixtures
- deployment authentication and access control

## Status

Active prototype. The current version is a planner; worker execution and delegation are roadmap items rather than features I claim are already complete.
