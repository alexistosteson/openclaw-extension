# Required spec sections

Not a spec. This is the short list of sections every spec in this directory must
carry **in addition to** whatever `superpowers:brainstorming` writes. Copy the
relevant blocks into the spec and fill them.

These are **presence checks, not judgment calls** — an unfilled `{{...}}` marker
is caught by the placeholder scan in `brainstorming`'s spec self-review, with no
reviewer needed. That is the point: the sections exist so a missing answer is
visible, rather than depending on someone noticing an absence.

---

## Deliberately absent

Required on **every** spec.

```markdown
## Deliberately absent

{{What this spec consciously does NOT do, and why. One line each. "Nothing" is a
valid answer — write it explicitly rather than deleting the section.}}
```

Scope creep is easiest to catch against a written boundary. This is also where
YAGNI lands at spec time; the plan-time check (every task traces to a spec
requirement — see CLAUDE.md's Planning ritual) is the enforcing half.

---

## Rendered output

Required on any spec that produces a **user-visible surface** — a page, a pane, a
CLI output format, a report, a notification. If a human will look at it, this
section is required.

```markdown
## Rendered output

{{A link to the captured prototype branch, OR a literal mockup of what the user
sees, with realistic values. Not a description of the data behind it.}}

{{If an owner-supplied sketch exists, paste it VERBATIM here — never paraphrase
it into prose.}}
```

**A data-shape description is not a rendered-output spec.** "per-session rows",
"shows tokens and cost", "a breakdown by model" describe the data layer. They
are not this section, and a spec whose Definition of Done is phrased that way
can be implemented exactly as written and still be wrong.

The cheapest way to fill this is to build the thing first: run the prototype
skill, react to it, and link the captured artifact. A prototype you have clicked
is better ground truth than a mockup you have written.

**This section is what both gates compare against.** Functional verification is
running-UI-vs-this-artifact, never code-vs-spec alone — code can match a
defective spec perfectly. See the Workflow chain in
[docs/HANDOFF.md](../../HANDOFF.md), step 5.

The **review** gate uses it too: any plan task rendering this surface carries
`+ surface (spec §Rendered output)` on its `Review:` line, and the reviewer is
given this artifact and returns a verdict on it separately. Tested — a reviewer
holding only the spec litigates its wording (a grouping key, a label) and never
asks whether the artifact itself is right.
