# Get Started — the front door (for anyone)

> The **first-contact router**. When someone new — technical or not — arrives at this repo and says
> "hi", "what is this?", or "let's get started", this is what the AI runs: a short, friendly
> conversation that orients them in a sentence, asks **one or two** questions to find out who they are
> and what they want, and routes them to the right next step. Tool-agnostic; the Claude Code
> `/henergy-start` skill and the Cursor command wrap it. It sits *above* the other entry points
> ([`personal-config.md`](personal-config.md), [`task-init.md`](task-init.md),
> [`checkpoint.md`](checkpoint.md), [`github-sync.md`](github-sync.md)) and dispatches to them.

---

## When to run this

Any first-contact signal with no specific task yet: "hi", "hey", "let's get started", "what is this /
what can you do", "I want to try this", "help me get oriented", or simply a person who has just
cloned/opened the repo and isn't sure where to begin. *(If they already have a concrete unit of work
— "I'm starting #2296" — go straight to [`task-init.md`](task-init.md) instead.)*

## The procedure

### 0. Make sure the context is current
Before anything, refresh the local copy so you're working from the latest shared context — run the
[`sync-context.md`](sync-context.md) step (auto fast-forward for non-technical users on a clean
`main`; a quick offer for technical ones — never over uncommitted work). If it's a ZIP download (no
git), note that and carry on.

### 1. Greet + one-sentence orientation
Welcome them and say what this is in a line: *"`hypha-energy-context` is Hypha's shared, AI-legible working
memory — I read it so we don't start from scratch, and we feed what we learn back in. Tell me a bit
about you and I'll point you to the most useful place to start."* Keep it warm and short.

### 2. Ask one or two questions (don't dump the whole repo)
- **What brings you here** — *building/contributing on the technical side*, or *understanding the
  platform and how the team works* (or *just exploring*)?
- **(If technical)** do you want to **set up to use it now**, or **look around first**?

Adapt to whatever they volunteer; skip questions they've already answered.

### 3. Route to what they need

| They are… | Route |
|---|---|
| **Technical, ready to use it** | Run [`personal-config.md`](personal-config.md) (`/henergy-dev-setup`) to set up their config. GitHub integration is asked as part of that setup (org board: **hypha.energy website development**, `Hypha-Energy/projects/13`). Explain the **cross-repo flow** (start here, change code in the cloned repo under `../src/`, record back; see root [`AGENTS.md`](../AGENTS.md)); when they have a ticket, [`task-init.md`](task-init.md) (`/henergy-task-init`) and [`checkpoint.md`](checkpoint.md). |
| **Technical, looking first** | Give a short tour: [`solution/tech/`](../solution/tech/AGENTS.md) and [`workflow/`](AGENTS.md) (the method + the commands). Offer `/henergy-dev-setup` whenever they're ready. |
| **Non-technical** (conceptual / strategic) | **No setup needed.** Walk them through [`solution/`](../solution/AGENTS.md) (what Hypha Energy is), then [`progress/`](../progress/AGENTS.md) + [`planning/`](../planning/AGENTS.md) for status. Tell them they can **just ask questions** and you'll answer from the context. |
| **Just exploring** | Point to [`README.md`](../README.md) → root [`AGENTS.md`](../AGENTS.md) → [`solution/overview.md`](../solution/overview.md), and offer to walk any of them. |

### 4. Honor the engagement profiles
Calibrate how much you ask of them (root [`AGENTS.md`](../AGENTS.md) → the four profiles): an active
technical contributor gets the full setup; a low-discipline technical one gets you doing more of the
scaffolding; conceptual/strategic people get value **without** any repo discipline — they ask, you
answer, and their input is captured for them.

### 5. Hand off cleanly
End by doing (or teeing up) the chosen next step — don't leave them at a menu. If you ran `henergy-dev-setup`
or started a tour, say what's happening and what comes next.

---

## Why this exists

The other entry points assume you already know what you want (set up config, start a task, record
progress, sync GitHub). This one assumes you **don't yet** — it's the welcoming, zero-prerequisite
door for the whole team, including people who'll never set up a personal config but should still get
value just by asking. It's the practical embodiment of "designed for the whole org, not technical
only."
