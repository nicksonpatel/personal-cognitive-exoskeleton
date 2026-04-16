# Architecture Overview

## High-Level Pipeline

```
┌─────────────────────────────────────────────────────────────┐
│                     YOU (Human Orchestrator)                 │
│                                                             │
│  research ←→ coding ←→ building ←→ debugging ←→ exploring  │
└──────────────────────────┬──────────────────────────────────┘
                           │
                    ┌──────▼──────┐
                    │   MONITOR   │  ← Phase 1 (build first)
                    │             │
                    │ • Intercept │
                    │ • Ask why   │
                    │ • Log       │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  LOG STORE  │
                    │             │
                    │ • Decisions │
                    │ • Context   │
                    │ • Outcomes  │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  SYNTHESIZER│  ← Phase 2 (build later)
                    │             │
                    │ • Patterns  │
                    │ • Rules     │
                    │ • Prompts   │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │ ORCHESTRATOR│  ← Phase 3 (build last)
                    │             │
                    │ • Route     │
                    │ • Delegate  │
                    │ • Escalate  │
                    └─────────────┘
```

## Phase 1: The Monitor

### Components

**Interceptor**
- Hooks into your workflow at natural breakpoints
- Possible integration points:
  - CLI wrapper (you invoke it before starting a task)
  - Browser extension (detects when you open a new AI chat)
  - VS Code extension (watches for context switches)
  - Simple hotkey trigger (manual, but zero-integration)

**Clarifier**
- Presents 2-3 structured questions per interception
- Questions are context-aware (adapts based on what it already knows)
- All answers are selectable options, not freeform (unless "other")
- Must complete in <15 seconds or you'll skip it

**Logger**
- Writes structured JSON entries to local storage
- Each entry follows the log schema (see log-schema.md)
- Append-only, no editing of past logs

**Dashboard (optional, later)**
- Weekly/daily summary of your orchestration patterns
- "You used research-first approach 73% of the time for new product ideas"
- Surfaces emerging patterns before the synthesizer is built

### Integration Strategy (Phase 1)

Start with the **simplest possible integration**: a CLI command or hotkey you invoke manually. No browser extensions, no auto-detection. Prove the logging value before investing in automation.

```
$ exo start "build a pricing page for X"
> What type of task? [research / build / validate / debug / explore]
> How clear is the goal? [clear / somewhat clear / fuzzy]
> First step? [decompose / research / prototype / ask someone]

$ exo log "switched to Claude for code generation"
> Why this tool? [better at code / has context / habit / cost]
> Expected output? [working code / draft to iterate / learning]

$ exo done
> Outcome? [completed / partial / abandoned / pivoted]
> Would you do it the same way? [yes / mostly / differently]
```

## Phase 2: The Synthesizer

- Runs periodically (weekly?) over accumulated logs
- Uses an LLM to identify recurring patterns
- Proposes draft rules in natural language:
  - "When goal is fuzzy AND task is product-related → always decompose first"
  - "When debugging → you prefer Claude over GPT 85% of the time"
- You review and approve/reject/edit proposed rules
- Approved rules become candidate orchestrator instructions

## Phase 3: The Orchestrator

- Receives a task/goal from you
- Applies learned rules to:
  - Decompose the goal
  - Select agents/tools
  - Generate prompts
  - Route sub-tasks
- Handles routine cases autonomously
- Escalates when confidence is low or the situation is novel
- Continues to log its own decisions (feeding back into the monitor)

## Data Flow

```
Action → Monitor → Enriched Log → Log Store
                                      ↓
                                 Synthesizer (periodic)
                                      ↓
                                 Draft Rules
                                      ↓
                                 Human Review
                                      ↓
                                 Approved Rules → Orchestrator
                                      ↓
                                 Autonomous Decisions → Log Store (feedback loop)
```

## Technology Considerations (TBD)

- **Log storage:** Local JSON files → SQLite → maybe a proper DB later
- **Monitor interface:** CLI first, then possibly a lightweight web UI or system tray app
- **Synthesizer:** LLM-powered (Claude/GPT reads logs, proposes patterns)
- **Orchestrator:** Could be a custom agent, or rules fed into existing frameworks
- **Language:** Python likely (ecosystem for LLM tooling), but TBD
