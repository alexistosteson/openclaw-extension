# Gotchas

Hard-won operational surprises worth never rediscovering. One entry each: symptom, cause, rule going forward.

## This repo's history was rewritten on 2026-08-05 — pre-existing commit SHAs are dead

**Symptom:** A commit hash cited in any note, transcript, or sibling repo dated before
2026-08-05 does not resolve here, or resolves to an unreachable object.

**Cause:** Every commit was rewritten with `git filter-repo --mailmap` to normalise a stale
author identity, then force-pushed. The repo was rewritten more than once that day, because a
later rewrite in a sibling repo cascaded hash-reference repairs back through here.

**Rule going forward:** Don't resolve an old SHA by lookup — `filter-repo` rekeys its
commit-map on each run, so `.git/filter-repo/commit-map` from an earlier pass is not a valid
index. Join old→new on **subject line plus author timestamp** instead. And note that
`git cat-file -t` is not a reachability test: unreachable objects still resolve. Use
`git rev-list --branches --remotes`. The full cleanup record is kept outside this repo.

## Commits are gated by a machine-wide identity hook

**Symptom:** `git commit` or `git push` fails with `identity-guard: BLOCKED` or
`identity-guard: CANNOT CHECK`.

**Cause:** `~/.githooks/` + a global `core.hooksPath`, installed 2026-08-05, refuses any
commit carrying a stale identity. It is not repo-local — it applies to every repo on the
machine, and to every agent working on it.

**Rule going forward:** Never bypass it with `--no-verify`; that re-creates the exact problem
the rewrite above existed to fix. Amend the identity
(`git commit --amend --reset-author --no-edit`), or use the documented, legible override
`IDENTITY_GUARD=off git <command>` if the guard is genuinely wrong. The guard's own
configuration is documented where it is installed, outside this repo.
