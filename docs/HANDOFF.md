# Project handoff — openclaw-extension

Entry point for project state — read after the always-loaded `CLAUDE.md`. Governing principles live in [CONSTITUTION.md](../CONSTITUTION.md). This file stays small on purpose: **reference detail lives in [`docs/handoff/`](handoff/), loaded only when your task needs it** (see the load-map below). Update it as part of the merge commit of every spec.

---

## Collaboration model

The project owner is a **non-technical Product Manager** — strong product judgment, some technical understanding, no hands-on coding proficiency. Calibrate every decision point accordingly:

- **Technical-only tradeoffs** (those that require coding proficiency to judge — module layout, data structures, function signatures, algorithm choice, test mechanics) → **do not ask**. Pick the best option, make the change, and state briefly what you chose and why.
- **Functional / feature / behavioral tradeoffs that happen to be rooted in a technical choice** (anything that changes what the system *does*, a feature's behavior, privacy posture, user-visible quality, or scope) → **translate into plain, non-technical terms and put the choice to the owner.** Never make the owner adjudicate an implementation detail dressed up as a question.

When unsure which bucket a decision falls in, lean toward presenting it — but presented in plain language, not in code terms.

---

## Current state

**Last updated:** 2026-07-20 — project scaffolded; charter pending.

{{CURRENT_STATE: one short paragraph per merge, replacing the previous. What shipped last, what state the system is in.}}

## Next

Run the charter brainstorm (fills CONSTITUTION §I–§IV and §VI, plus CLAUDE.md's Data boundaries), then Spec 1.

---

## Workflow — the ritual chain per spec

1. **Set up worktree** — `superpowers:using-git-worktrees` → branch `claude/spec<N>-<topic>`, in `.worktrees/` at the project root (already gitignored).
2. **Brainstorm** — `superpowers:brainstorming` → walk design sections with the owner one at a time → write spec to `docs/superpowers/specs/YYYY-MM-DD-spec<N>-<topic>.md`, carrying the required sections from [specs/_SPEC-SECTIONS.md](superpowers/specs/_SPEC-SECTIONS.md) → run the skill's **inline spec self-review** (its placeholder scan catches unfilled required sections) → commit. *(Superpowers 5.0.6 deleted the spec-document-reviewer loop and the agent no longer exists — do not dispatch it.)*
3. **Plan** — Planning ritual in [CLAUDE.md](../CLAUDE.md) (NASA audit + task tiering + task-to-spec traceability) → `superpowers:writing-plans` → write to `docs/superpowers/plans/YYYY-MM-DD-spec<N>-<topic>.md` → run the skill's **inline plan self-review** → commit. *(The plan-document-reviewer loop was removed in the same release.)*
4. **Execute** — `superpowers:subagent-driven-development`, following the **tiered-dispatch** skill: one subagent per task, review depth per the plan's `Review:` line, tier log per task. Progress rides in the skill's plan-scoped ledger at `.superpowers/sdd/<plan-basename>/progress.md` — the within-spec recovery record; this file remains the record across specs. Pause between tasks per CLAUDE.md's Execution ritual.
5. **Verify functionally** — before declaring any task done, exercise the affected flow for real. "Unit tests green" is not sufficient on its own. For any spec with a **Rendered output** section, the comparison is the **running surface against that artifact** — never code-against-spec alone, which passes whenever the code faithfully implements a defective spec.
6. **Merge** — Spec-merge ritual in [CLAUDE.md](../CLAUDE.md), including this file's update.

---

## Load-map

| Need | Load |
|---|---|
| What has shipped | [handoff/shipped.md](handoff/shipped.md) |
| Operational surprises / traps | [handoff/gotchas.md](handoff/gotchas.md) |
| Constitution change history | [constitution-amendments.md](constitution-amendments.md) |
| A specific spec or plan | `docs/superpowers/specs/`, `docs/superpowers/plans/` |
