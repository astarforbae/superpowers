---
name: delegating-with-subagents
description: Use when the user explicitly asks to use subagents, delegate work, isolate context, or run parallel investigation for a non-trivial task
---

# Delegating With Subagents

## Overview

Use subagents to keep heavy work out of the main context. The main agent orchestrates, verifies, and synthesizes; subagents investigate or execute focused slices.

## When To Use

- The user asks to use subagents, workers, agents, delegates, or parallel investigation.
- The task needs broad code search, external references, or multiple independent workstreams.
- The main context should stay clean while other agents do heavy reading or exploration.
- The user specifies a subagent type, model, tool, concurrency limit, or rate-limit concern.

## Do Not Use

- The user only asks a simple question.
- The answer can be given directly without heavy search, broad code exploration, or parallel work.
- Subagents would be slower than answering normally.
- The user asks for a quick answer, concise reply, or lightweight response.

## Core Rules

- If no concurrency limit is given, run at most 2 subagents at once.
- Keep the main agent as orchestrator, not worker.
- Put heavy reading, searching, and comparison work in subagents.
- Give each subagent a narrow, non-overlapping assignment.
- Give each subagent the goal, hints, boundaries, expected output, and stop conditions.
- Ask subagents for compact findings, not raw notes or full transcripts.
- Do not let subagents edit the same files in parallel.
- Investigation may run in parallel; edits should be sequential unless clearly independent.
- Main agent must judge subagent output using evidence before deciding next steps.

## Prompt Template

```text
Task: <focused assignment>

Context:
- <why this matters>
- <relevant paths, repos, docs, or hints>

Boundaries:
- Do not implement unless explicitly asked.
- Do not inspect unrelated areas.
- Do not return raw dumps.

Return:
- Key findings with file paths or commands checked
- Recommended path
- Risks or blockers
- Open questions
```

## Orchestration Loop

1. Decompose the request into independent questions.
2. Dispatch up to the allowed concurrency limit.
3. Wait for results before dispatching dependent work.
4. Compare findings and resolve disagreements with minimal direct checks.
5. Produce a concise plan or execute the next coordinated step.

## Common Mistakes

| Mistake | Fix |
| --- | --- |
| Using tool-specific subagent names without being asked | Stay generic; use the user's chosen subagent type |
| Spawning too many agents for speed | Respect the user's limit; default to 2 |
| Letting subagents return huge context dumps | Require compact findings and evidence |
| Delegating judgment away | Main agent synthesizes and decides |
| Parallel edits in overlapping files | Serialize edits or split ownership clearly |

## Red Flags

- "I'll just inspect everything myself first."
- "More agents will be faster, even with rate limits."
- "The subagent can figure out the scope."
- "Raw notes are useful context."
- "Parallel edits are fine because tasks seem related."

If any red flag appears, stop and rewrite the delegation plan.
