# Contributor Profiles

> The six engagement profiles that shape how AI tools adapt their behavior when helping Hypha
> contributors. Referenced from the root [`AGENTS.md`](../AGENTS.md). Identify the profile first —
> it determines workflow depth, satellite setup, GitHub integration, auto-capture behavior,
> and how the AI should communicate. When in doubt, ask one or two orienting questions rather than
> guessing.

---

## The six profiles

### A — Engaged developer (technically knowledgeable)

**Who:** A developer who actively works in code repos, understands tooling, and is willing to
maintain SDD discipline — running skills explicitly, writing decisions back, keeping specs current.
The reference implementation of the workflow.

**GitHub integration:** Enabled if opted in (`github.enabled: true` in `AGENTS.local.md`).

**SDD discipline:** Full — explicit skill invocations, human-decides posture, checkpoint at session
end. AI surfaces callouts and open threads; developer confirms direction.

**AI posture:** Independent with callouts. State the review mode and decision posture used. Defer
on all non-obvious calls.

**Examples:** Ger Roza (gerroza).

---

### B — Developer, low technical discipline (vibe-coder)

**Who:** A developer who works in code repos but won't actively engage with process tooling —
they're focused on shipping, not on maintaining context. May use Cursor primarily and rely on AI
for most workflow mechanics. Understands the product technically but avoids overhead.

**GitHub integration:** Enabled if opted in; otherwise skipped silently.

**SDD discipline:** Minimal input — AI does the upkeep and just shows the result. Specs are still
written before code (the discipline is in the method, not the manual steps). Checkpoint may be
triggered by soft signals (CI/build checks, new-task start, 30-day cleanup) rather than explicit
invocation.

**AI posture:** Do more, narrate less. Complete the context capture without interrupting the flow.
For any ambiguous gate (phase transition, spec sign-off), still ask — but frame the question as
one clear choice, not a process explanation.

**Examples:** Any developer added to the team without prior SDD exposure.

---

### C — Non-dev contributor, technically aware (PM / PO / Tech Lead)

**Who:** Understands the product and platform at a conceptual/technical level — can read specs,
follow architecture discussions, and give meaningful input on acceptance criteria — but does not
write code or interact with development tooling directly.

**GitHub integration:** Read-only if needed (for issue context); no write operations.

**SDD involvement:** May review specs and provide acceptance input. Not responsible for
implementation tickets or the dev workflow. Contributions arrive as feedback, direction-setting,
or sign-off — captured here by the AI or an engaged dev, not by themselves.

**AI posture:** Explain what's happening without requiring process knowledge. Use product/outcome
framing, not tooling framing. Capture their input into the right artifact without making them do it.

---

### D — Non-dev org member (non-technical)

**Who:** An org member who wants to understand what's happening at the product/feature level —
deliverables, timelines, outcomes — but has no interest in technical or process detail. May
contribute ideas or feedback on user-facing decisions.

**GitHub integration:** No.

**SDD involvement:** None directly. Their input is captured by others; they consume summaries and
platform outputs, not raw context.

**AI posture:** Plain language. Focus on outcomes, not mechanics. Route complex technical topics to
a summary or a relevant team member.

---

### E — External technical integrator (non-Hypha dev)

**Who:** A technically knowledgeable person outside the Hypha team who wants to consume, integrate
with, or build on top of the Hypha platform — APIs, contracts, webhooks, SDKs, MCP server. Not a
team member; won't contribute to `hypha-energy-context` or follow internal SDD.

**GitHub integration:** Read-only (public repos only).

**SDD involvement:** None. Point them toward `solution/tech/` for architecture, deployed contract
addresses, API shape. Their interface is the public surface of the platform.

**AI posture:** Technical, outward-facing. Explain the external API surface and integration points
clearly. Don't expose internal workflow or progress details.

---

### F — External end-user / non-org member

**Who:** A user of the Hypha platform who is not a team member. Wants to understand the product in
terms of features, UX, and functionality — not the development process or internals.

**GitHub integration:** No.

**SDD involvement:** None. `hypha-energy-context` is not their interface; the platform is. If they arrive
here, orient them toward user-facing resources and the platform itself.

**AI posture:** Friendly, user-product framing. No process or technical tooling language.

---


## Mapping to the existing four-profile model

The root `AGENTS.md` previously described four engagement profiles. This six-profile model refines
and extends them — the mapping is:

| Old (4-profile) | New (6-profile) |
|---|---|
| Disciplined active contributor | A — Engaged developer |
| Technical co-contributor, low process discipline | B — Vibe-coder |
| Conceptual contributor, platform-interfaced | C — Non-dev, technically aware |
| Strategic stakeholder, output consumer | D — Non-dev org member |
| *(not previously modelled)* | E — External integrator |
| *(not previously modelled)* | F — External end-user |
