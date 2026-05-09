# ADR (Architecture Decision Record) Operating Rules

## Purpose

Record important decisions and their rationale
to reduce future re-evaluation and onboarding costs.

---

## Basic Principles

- Manage ADRs in Git as Markdown files
- ADRs record the outcome of a decision
- Discussion may be distributed, but conclusions must be consolidated in the ADR
- Both humans and coding agents may author ADRs

---

## Directory Structure

```text
docs/adr/
  0001-xxx.md
  0002-yyy.md
```

---

## ADR Lifecycle

1. Create an ADR with the author's current final conclusion
2. Open a PR and review it
3. Before the PR is approved, revise the ADR if review feedback changes the conclusion
4. PR approval = ADR approval
5. After merge, preserve the approved decision as history

Note: `Proposed` is not used.

---

## Status Definitions

In this project, `Status` represents only the final conclusion.

- `Accepted`  
  A decision that has been adopted

- `Rejected`  
  A decision that was considered but not adopted  
  (including decisions made by humans and coding agents)

- `Deprecated`  
  A decision that was previously adopted but is now discouraged  
  (including cases where there is no clearly defined replacement)

- `Superseded`  
  A decision that has been explicitly replaced by another ADR  
  (the two ADRs should link to each other)

---

## How to Use Each Status

- `Rejected`: The option itself was not adopted
- `Deprecated`: The option was previously adopted but is now discouraged
- `Superseded`: The option was replaced by a newer ADR

---

## Required Rules

### 1. An ADR must contain a conclusion when it is created

- `Status` must always be written as a final state
- `Proposed` is not allowed

---

### 2. A PR is for reviewing the validity of the decision

- Before approval, the ADR is still under review, so revising the content and `Status` is allowed
- Treat the merged ADR as the first approved historical record
- After merge, do not rewrite one final conclusion into another in place
- If a later conclusion changes the decision, create a new ADR or use `Deprecated` / `Superseded` as appropriate

---

### 3. `Rejected` decisions must always be recorded

- The reason for rejection is important knowledge
- It helps prevent the same discussion from recurring

---

### 4. `Alternatives` must include the reason for rejection

For each option, always include:

- Summary
- Reason it was not adopted

If there is no reason, the ADR has limited value.

---

### 5. Handling minor edits

The following may be updated freely:

- Typo fixes
- Non-semantic wording improvements that do not change the meaning
- Added links

Note: Direct pushes without a PR are allowed only for non-semantic edits, depending on team policy.

---

### 6. Handling major changes

Create a new ADR for the following:

- Changes to a decision
- Reconsideration of a technology choice
- Architecture changes

---

## ADRs by Coding Agents

- Decisions made by agents may also be recorded as ADRs
- Options that were not adopted should remain as `Rejected`

### What to Record

- Options that were sufficiently considered
- Decisions that are meaningful enough to document

### What Not to Record

- Options that were only lightly considered
- Temporary or highly local decisions
- Fine-grained implementation-level branches

---

## Using `Deprecated` / `Superseded`

### When to use `Deprecated`

- A better approach exists
- The decision is planned for gradual retirement

### When to use `Superseded`

- The decision has been explicitly replaced by a new ADR

Procedure:

1. Create a new ADR
2. Change the old ADR's `Status`
3. Add a link to the new ADR

---

## ADR Scope

In scope:

- Technology choices
- Architectural policies
- API design
- Decisions with long-term impact

Out of scope:

- Implementation details
- Temporary decisions
- Small changes

---

## Naming Convention

`0001-short-description.md`

Examples:

`0001-use-rest-api.md`  
`0002-adopt-clean-architecture.md`

---

## Relationship to Sprints

- Create ADRs during a sprint
- Finalize decisions through PR review

---

## Anti-Patterns

- Not writing the reason an option was rejected
- Making ADRs too long
- Ending discussions verbally without recording them
- Leaving raw fragments of thought as-is

---

## Operating Guidelines

- Prioritize continuity over perfection
- Aim to write one within 10 minutes
- Prioritize recording the "why"

---

## Notes

An ADR records the outcome of a decision,
not the entire thought process itself.

However, `Rejected` entries preserve the history of what was considered.
