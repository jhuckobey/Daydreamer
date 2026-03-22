# Daydreamer

**Type:** Openclaw Skill
**Version:** 2.2.0

An AI agent skill that emulates the human act of daydreaming. The agent accumulates memories from its daily work and periodically synthesizes them into novel ideas by randomly traversing its memory log through a series of associative, hypothetical, and web-augmented cycles.

---

## How It Works

A **Python conductor script** handles the mechanical parts (cycle counting, random number generation, memory parsing, state tracking). The **AI agent** handles the creative parts (semantic matching, hypothetical reasoning, web searching, analytical questioning, final synthesis).

**Script as conductor, agent as musician.**

This split means:
- Deterministic cycle execution — even at 100+ cycles, nothing gets lost
- True randomness via Python's `random` module, not LLM approximation
- Significantly fewer tokens consumed — the agent gets clean, focused prompts per cycle instead of carrying the entire session in context
- Reliable state tracking across arbitrarily long sessions

---

## What It Does

- **Accumulates memories** daily into a numbered list (`Daydreams.MD`), capturing meaningful events and insights from work sessions.
- **Daydreams** by randomly walking through those memories using four traversal modes:
  - **Semantic Association** — find a thematically similar memory
  - **Hypothetical Exploration** — generate a "what if", reason through it, jump to a relevant memory
  - **Web Search Excursion** — search the web, land on a random result, connect it back to a memory
  - **Analytical Question** — ask a direct question about the accumulated context ("how does X work?", "does X apply here?"), think through the answer, and use it to deepen understanding
- **Synthesizes ideas** at the end of each session — novel connections that emerge from the combination of disparate memories.
- **Logs sessions** to `Daydreamlog.MD`, including every memory visited, every mode used, and the final synthesis.

---

## Prerequisites

- **Python 3.8+** (no external dependencies — standard library only)

---

## Files Created in Workspace

| File | Description |
|------|-------------|
| `Daydreams.MD` | Numbered daily memory log |
| `Daydreamlog.MD` | Log of completed daydream sessions and their outputs |
| `ideas/NNN-title.md` | Standalone idea files — one per session, auto-numbered |
| `daydreamer-config.json` | Configuration: frequency, cycles per session, last run dates |
| `.daydream-session/` | Temporary exchange directory during active sessions (auto-cleaned) |

---

## Daydream Types

| Type | Output |
|------|--------|
| **full** (default) | Open-ended — can produce an idea, recommendation, question, observation, warning, or anything else that emerges naturally. |
| **idea** | Focused on generating a novel, actionable idea — something that could be built or pursued. |

The default type is set in `daydreamer-config.json` and can be overridden per-session.

---

## Usage

### Slash Command

```
/daydream              # Default type and cycles
/daydream 5            # 5 cycles, default type
/daydream idea         # Default cycles, idea mode
/daydream idea 5       # 5 cycles, idea mode
/daydream full 20      # 20 cycles, full mode
```

### Natural Language

- "Daydream for me"
- "Start a daydream session"
- "Force an idea daydream with 15 cycles"
- "Daydream in full mode"

### CLI (direct script usage)

```bash
python scripts/daydream.py init                    # First-time setup
python scripts/daydream.py status                  # Check config and memory count
python scripts/daydream.py add-memory "Fixed X."   # Add a memory manually
python scripts/daydream.py run --cycles 10 --batch # Start a session
python scripts/daydream.py finalize                # Write log after agent completes
```

---

## Configuration

On first install, the agent will ask whether to use defaults or customize:

| Setting | Default | Description |
|---------|---------|-------------|
| Frequency | Once per day | How often scheduled daydreams run |
| Cycles per session | 10 | Number of random-walk steps per session |
| Default type | full | `full` (open-ended) or `idea` (idea generation) |

Settings are stored in `daydreamer-config.json` and can be changed by editing the file or asking the agent to reconfigure.

---

## Plugin Structure

```
Daydreamer/
├── skills/
│   └── daydreamer/
│       └── SKILL.md          # Full skill definition and agent instructions
├── commands/
│   └── daydream.md           # /daydream slash command
├── scripts/
│   └── daydream.py           # Conductor script (Python, no dependencies)
└── README.md
```
