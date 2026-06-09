# COORD — Coordination Protocol for Autonomous Agents

**The HTTP of the agent stack.** A standard protocol for how agents coordinate work: task definition, lifecycle, state handoff, dependency resolution, escalation, and human-in-the-loop — framework-agnostic.

## Why

A2A gives agents a way to pass messages. It doesn't define:

- How to split a goal into sub-tasks and assign them
- How context travels between agents without copying chat history
- What happens when an agent gets stuck or times out
- How human approval gates interrupt an agent pipeline
- How to trace causality across 5 concurrent agents

Every framework solves these *inside* its own walled garden. COORD defines a common language so any framework can interoperate.

## Where It Fits

```
L4  Application         CrewAI / LangGraph / Claude SDK
                          ↑ COORD ← here
L3  Coord & Comm        A2A (transport) + MCP (tools) + MCP-DS (discovery)
L2  Runtime             LangGraph / OpenAI SDK / Smolagents
L1  Models              GPT-4o / Claude / Gemini
```

- **A2A** transports messages (TCP)
- **COORD** defines what those messages mean (HTTP)

## Quick Overview

### Task Envelope
A sealed JSON format any agent can accept, process, and return — with goal, inputs, dependencies, state pointers, constraints, output contract, and escalation rules.

### Coordination Lifecycle
A 12-state machine: `CREATED → ASSIGNED → IN_FLIGHT → NEED_HELP → REVIEW → COMPLETED / FAILED / ESCALATED / REASSIGNED / CANCELLED`

### State Handoff via Pointers
Durable memory pointers (`memory://wf_001/state`) instead of passing full chat history. Git-like versioning, no redundant context passing.

### Dependency Graphs
Workflow DAG where tasks declare `depends_on`. Coordinator resolves parallelism vs. sequential.

### Human-in-the-Loop
Standard `hitl_request` format with options, context, and timeout. Just another state transition.

---

Read the [full spec →](https://jianran.github.io/agent-coord/)
