# Project guidance for Claude Code

**Read these before any non-trivial work in this repository, in order:**

1. **[CONSTITUTION.md](CONSTITUTION.md)** — highest-authority project document: purpose, organizing principles, locked technical foundations, architectural invariants, process defaults, roadmap. Defer to it on any conflict short of an explicit user instruction.
2. **[docs/HANDOFF.md](docs/HANDOFF.md)** — living state of the project: what's shipped, what's next, the workflow, and a load-map. Updated as part of every spec-merge commit.

Both are **compact routers**. Reference detail lives in load-on-demand files under [`docs/handoff/`](docs/handoff/) and the amendment history in [docs/constitution-amendments.md](docs/constitution-amendments.md). Load only what the task needs; don't slurp every subfile.

## Data boundaries

{{DATA_BOUNDARIES: set by the charter brainstorm — what data may or may not enter Claude sessions, and any user-only operations. Delete this section if the project touches no sensitive data.}}

## Spec ritual

Every spec carries the sections in [docs/superpowers/specs/_SPEC-SECTIONS.md](docs/superpowers/specs/_SPEC-SECTIONS.md) on top of what `superpowers:brainstorming` writes: **Deliberately absent** always, and **Rendered output** whenever the spec produces a user-visible surface. Both are presence checks — an unfilled `{{...}}` marker trips the placeholder scan in brainstorming's spec self-review, so no separate reviewer is needed.

## Planning ritual

Before writing an implementation plan for any spec:

1. Run the NASA Power of Ten audit on the files the spec will touch: `/nasa-code <comma-separated paths>`
2. Fold any 🔴 Critical or 🟠 High findings into the plan as explicit fix steps.
3. Note 🟡 Medium findings in the plan as advisory comments.
4. Tier every task: add the per-task `Model:`/`Review:` line per the **tiered-dispatch** skill. Any task rendering a user-visible surface also carries `+ surface (<ref to the spec's Rendered output section>)` on that line — the reviewer is then given the artifact and returns a verdict on it separately from spec compliance.
5. **Trace every task back to a spec requirement.** `writing-plans`' self-review already checks the forward direction (every spec requirement has a task); run the inverse too — a task that traces to nothing in the spec is scope creep. Cut it or amend the spec. This is where YAGNI is enforced, now that the spec-document reviewer is gone.
6. The plan self-review also checks step 4's annotations: flag any `haiku` task lacking a cited precedent, containing prose-only steps ("add validation"), or without a mechanical check, and any task missing its reasons.

## Execution ritual

When executing a plan task-by-task, follow the **tiered-dispatch** skill from the first implementer dispatch.

**Check in between tasks.** `superpowers:subagent-driven-development` (6.2.0+) instructs the controller to execute every task without pausing. In this project that is overridden: pause after each task, report what landed, and confirm before dispatching the next. Owner preference — see the Collaboration model in [docs/HANDOFF.md](docs/HANDOFF.md). This instruction outranks the skill.

## Spec-merge ritual

When merging a spec branch to `main`:

1. Rebase the spec branch onto `main` if branches have diverged.
2. Run the full test suite on the rebased branch and confirm green: `(test command TBD — set once language is chosen)`
3. Fast-forward `main`.
4. **Update the handoff docs** in the same merge — add the new spec to [docs/handoff/shipped.md](docs/handoff/shipped.md), bump the "Last updated" date + "Current state" snapshot and advance "Next" in [docs/HANDOFF.md](docs/HANDOFF.md), record any newly-discovered gotchas in [docs/handoff/gotchas.md](docs/handoff/gotchas.md), and if the roadmap shifted, amend [CONSTITUTION.md](CONSTITUTION.md) §VI + [docs/constitution-amendments.md](docs/constitution-amendments.md). Commit on `main` directly (doc-only follow-up).
5. Remove the worktree and delete the branch.
