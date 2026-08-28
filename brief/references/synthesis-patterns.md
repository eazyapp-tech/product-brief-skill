# Synthesis-altitude patterns

Five tactics specific to **synthesis-altitude** briefs — features that present multiple cuts of the same data so a user can answer *"what shape is the problem?"*. Periodic / triggered use, not daily ritual.

**Do NOT apply these to Signal, Action, or Authoring briefs.** They were derived from synthesis-screen work; forcing them onto other altitudes bloats the brief without sharpening the bet.

Load this file only when Phase 1 altitude check returns **Synthesis**.

---

## Pattern 1 — Aspire-for-all-N lead-line · template §5

Synthesis briefs typically have N tiered information-need questions (Q1, Q2, … Qn) split into must-ship / nice-to-have / cut-if-time. Without an explicit aspire line, engineers read the tiers as planned scope cuts.

Add above the Must-ship block:

```
**The bet is to ship all N questions.** The tiers below describe graceful
degradation if the cycle slips — not the planned scope. Cut order applies
only when time forces a choice.
```

This keeps operator-pain ranking as the anchor and the tiers as contingency.

---

## Pattern 2 — When a question splits further · template §5

When a question can be sub-divided (categories, sub-types, source-splits, status-splits), the sub-division must justify itself or get dropped. Three valid resolutions:

- **Domain-impossible** — the entity is a state, not an event; or has no meaningful sub-divisions. Drop the layer, name why, move on.
- **Deferred with anchor** — bundle the sub-division with a larger future scope and name the anchor (the upstream feature that makes the defer non-arbitrary). Never defer without an anchor.
- **None needed** — the signal is already carried by an adjacent question or a downstream drill. Don't add a parallel cut.

A sub-division that fits none of these three is mis-scoped — re-examine the information need before inventing a fourth resolution.

---

## Pattern 3 — Paired-cut axes · template §5 + critique Lens 4

When two stretch-tier items might split (one keepable, one cuttable), add per-user-segment guidance. The segment axis varies by feature — pick the axis that actually divides the user base for THIS feature. Common axes:

- **Capacity axis** — solo user vs. staffed / multi-person team
- **Tooling axis** — user with an existing workaround vs. user without one
- **Role axis** — owner vs. operator vs. caretaker (when one role authors, another reads)
- **Scale axis** — single-entity user vs. multi-entity user

If you can't name the axis that divides your user base for this specific feature, you don't have a paired-cut — you have a single-tier cut. Don't force the structure.

---

## Pattern 4 — Visibility-cost quantification · template §3 (Why now) or §4

When the user's workaround runs on a fixed cadence (weekly report, end-of-period review, manual reconciliation pass), the cost of being blind between refreshes is measurable. Quantify it.

One useful formula (when cadence-based):

```
cadence interval × per-entity frequency = visibility moments lost per period
```

A number anchors "why now" against a real cost — not a vague "they don't see it well." Other quantification approaches (decision latency, error rate, opportunity cost) work when the cadence formula doesn't fit; the point is to anchor with a measurable, not which measurable.

If the user has no workaround at all (information is fully absent), this doesn't apply — use the no-aggregate framing in Pattern 5.

---

## Pattern 5 — Workaround-state framing · template §4

The "what user does today" section's bullets should reflect which **workaround state** the user is in. Three common states (not exhaustive — flag if you observe a fourth):

- **No aggregate view** — the information is fully absent or scattered with no roll-up. Pain is **no-ceiling**; the user can't form an opinion at all. Bullets emphasize blindness.
- **Async / batched workaround** — the information exists but on a cadence (weekly export, manual roll-up). Pain is **time-lag ceiling**; the user sees the number, but the decision window has passed. Bullets emphasize the lag, not the absence.
- **Fragmented partial views** — the information exists in multiple places but no place reconciles them. Pain is **trust ceiling**; the user sees pieces but can't trust the sum. Bullets emphasize the mental math, not the missing data.

If §4 doesn't reflect which state the user is in, the bet is mis-anchored — the screen will either over-promise (treating a fragmented user as blind) or under-deliver (treating a blind user as already-batched).

---

## When NOT to apply these patterns

- Signal briefs (homescreen / badge / push) — operator's job is "is anything broken right now?", not "what shape is the problem?". The tiers, sub-divisions, and workaround-state framings don't transfer.
- Action briefs (worklist / call-list / inbox) — operator's job is per-item action. Aspire-for-all-N doesn't apply; cuts work item-by-item.
- Authoring briefs (form / editor / settings) — operator's job is to create or modify, not to consume an aggregated view. Visibility-cost formula doesn't apply; workaround states don't fit.
- Commercial / scope / gatekeeper briefs (LM-style) — different shape entirely; force-fitting these tactics will add noise.

If unsure, re-run Phase 1 altitude check.
