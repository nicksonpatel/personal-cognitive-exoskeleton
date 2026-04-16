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

### Options, Not Typing — With a Smart Default
Freeform journaling dies within days. Every interaction with the monitor should be structured: tap-able options, multiple choice, quick selections. But a blank multiple-choice form also dies. The monitor must **propose the most likely answer first** — you confirm or correct. This means the monitor needs enough intelligence to make a confident guess before asking. A dumb form adds friction. A smart form that's usually right adds almost none.

### Capture Decomposition, Not Just Delegation
The real intelligence isn't "I used Claude for this." It's "I broke this vague product idea into 3 research questions, validated the market first, then scoped the MVP." The monitor must capture the *decomposition* step — the moment before you even touch an agent.

### Rules Should Decay
Extracted patterns will be overfitted to recent projects. Rules that haven't been validated recently should lose confidence and eventually get challenged or retired.

### Hard Minimum Before Rule Extraction
The synthesizer must not surface rules before enough data exists. Proposing rules too early creates two failure modes: you trust them (bad — they're overfitted) or you distrust everything it proposes (and stop using it). A hard minimum of **4–6 weeks of pure observation** should be enforced before any rule is surfaced. This discipline must be built into the system, not left to willpower.

### Contextual Triggers Are the Signal
Contradictory patterns — "sometimes I research first, sometimes I build first" — are not noise to resolve. They're the actual signal. The difference isn't what you did, it's what context triggered which approach. Every log entry must capture the **contextual trigger**: what made this situation feel like a research-first moment vs. a build-first moment? That's where the orchestrator intelligence lives.

### Close the Loop with Outcome Quality
Logging goal → decomposition → agent choice → reasoning is incomplete without outcome feedback. Without it, you're encoding your habits, not your *good* habits. Every session must close with outcome quality — and ideally, whether you actually used the output downstream.

## What This Is NOT

- **Not an agent framework** — it doesn't replace LangGraph, CrewAI, etc.
- **Not a prompt library** — it generates and evolves prompts from observed behavior
- **Not a task manager** — it doesn't track your TODO list, it tracks your *reasoning*
- **Not a general product** (yet) — it's built for one user: you

## Design Commitments (Resolved)

These were open questions but have been decided:

**Breakpoints:** The monitor fires at these specific moments:
1. When you open a new chat with a specific purpose (not exploratory/idle)
2. When you switch tools mid-task — this is a routing decision signal
3. When you copy output from one agent into another — this is a handoff moment, rich with intent
4. When you abandon something and restart — the pivot is where learning is richest

**Rule extraction minimum:** 4–6 weeks of pure observation before any rule is proposed. Hard-coded, not advisory.

**Monitor posture:** The monitor proposes first, you confirm or correct. Never blank forms.

## Open Questions

- How much latency/friction is acceptable before it kills adoption?
- What's the minimum viable log entry? (Too sparse = useless, too rich = abandoned)
- How does the monitor detect breakpoints automatically vs. requiring manual trigger? (Start with manual, earn automation)
- How does outcome quality decay affect rule confidence over time — is it time-based, usage-based, or explicit challenge?
