# formal-spec-check — Reference

## Repository layout conventions

### Colocated authoring model

Given a spec **`path/to/feature/requirements.md`**, emit **`path/to/feature/requirements.als`** beside it (**shared basename**, **paired extension**).

- **Variants** — `requirements.alt-auth.als`, `requirements.experiment-scope.als`, …
- **Summaries / logs** — `requirements.als.summary.md` plus **`requirements.for-<N>.alloy.log`** for global scope **`N`** ∈ {**3**,**5**,**8**}. Replace `<N>` literally (e.g. `requirements.for-5.alloy.log`).

When multiple `.als` files share a baseline name, prepend the variant stem (**`requirements.alt-auth.for-5.alloy.log`**) so filenames stay unique.

### Chat fallback root

Paste-only inputs land under **`formal/from-chat/YYYY-MM-DD-<topic-slug>/`**:

- **`topic-slug`** — Lowercase hyphenated gist; prioritise explicit user wording, else synthesise ≤ **40** chars (`authz-matrix-review`).
- Populate **`intent.md`** (raw paste) unless the user forbids persistence, then scaffold `.als`, logs, summaries per the basename rules (**`intent.als`**, …).

## Traceability expectations

Annotate Alloy constructs:

```text
// Trace: docs/auth/policy.md#Lifecycle — section "Delegation"
sig Principal { delegates: set Principal }
```

Minimum link payload: **`path` + anchor** (Markdown heading slug, `#` fragment, heading text, line span). Keep comments short; expand rationale in **`*.als.summary.md`**.

Prioritise **named `pred` wrappers** referenced by **`check`** blocks so summaries can cite stable symbols.

## Modeling posture

### Checks first

Draft obligations as **`pred obligation_foo[h] { … }`** or inline **`check`** bodies. Each materially distinct requirement **owns** a **`check`** (or clearly named wrapper) so summaries can expose coverage matrices.

Reserve **`run`** for scenario sketches / setup assistance—not the primary QA surface.

### Temporal / Alloy 6 trace mode (β lane)

Activated **only when** prose explicitly mandates evolving state or ordered traces. Mandatory summary bullets:

1. Horizon / `steps` pulled verbatim from Alloy commands (**no supplemental fixed ladder beyond user/model choices** unless the stakeholder requests it separately).
2. Known limitations (**bounded horizon**, abstraction gaps).
3. Pointer to Alloy sections mirroring prose timing assumptions.

### Ambiguity triage pipeline

Default: catalogue unset decisions under **`Assumptions`** inside **`*.als.summary.md`** (bullet list referencing spec gaps). Pause and ask stakeholders **when** flipping an assumption swings compliance conclusions (violations ⇄ passes) or forks mutually exclusive behaviours.

Document each assumption ID (`A1`, …) reused across model + prose.

### Integer domains

Mirror this ladder alongside global scopes:

| Pass | Alloy `Int` scope |
| --- | --- |
| 1 | `none`/`0..3` (choose minimal viable) |
| 2 | widen to **`0..7`** if analyses require richer arithmetic |

State which pass ran inside both logs and summaries.

### Global scope ladder

For each **`check`** / command block:

| Stage | Typical command suffix | Notes |
| --- | --- | --- |
| 1 | `for 3` | Fast smoke |
| 2 | `for 5` | Medium |
| 3 | `for 8` | Deeper fuzz |

**SAT / counterexample** → stop escalating **that ladder** after logging + interpreting failures; unresolved obligations stay tracked in summaries.

Prefer **whole-module executions** (`java -jar … exec … file.als` or `alloy6 exec …`) so every obligation runs per attempt; specialised single-command tooling is optional only when CLI constraints demand it **and** summaries narrate gaps.

Hard-stop each CLI attempt around **≈60s**; if timeouts occur, rerun with tightened scope/plots or escalate to stakeholder—note reason in summaries.

### Finishing caveat

Successful **`UNSAT` across ladders** ⇒ **still bounded**. Summaries **must** restate Alloy’s finiteness + abstraction limits (no unchecked “proof complete” wording).

## CLI invocation cheat sheet

1. Locate binary or JAR (**Alloy ≥ 6.2** exposes `exec`, `solvers`, etc.). Example probes:
   - `command -v alloy && alloy exec -h`
   - `java -jar org.alloytools.alloy.dist.jar solvers`
2. Solver selection: inherit project defaults (`sat4j`-class) unless the environment demands another backend—record choice in summaries if non-default.

```bash
java -jar org.alloytools.alloy.dist.jar exec -f -s sat4j /abs/path/requirements.als > requirements.for-3.alloy.log
```

Adapt flags to the distributor’s synopsis (`--help`). Capture **verbatim** command lines per log footer.

Logs should include timestamps, Alloy build string, scopes, SAT/UNSAT outcomes, truncated counterexample tables (+ links to Alloy UI export if handy).

### Environment failures

Explain missing JVM, inaccessible JARs, SAT timeouts, corrupt models succinctly (**Japanese** summaries) alongside remediation hints (PATH updates, reinstall, scope reduction).

## `*.als.summary.md` skeleton

Use Japanese headings mirroring stakeholder language; keep Alloy identifiers verbatim.

```markdown
# Alloy summary — `<stem>.als`

## Coverage matrix
| Spec reference | Alloy obligation | Result (3 / 5 / 8 / Int passes) |
| --- | --- | --- |

## Commands executed
ordered shell snippets + rationale

## Assumptions (`A1`…)
explicit bullet list tying back IDs used in Alloy comments

## Findings / counterexamples
readable narrative + artifact pointers (`*.log`)

## Residual risks
bounded analysis, undone obligations, unanswered questions for stakeholders

## Appendix — trace excerpt
minimal fragments copied from specs (optional)
```

## Anti-patterns

- Silent defaults that flip pass/fail classifications without **`Assumption`** linkage.
- Skipping **`check`** coverage tables when multiple obligations exist.
- Mixing contradictory temporal models without stakeholder acknowledgement.
- Deleting unsuccessful logs (**keep failures**—they anchor debugging).

## Institutional memory

Permanent engineering policy deltas belong in `./docs/adr/` (`AGENTS.md`). Keep SKILL lean; escalate repeated governance debates into ADRs.
