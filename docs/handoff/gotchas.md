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

## `main` is fenced by a ruleset, not by branch protection — and the owner bypasses it

**Symptom:** A push to `main` is refused for an agent but succeeds for the owner, and
`GET /branches/main/protection` returns `404 Branch not protected` — which reads as
"there is no fence" and is wrong.

**Cause:** The fence is a **repository ruleset** (`fence-main`), not legacy branch
protection. Legacy protection only knows roles: either admins bypass, and the fence is
decorative for any owner-minted token, or nobody bypasses and the owner is locked out of
their own repo, because GitHub forbids approving your own pull request. A ruleset can name
who bypasses, so `main` requires a pull request and one approving review for everyone
*except* the repository-admin role. Automation holds no such role.

**Rule going forward:** Read the fence at `GET /repos/{owner}/{repo}/rules/branches/main`,
or `GET /rulesets/{id}` for `enforcement` and `current_user_can_bypass`. The branch-
protection endpoint answers a question nobody is asking here, and its 404 is not evidence
of an unfenced branch. Deleting the ruleset is the only way to unfence `main`.
