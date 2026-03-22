---
description: Trigger a daydream session. Optionally specify type (full/idea) and number of cycles.
argument-hint: [full|idea] [cycles]
allowed-tools: [Read, Write, Edit, Bash, WebSearch]
---

# /daydream

Invoke the Daydreamer skill to run a daydream session.

## Arguments

$ARGUMENTS

Parse the arguments flexibly:
- `/daydream` → default type and cycles
- `/daydream 5` → 5 cycles, default type
- `/daydream idea` → default cycles, idea type
- `/daydream idea 5` → 5 cycles, idea type
- `/daydream full 20` → 20 cycles, full type

Recognize `full` and `idea` as type keywords. Any number is the cycle count.

## Instructions

### 1. Check prerequisites

```bash
python scripts/daydream.py status
```

If "Not initialized", run first-install setup per the Daydreamer skill instructions.

### 2. Start the session

Build the start command from parsed arguments:

```bash
python scripts/daydream.py start --cycles {N} --type {TYPE} --forced
```

### 3. Process cycles

Read `.daydream-session/session_state.json` for context.

For each prompt file (`.daydream-session/prompt_cycle_NNN.json`):
1. Read the prompt — it tells you the mode and provides full accumulated context.
2. Execute the creative task for that mode (see SKILL.md for mode descriptions).
3. Write the response file (`.daydream-session/response_cycle_NNN.json`).
4. Advance: `python scripts/daydream.py next-cycle`

Repeat until `next-cycle` reports `cycles_complete`.

### 4. Synthesize

Read `.daydream-session/prompt_synthesis.json`. It contains the full accumulated context AND `synthesis_instructions` specific to the daydream type. Follow those instructions when writing your synthesis.

Write `.daydream-session/response_synthesis.json`.

### 5. Finalize

```bash
python scripts/daydream.py finalize --forced
```

### 6. Present results

Tell the user what you concluded. Present the synthesis directly and conversationally. Point them to the idea file in `ideas/`.

This is a **forced daydream** — `--forced` ensures the daily schedule is not affected.
