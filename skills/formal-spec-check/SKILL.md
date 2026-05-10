---
name: formal-spec-check
license: MIT
metadata:
  version: "1.0.0"
description: >-
  Ships Alloy 6 models, bounded CLI passes, logs, and summaries beside specs to hunt omissions via checks.
  Use when checking Markdown/design docs with Alloy QA, omission sweeps needing counterexamples, or deriving colocated Alloy artefacts—not Alloy-only tutoring sans artefacts or work
  unrelated to authored prose.
---

# formal-spec-check

**USE FOR:** Alloy models beside docs | omission sweeps | bounded CLI + summaries | obligations tied to specs.

**DO NOT USE FOR:** Alloy tutoring only | code work without specs | teams refusing artefacts without rescoping.

Deep detail: [REFERENCE.md](REFERENCE.md).

## Preconditions

Locate **Alloy ≥ 6.2** runnable headlessly (`which alloy` **or** `java -jar org.alloytools.alloy.dist.jar help` listing `exec`/`solvers`). Stop if unresolved. Aim **≤ 60 s** per CLI invocation unless waived and explained in artefacts.

## Output norms

Default **Japanese** for chat prose + `basename.als.summary.md` readers; keep Alloy identifiers ASCII.

## Steps

1. **Sources:** cite repo Markdown/design paths first; pasted-only payloads land inside `formal/from-chat/YYYY-MM-DD-topic-slug/`.
2. **Traceability:** annotate `sig/fact/pred/check` constructs with concise `path + heading` breadcrumbs per REFERENCE.
3. **`check` posture:** obligations live in Alloy checks/asserts; optional `run` sketches only once justified in summaries.
4. **Temporal tier:** Alloy 6 trace/time features strictly after prose explicitly evolves over clocks; summarise horizon/`steps` limits.
5. **Ambiguity ladder:** catalogue `Assumptions (A…)` inside summaries—pause for stakeholder input whenever an assumption would flip verdicts.
6. **Runs:** whole-module `java -jar … exec` (or `alloy6 exec`) at `for 3 → 5 → 8` + Int widening in REFERENCE—stop after SAT, write required logs/summaries, note finiteness caveats on lingering UNSAT.

## Example prompts

- “Run Alloy on docs/features/auth/policy.md and summarise contradictions.”

CLI samples + triage: REFERENCE.md.
