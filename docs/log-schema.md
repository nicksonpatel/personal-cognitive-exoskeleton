# Log Schema Design

## Design Goals

- **Structured enough** to be machine-learnable
- **Lightweight enough** that you'll actually fill it in
- **Rich enough** to capture the *why*, not just the *what*

## Core Log Entry

Each entry represents one orchestration decision or action.

```json
{
  "id": "uuid",
  "timestamp": "2026-04-16T12:30:00Z",
  "session_id": "uuid",

  "goal": {
    "description": "Build a pricing page for product X",
    "clarity": "fuzzy | somewhat_clear | clear",
    "type": "research | build | validate | debug | explore | plan"
  },

  "action": {
    "type": "decompose | delegate | switch_tool | reformulate | escalate | pivot | complete",
    "description": "Broke goal into 3 sub-tasks: research competitors, design layout, implement",
    "tool_or_agent": "claude | gpt | copilot | perplexity | custom_agent | manual | other",
    "tool_reason": "better_at_task | has_context | speed | cost | habit | only_option"
  },

  "context": {
    "confidence": "high | medium | low",
    "prior_attempts": 0,
    "parent_task_id": null,
    "sub_tasks": ["uuid1", "uuid2"],
    "tags": ["product", "frontend", "mvp"]
  },

  "outcome": {
    "status": "completed | partial | abandoned | pivoted",
    "quality": "good | acceptable | poor | unknown",
    "would_repeat": "yes | mostly | differently",
    "notes": "optional freeform"
  }
}
```

## Field Definitions

### goal.clarity
How well-defined the task was when you started:
- **clear** — You knew exactly what to do and what "done" looks like
- **somewhat_clear** — General direction was known, details were fuzzy
- **fuzzy** — Vague idea, needed decomposition or research first

### goal.type
The nature of the work:
- **research** — Gathering information, exploring a space
- **build** — Creating an artifact (code, design, document)
- **validate** — Testing an assumption or checking quality
- **debug** — Fixing something broken
- **explore** — Open-ended investigation, no specific deliverable
- **plan** — Decomposing a larger goal into steps

### action.type
What you actually did at this decision point:
- **decompose** — Broke a goal into smaller sub-tasks
- **delegate** — Handed a task to a specific agent/tool
- **switch_tool** — Changed from one agent/tool to another mid-task
- **reformulate** — Rephrased a prompt or restructured the task
- **escalate** — Decided this needed human input or a different approach
- **pivot** — Abandoned current approach for a fundamentally different one
- **complete** — Finished the task

### action.tool_reason
Why you chose this particular tool/agent:
- **better_at_task** — This tool is objectively better for this type of work
- **has_context** — This tool already has the conversation context
- **speed** — Faster response or iteration
- **cost** — Cheaper option
- **habit** — Default choice, no strong reason
- **only_option** — No alternative available

## Session Structure

A session groups related log entries:

```json
{
  "session_id": "uuid",
  "started_at": "2026-04-16T12:00:00Z",
  "ended_at": "2026-04-16T14:30:00Z",
  "top_level_goal": "Build MVP of pricing page",
  "summary": "auto-generated or manual session summary",
  "entry_count": 12,
  "tools_used": ["claude", "copilot", "figma"],
  "outcome": "completed | partial | abandoned"
}
```

## Example: A Real Session

```
Session: "Research and build a pricing page"

Entry 1: goal=fuzzy, action=decompose
  → Broke into: research competitors, decide on model, design, implement

Entry 2: goal=clear, action=delegate, tool=perplexity, reason=better_at_task
  → "Research competitor pricing pages"

Entry 3: goal=somewhat_clear, action=delegate, tool=claude, reason=better_at_task
  → "Help me decide between 3-tier and usage-based pricing"

Entry 4: goal=clear, action=delegate, tool=claude, reason=has_context
  → "Generate the React component for the pricing page"

Entry 5: goal=clear, action=switch_tool, tool=copilot, reason=speed
  → "Moved to Copilot for inline code iteration"

Entry 6: action=complete, outcome=completed, quality=good, would_repeat=yes
```

## Monitor Question Mapping

Each field maps to a quick question the monitor asks:

| Field | Question | Options |
|---|---|---|
| goal.type | What kind of task? | research / build / validate / debug / explore / plan |
| goal.clarity | How clear is the goal? | clear / somewhat / fuzzy |
| action.type | What did you just do? | decompose / delegate / switch / reformulate / pivot |
| action.tool_reason | Why this tool? | better at it / has context / speed / cost / habit |
| confidence | How confident are you? | high / medium / low |
| outcome.status | How'd it go? | completed / partial / abandoned / pivoted |
| outcome.would_repeat | Same approach next time? | yes / mostly / differently |

## Storage

**Phase 1:** Local JSON files, one per session, in a `logs/` directory
**Phase 2:** SQLite for querying across sessions
**Phase 3:** Whatever the synthesizer needs
