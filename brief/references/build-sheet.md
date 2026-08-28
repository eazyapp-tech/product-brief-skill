# The Build Sheet — and the document-altitude ladder

The Brief is one rung. A feature usually moves through three documents, each at a different altitude. Mixing them is the most common authoring failure — a Brief that drifts into tasks, or a Build Sheet that re-argues the why.

```
BRIEF                →  GROUND-TRUTH CONTRACT      →  BUILD SHEET
(why + WHAT)            (schema-true definitions)     (HOW + tasks)
one PM's bet           every number, verified,       what engineering builds,
plain language         file:line-cited, the          ticket-ready
                       layer of truth the
                       Build Sheet is written
                       against
```

- **Brief** — the operator's need and the bet. No engineering shape. (This skill.)
- **Ground-truth contract** *(optional, for data/analytics features)* — every number with one plain-English formula, where it comes from in the data (`file:line`), what it tells the operator, and how it behaves in every mode. Built by verifying against the actual schema, not assumptions. This is where a *correctness pushback* gets locked (hard-rule 12): the code's wrong definition is caught, the right one is recommended, both cited. A "Time × Number matrix" proving every number has a defined behavior in every period mode (no blank cells — an N/A is a decision with a reason) is the coverage test.
- **Build Sheet** — the engineering spec, written *against* the contract. Each fact is carried, not re-derived.

## Writing the Build Sheet

The reader is two or three engineers who will build it top-to-bottom. The bar is the same as the Brief's: simple, scannable, humane — but the content is tasks, not bets.

**Structure that works:**
1. **A short "Data Foundation" section first**, if one thing everything else depends on exists (a keystone table, a shared query, a migration). Engineers need the floor before the rooms.
2. **One task table**, grouped, with exactly four columns:

   | Task | Why | Files / queries | Acceptance check |
   |---|---|---|---|

3. **An "open decisions" section** for the genuine owner's-calls (see below).

**Rules the table lives or dies by:**
- **"Why" is one plain sentence — the reason, never the evidence.** All `file:line` anchors live in the "Files / queries" column. A "Why" cell carrying citations becomes a wall and stops being scannable.
- **Every acceptance check is concrete and testable** — "a bed empty most of the month reads its true low %, not ~100%", not "works correctly". The acceptance column is the most build-ready, most humane part of the doc; earn every cell.
- **Carry facts from the contract, don't re-derive.** Cite the contract section; if the contract is unsure, the Build Sheet doesn't get more sure.
- **De-bold.** Bold marks the thing being built or defined — nothing else. If everything is bold, nothing is.

## Leaving decisions open ≠ hedging

A Build Sheet (and a Brief) may carry decisions that are genuinely the owner's call — *not* to be force-resolved. The line:

- **Hedging** is unowned and lazy: "worth a check," "TBD," "maybe." Resolve, schedule, or drop it. (Hard-rule 4.)
- **Deferring** is owned and deliberate: "you decide the cut-order at kickoff; I recommend X because Y." It has an owner, a recommendation, and a trigger.

Deferring the right things is a sign of judgment, not indecision. Flag them in their own section; never bury a real owner's-call as a settled fact, and never dress a hedge up as a deferral.

## Hand off through the gate

Before a Build Sheet (or any derived doc) reaches a human, run **`doc-handoff-review`** — the fact-check reviewer re-verifies every carried `file:line` against the parent contract (derived docs drift silently), and the readability reviewer checks it's humane to read. A derived doc that hasn't been re-verified against its parent is the single highest-risk handoff.
