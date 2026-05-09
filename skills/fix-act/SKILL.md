---
name: fix-act
description: >
  Use when the user explicitly invokes fix-act to repair GitHub Actions—optional failed run URL, or
  latest failure on default branch here. Uses gh: logs, CI/test patches, push, watch until every check on
  that commit passes. Not for unrelated tasks or other repos until origin matches. Requires gh auth
  and a clean git working tree.
license: MIT
metadata:
  version: "1.0.0"
---

# fix-act (**UTILITY SKILL**)

**USE FOR:** saying `fix-act`, sharing a failed Actions URL, or “fix latest CI failure here.”

**DO NOT USE FOR:** topics outside Actions/this repo, or a run URL for a different `owner/repo` than workspace `origin` without user confirmation.

## Preconditions

`gh auth status` OK; `git status --porcelain` empty (else stop; user cleans tree).

## Run selection

- **URL:** parse id + repo; if repo ≠ `gh repo view --json nameWithOwner -q .nameWithOwner`, **stop**.
- **No URL:** newest `conclusion=failure` on default branch (`cancelled` ignored).

## Investigate

`gh run view <id> --log-failed`; map errors to workflows/code. More commands: [REFERENCE.md](REFERENCE.md).

## Autonomy

Commit/push **without** asking only for singular, low-risk fixes (workflow typos, wrong paths, obvious test expectation drift, patterns already used in-repo). **Ask first** for security, secrets, major upgrades, removing required jobs, or multiple plausible fixes.

## Push strategy

If failing run’s `headBranch` is the default branch → create `fix/ci-<topic>-<runid>`, push, open PR (cite run URL). Else push directly to that feature branch.

## Watch until all checks pass

On the new commit: blocking = `failure`, `timed_out`, `action_required`; OK = `success`, `skipped`, `neutral`; `cancelled` ≠ done. Wait up to **60 minutes** per push; at most **5** remediate cycles per incident; if another workflow fails on the same commit, continue inside those limits.

## Safety

Do not print secrets; do not weaken required checks without explicit approval.
