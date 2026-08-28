# Brief — Section Template (distilled from DA-01 v0.7)

## Frontmatter (always)

```yaml
---
title: <Feature ID> <Name> — Brief
date: YYYY-MM-DD
tags:
  - rentok
  - brief
  - <domain>
status: Living document · v0.1
owner: <PM name>
time_budget: <e.g., 6-week build cycle>
companion_to: <if any — Build Sheet, PRD, etc.>
---
```

## Sections (in order)

### INFO callout — "What this is / What this is NOT"

3-4 lines. Sets reader expectation.

```
> [!INFO] What this is
> A **Brief** for the <feature> — written before design. What the PM wants the thing to *do*, not how it should look.
>
> **What this is NOT:** an engineering spec or a design doc. Those live in `[[<Feature> Build Sheet]]` and Figma.
```

---

### 1. In one line

2-3 sentences. The bet. Plain language. No arbitrary numbers in the headline.

Pattern:
> [User] runs [problem] blind. To find out [thing they need to know] they [current painful workaround]. Most don't bother — they [default behavior], and [consequence]. This <thing> shows them [the picture] in one glance: [what / what / what]. **[Time budget] to ship V1.**

**Subtitle convention (when the user's primary language differs from the brief's writing language):** follow the headline with a one-line italicized subtitle phrased in the user's own asking-language — the exact question they would ask out loud.

If you can't write that one-line question in the user's voice, the brief is at the wrong altitude or aimed at the wrong user.

If the user and brief share a primary language, the subtitle is optional — use it only when it sharpens the altitude.

---

### 2. The operator

- **Position anchor:** "this <thing> is for [user] when they want to [job] — not when they want to [adjacent job]"
- **Primary persona:** 4-5 lines, with operator-voice quote if available
- **Secondary personas:** 1 line each, demoted clearly
- **Where it sits vs other screens/flows:** "the homescreen says X; the worklist says Y; this <thing> says Z"
- **Hand-off line:** "tapping any row drills into [adjacent screen/flow]" if applicable

If the personas are composite, footnote it. If from real field interviews, footnote source count.

---

### 3. The problem

3 pains, same root cause — written in operator voice (not PM voice).

Pattern:
```
Three pains, same root cause — [the root cause in plain words]:

1. **[Pain 1 in operator words].** [1-2 sentences of detail.]
2. **[Pain 2 in operator words].** [1-2 sentences.]
3. **[Pain 3 in operator words].** [1-2 sentences.]

[The bet's inversion. Not "fix the leak" but "make the picture clear." Recovery follows.]

**Why now:** [1-2 lines on why this cycle, not next.]
```

---

### 4. What [user] does today

3-5 bullets in operator voice. Each bullet = a current workaround. 

Pattern per bullet:
```
- **[Verb-phrase describing what they do].** [Operator-voice quote if available.] [Why it's bad: slow / vague / error-prone / etc.]
```

Closing line (optional if the bullets are self-explanatory): "The <thing> has to beat all four: faster than X, more complete than Y, more honest than Z, less effort than W."

---

### 5. What we must ship — and what we cut if time runs short

Three buckets. Direct decisions. No hedging.

```
**Must ship (V1 is not V1 without these):**
- **Q1 — [thing].** [1 line.]
- **Q2 — [thing].** [1 line.]

These [explain why these two are the core bet]. If we can't ship them, we don't ship.

**Nice to have (ship if time allows):**
- **Q3 — [thing]**
- **Q6 — [thing]**
- **Q7 — [thing]**

These answer [the next-tier question].

**If time runs short, cut in this order** *(ranked by operator-pain, NOT by engineering-cost — see SKILL rule #11)*:
1. **Q9** — [why the operator would miss this least: rare event / already in another flow / informational not action-driving].
2. **Q4** — [next-lowest operator pain, why].
3. **Q8** — [next, why].
4. **Q5** — [next, why].

Don't touch [must-ship + nice-to-have] until all four above have been cut.

**If pairs in the nice-to-have tier might split** (one keepable, one cuttable), give per-operator-segment guidance:
- Operator with [trait] → cut [item A], keep [item B] — because [pain logic]
- Operator with [other trait] → cut [item B], keep [item A] — because [pain logic]
- Default → keep both, they're paired
```

**Why operator-pain ranking matters.** Engineering will push back on individual cuts based on build cost — that's their job. PM ranking by build-cost preempts their input and produces worse briefs. Two rankings converge during cycle planning; the brief locks the operator-pain one.

---

### 6. What each [question / capability] needs

One line per item. Information requirement only — NO UI words.

Pattern:
```
Each line describes the *information* — not how it should look. **How** the screen shows it is the designer's call. Tapping any row/segment/chart takes the user to [destination].

1. **[Q1 short name].** [Information described.]
2. **[Q2 short name].** [Information described.]
...
```

Forbidden words in this section: button, chart, tab, card, modal, dropdown, slider, toggle, header, banner, hero card, tile, widget, popup.

Allowed (information-need words): number, count, list, signal, split, breakdown, comparison row, band, segment.

---

### 7. What we're NOT building this cycle

3-5 bullets. Only things someone might actually argue for. Implicit / obvious cuts DON'T belong here.

**Frame each cut from the user's perspective, NOT from the build perspective.** The team reads this section; the user is the one not getting the thing — they deserve the rationale.

