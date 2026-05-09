---
name: create-adr-from-conversation
description: >-
  Creates Architecture Decision Records (ADRs) under docs/adr/ from the current
  chat or summarized decision context. Applies docs/adr/template.md for
  structure and docs/adr/README.md for project rules. Use whenever the user asks
  to record an architecture or technology decision, write an ADR, document why
  something was chosen or rejected, capture alternatives from a discussion, or
  formalize a decision reached in conversation—even if they do not say "ADR" by
  name.
---

# Create ADR from conversation

## Goal

Produce a new Markdown ADR file that matches this repository’s ADR rules and template, using information from the ongoing conversation (or the user’s explicit summary if the conversation lacks detail).

## Source of truth (read before writing)

1. **Structure and section order** — `docs/adr/template.md`  
   Copy its headings and section layout. Remove HTML comment placeholders from the final file; replace them with real content.

2. **Operating rules** — `docs/adr/README.md`  
   Follow lifecycle, status definitions, naming, alternatives rules, scope, and anti-patterns. If anything in this skill conflicts with `README.md`, **README wins**.

If either file is missing or path differs, ask the user or search the repo for the current ADR docs location.

## Workflow

1. **Confirm scope** — Per `README.md`, ADRs cover technology choices, architectural policy, API design, and other long‑impact decisions—not one-off implementation details or purely local/temporary choices. If the topic is out of scope, say so briefly and suggest a lighter doc (issue, PR description) instead.

2. **Resolve the next filename** — List `docs/adr/*.md` and pick the next four-digit prefix after the highest existing `NNNN-*.md`. Use kebab-case after the number, per `README.md` naming (`0001-use-rest-api.md` style).

3. **Extract from the conversation** — Infer or ask for:
   - Problem / background (Context)
   - The adopted conclusion (Decision) and **final** `Status` (`Accepted`, `Rejected`, `Deprecated`, or `Superseded`)
   - Tradeoffs and operational impact (Consequences: real pros **and** cons)
   - Options that were considered, each with **summary** and **reason not adopted** (required for alternatives that matter)

4. **Write the file** — Fill every section the template defines. Do not leave instructional comments from the template in the committed ADR.

## Status and lifecycle (from README)

- **No `Proposed`.** The ADR is written with a terminal status from the start.
- **`Rejected`** — Still record when a serious option was not adopted; rejection rationale is valuable.
- **`Superseded` / `Deprecated`** — Use as defined in README; link related ADRs when replacing or obsoleting a decision.

## Tone: objective judgment (required)

ADR text should help future readers **reconstruct a fair picture of the decision**, not sell the outcome.

- **Decision** — State what was chosen in clear, neutral language. Avoid superlatives (“best,” “ideal,” “obvious”) unless the conversation established measurable criteria.
- **Consequences → Pros** — List real benefits without stacking adjectives or repeating the same point. One honest line beats three promotional ones.
- **Consequences → Cons** — Always include genuine downsides, constraints, or risks. If the chosen path has no meaningful downside, say what was traded off (e.g. complexity elsewhere, narrower option space) or note “limited downside in current scope” and why.
- **Alternatives** — For each option not adopted, give a factual summary and a concrete reason it was not chosen. Do not straw-man alternatives to make the picked option look better.

If the conversation only contains enthusiasm for one option, still add a short “what we give up” or “what we did not pick and why” so the ADR stays balanced.

## Output

- **Path:** `docs/adr/NNNN-short-description.md` (new file; do not overwrite existing ADRs unless the user explicitly asks to amend a draft before merge).
- **Format:** Valid Markdown matching `template.md` sections.
- **Links:** Add `## Links` entries when issues, PRs, or external references exist.

## Checklist before finishing

- [ ] Filename and numbering follow `README.md`
- [ ] `Status` is one of the allowed final states; no `Proposed`
- [ ] `Alternatives Considered` entries that matter include both summary and reason not adopted
- [ ] Consequences include both pros and cons (or explicit statement when tradeoff is negligible)
- [ ] Wording is factual and proportionate—not advocacy for the chosen path
