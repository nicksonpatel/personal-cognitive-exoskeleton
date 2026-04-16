# Concept: Personal Cognitive Exoskeleton

## Core Insight

When you orchestrate AI agents, you make dozens of implicit decisions per session:
- How to decompose an ambiguous goal into actionable steps
- Which tool or agent to use for each step (and why)
- When to pivot, retry, or reformulate a prompt
- What "good enough" looks like for the current context
- When to go deep vs. when to stay broad

These decisions are your **orchestration intelligence**. Right now, it's invisible — trapped in your head, lost after every session.

## Vision

A personal system that:

1. **Observes** your orchestration behavior with minimal friction
2. **Asks** structured questions to capture intent (not just actions)
3. **Accumulates** a rich log of goal → decomposition → agent choice → reasoning → outcome
4. **Surfaces** patterns you didn't know you had
5. **Encodes** those patterns into reusable orchestration rules
6. **Operates** autonomously for routine decisions, escalating the rest

## Key Principles

### Monitor Before Automate
You can't encode rules you haven't discovered. The monitor comes first — weeks of data collection before any automation. Resist the urge to build the orchestrator early.

### Options, Not Typing
Freeform journaling dies within days. Every interaction with the monitor should be structured: tap-able options, multiple choice, quick selections. The system should propose the likely reason, and you confirm or correct. This gives structured, learnable data with near-zero friction.

### Capture Decomposition, Not Just Delegation
The real intelligence isn't "I used Claude for this." It's "I broke this vague product idea into 3 research questions, validated the market first, then scoped the MVP." The monitor must capture the *decomposition* step — the moment before you even touch an agent.

### Rules Should Decay
Extracted patterns will be overfitted to recent projects. Rules that haven't been validated recently should lose confidence and eventually get challenged or retired.

## What This Is NOT

- **Not an agent framework** — it doesn't replace LangGraph, CrewAI, etc.
- **Not a prompt library** — it generates and evolves prompts from observed behavior
- **Not a task manager** — it doesn't track your TODO list, it tracks your *reasoning*
- **Not a general product** (yet) — it's built for one user: you

## Open Questions

- What are the right breakpoints to intercept? (Before new chat? After tool switch? On context shift?)
- How much latency/friction is acceptable before it kills adoption?
- What's the minimum viable log entry? (Too sparse = useless, too rich = abandoned)
- When does the system have enough data to propose its first orchestration rule?
- How do you handle contradictory patterns? (Sometimes you research first, sometimes you build first — context matters)
- Should the monitor be passive (watches and asks after) or active (intercepts before)?