Pattern per bullet:
```
- No [thing]. [User-side rationale — why the user doesn't need this in V1, or why some other pain matters more right now. Avoid "V2/V3" / "out of scope" / "defer" as the only justification.]
```

Examples (good vs bad framing):
- ❌ "No AI defaulter scoring. V2 / V3." — engineering framing, no user logic
- ✅ "No AI defaulter scoring. Operators ask for clearer numbers first; predictive scoring without trustworthy underlying data would erode confidence."
- ❌ "No portfolio roll-up for Priya. Out of scope."
- ✅ "No portfolio roll-up for Priya. She tells us per-property is where her actual decisions happen; a roll-up is decorative at her property count."

Closing line if needed: "If anything mid-cycle would require [X], cut it."

---

### 8. Traps & risks

Two sub-blocks.

**Traps (decided in advance):** 4-6 bullets, each a pre-resolved decision masquerading as a thing-to-think-about.

**Label each trap as USER or TEAM:**
- **USER** traps = trust risks, value risks, experience risks — things the *user* might feel if not handled. Example: "two homepage tiles disagreeing — operator spots the difference, trust dies."
- **TEAM** traps = build / migration / design / coordination concerns — things the *team* needs to navigate. Example: "partial-paid pattern uses destructive mutation — eng knows, no need to re-model."

Brief should have both, clearly labeled. If all traps are TEAM-labeled, the brief is engineering-served. If all are USER-labeled, build complexity is hidden.

Pattern (plain trap):
```
- **[Trap name in plain words].** [Why it could go wrong] — [decision already made + reason].
```

Pattern (codebase-anchored trap — REQUIRED format for any backend / data claim):
```
- **[Trap name].** [PM claim or risk]
  → Verified in code at `path/file.ts:Lxx` (or `graphify node <id>`) — [actual finding]. [Decision based on that finding.]
```

Example (from DA-01):
```
- **Partial-paid handling: inherits production behavior.** Production already reduces the unpaid amount when a partial payment lands.
  → Verified in code at `src/services/InvoiceService.ts:L342` — destructive `i.amount` mutation pattern (no row-split). The screen inherits this. Do not try to model "this bill is 60% paid" — that's a separate, much bigger feature.
```

If the brief mentions a backend constraint, existing behavior, or required change WITHOUT a citation, the critique pass flags it as a BLOCKER.

**The four risks** — table:
```
| Risk | Read | Mitigation |
|------|------|------------|
| Will users use it? | HIGH/MED/LOW | [evidence] |
| Will they understand it? | HIGH/MED/LOW | [mitigation] |
| Can we build it in [time]? | HIGH/MED/LOW | [list of backend tasks each with file:line / function / graphify node anchor — uncited claims flagged in critique] |
| Is it worth it? | STRONG/MED/WEAK | [why] |
```

**Standard risk prompts (check each — address or explicitly skip):**
- **Data readiness:** Does this feature show data entered by another actor (operator, admin, system)? If so: what happens when that data isn't ready? Specify empty state copy with timing. Decide: block activation until data exists, or let empty states handle the gap.
- **Empty experience:** Does this feature have a permission/visibility system that can hide content? If so: what happens when everything is hidden? Decide: hard block, soft warning, or accept the consequence.

**When to stop and reconsider:**
- **Mid-cycle (week N):** if [signal] → [action].
- **Post-launch:** if [signal] → [action].

---

### Footer

```
**Things to test with operators during the cycle** (don't block kickoff; settle before launch):
- [Specific scheduled validation — owner + week if possible]

**Related docs:**
- Engineering spec (post-design): `[[<Feature> Build Sheet]]`
- Legacy PRD (historical): `[[<old PRD>]]` (if any)
- Codebase ground truth: `[[<ground truth doc>]]`
- Design system + Figma: frame `<id>`

**Key decisions locked:**
- [decision 1]
- [decision 2]
- ...

**Changelog:**

| Date | Version | Change |
|------|---------|--------|
| YYYY-MM-DD | v0.1 | Initial Brief |
```

---

## Sizing guidance

| Feature complexity | Body length | Sections |
|--------------------|-------------|----------|
| Simple (one screen, ≤3 capabilities) | 600-900 words | 5-6 |
| Standard (one screen, 5-10 capabilities, backend changes) | 900-1500 words | 7-8 |
| Complex (multi-screen flow, integrations) | 1500-2000 words | 8 + maybe one extra |

If the brief is shorter than 600 words it's probably too thin. If longer than 2000 it's a PRD trying to escape.

## What to drop if the brief is for a simpler thing

- Risk table → 1 line
- Traps section → merge into "What we're NOT building"
- Three personas → one
- Footer → just changelog
- "What each question needs" section → merge into "What we must ship" if there are < 5 capabilities

## What to NEVER add even for complex things

- A "Decision Log" separate from changelog (merge — one Changelog at footer)
- An "Open Questions" section (decide or schedule)
- A "Maintenance Log" separate from Changelog (merge)
- A breadboard / fat-marker diagram UNLESS the doc is genuinely Shape Up shaped
- A "Methodology" or "Approach" section (this is a brief, not a research report)
- A "Glossary" (use plain words; if you need a glossary, your vocab is wrong)
