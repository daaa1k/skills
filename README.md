# skills

Waza skill bundles for authoring and evaluating agent skills in this repository.

## Skills

| Skill | Description |
| ----- | ----------- |
| **fix-act** | Remediate failing **GitHub Actions** with `gh`: fetch logs, apply low-risk fixes, **push**, and **watch** until **all checks** on the pushed succeed. Assumes **explicit user invocation** (optional failed run URL, or latest `failure` on the default branch of this `origin` repo). Stops if the working tree is dirty, if the run URL targets another repo, or when changes need product/security judgment—otherwise follows bounded autonomy rules in `SKILL.md`. |

## Prerequisites

- [Waza CLI](https://github.com/microsoft/waza) — see [CLI reference](https://microsoft.github.io/waza/reference/cli/)
- Optional: [GitHub CLI](https://cli.github.com/) (`gh`) when using **fix-act** for real runs

## Authoring and evaluation

1. Create a new skill:
   ```bash
   waza new skill my-skill
   ```

2. Edit the skill under `skills/<name>/SKILL.md` and eval tasks under `evals/<name>/`.

3. Run evaluations:
   ```bash
   waza run                  # all suites
   waza run fix-act          # one skill
   waza run evals/fix-act/eval.yaml
   ```

4. Check skill compliance:
   ```bash
   waza check
   waza check fix-act
   ```

5. Push to `main` (or open a PR); CI runs evaluations when `skills/**` or `evals/**` change.

## Installing skills with Microsoft APM

[Agent Package Manager (APM)](https://microsoft.github.io/apm/) installs skills from git and deploys them into the agent runtime layout (for example **`.agents/skills/`** for Cursor, Copilot, OpenCode, Codex, and Gemini, or **`.claude/skills/`** for Claude).

### Install APM

Follow the [Quick Start](https://microsoft.github.io/apm/getting-started/quick-start/) for your platform. You need the `apm` binary on your `PATH`.

### Install a skill from this repo

Skill sources live at **`skills/fix-act/SKILL.md`** relative to this repository’s root. APM uses **`owner/repo/<path-to-skill-directory>`**:

```bash
apm install <github-owner>/<github-repo>/skills/fix-act
```

Example for a repo hosted at `https://github.com/your-org/skills`:

```bash
apm install your-org/skills/skills/fix-act
```

APM copies the skill into your runtime skills root (for example `.agents/skills/`) and can pin the revision in `apm.lock.yaml`. If your fork uses a different layout, see the [Skills guide](https://microsoft.github.io/apm/guides/skills/).

### Further reading

- [APM skills guide](https://microsoft.github.io/apm/guides/skills/)
- [Dependencies and locking](https://microsoft.github.io/apm/guides/dependencies/)
