# fix-act — `gh` reference

## Latest failure on default branch (`failure` only)

```bash
owner_repo=$(gh repo view --json nameWithOwner -q .nameWithOwner)
default=$(gh repo view --json defaultBranchRef -q .defaultBranchRef.name)
gh run list --repo "$owner_repo" --branch "$default" \
  --limit 20 --json databaseId,conclusion,status,workflowName,url,createdAt \
  --jq '.[] | select(.conclusion=="failure")'
```

Take the newest `databaseId`.

## Inspect a run

```bash
gh run view RUN_ID --repo OWNER/REPO --json url,name,headBranch,headSha,event,workflowName,conclusion
gh run view RUN_ID --repo OWNER/REPO --log-failed
```

(Optional) Annotations: `gh api repos/OWNER/REPO/actions/runs/RUN_ID` plus jobs.

## After push: watch workflows

```bash
gh run list --workflow "WORKFLOW_FILE.yml" --branch HEAD_BRANCH --limit 5
gh run watch RUN_ID --exit-status
```

## All check runs on a commit

```bash
gh api repos/OWNER/REPO/commits/SHA/check-runs --paginate
```

Fold by check name; any conclusion in `failure`, `timed_out`, `action_required` blocks “all green”. `skipped` and `neutral` are OK. If PR exists: `gh pr checks <NUMBER>` as a summary.

## PR from default-branch fix branch

```bash
gh pr create --title "fix(ci): …" --body "Fixes failed run: RUN_URL" --base DEFAULT --head FIX_BRANCH
```
