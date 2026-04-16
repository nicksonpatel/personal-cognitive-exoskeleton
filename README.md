# Personal Cognitive Exoskeleton

A system that observes, learns, and eventually replicates how you orchestrate AI agents and LLMs for knowledge work.

## The Problem

You navigate between research, coding, product building, and debugging — all through LLMs and agents. Your orchestration decisions (which agent, what decomposition, when to pivot) are implicit and live entirely in your head. Every session starts from zero because none of that decision-making intelligence is captured or reusable.

## The Idea

Build a pipeline with two phases:

### Phase 1: The Monitor (Cognitive Flight Recorder)
- Intercepts your workflow at natural breakpoints
- Asks structured, tap-able clarifying questions (not freeform journaling)
- Captures the **why** behind your orchestration decisions, not just the **what**
- Stores everything as structured logs

### Phase 2: The Orchestrator (Learned Automation)
- Synthesizes patterns from accumulated monitor logs
- Drafts orchestration rules and system prompt updates
- Handles routine 80% of decisions autonomously
- Escalates novel/ambiguous 20% back to you

## What Makes This Different

| Existing Work | What It Does | Gap |
|---|---|---|
| VIGIL (2025) | Watches AI agents for self-improvement | Observes AI, not humans |
| Imitation Learning | Learns from expert demos | Narrow tasks (robotics, games), not open-ended knowledge work |
| Human-in-the-Loop | Queries humans to improve agents | Studied in narrow contexts, not personal orchestration |
| LangGraph / CrewAI / AutoGen | Routes between agents | Assumes rules exist; doesn't discover them from behavior |

**The gap:** No system watches a human orchestrator doing messy, multi-tool knowledge work and distills that into transferable, evolving rules.

## Project Status

**Phase:** Discovery & Design

## Structure

```
docs/
  concept.md          — Core idea, motivation, and vision
  architecture.md     — System design and pipeline overview
  log-schema.md       — Schema for capturing orchestration decisions
```
