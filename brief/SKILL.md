---
name: brief
description: Write a Product Brief — PM intent before design, before PRD. One opinionated person's bet, in plain language, no AI slop. Use when starting a new feature, screen, workflow, or product where the WHY needs to land before the WHAT and the HOW. Not a PRD — sits upstream. Routes on "brief", "product brief", "operator brief", "PM brief", "/brief", or any "I want to land what we're building before designing".
---

# Brief — Product Brief writing skill

## Write in plain language (always)
**The test (apply to every word):** can the reader rebuild this from concepts they already own, without your dictionary? If a word only resolves for someone who shares your context, replace it or gloss it on first use. Simplifying is decompression toward shared words (first-principles in reverse), not dumbing down. The banned list is just this test's common outputs.

**The thin line — the test cuts both ways.** Decide by *whose* dictionary the word lives in, not how hard it looks. Unpack words that need *your* (author's) dictionary (the banned list, plus the skill's own working vocabulary below); but **leave untouched words already in the *reader's* dictionary.** For a domain audience the domain's own vocabulary is shared ground, not jargon (property/RentOk: vacant, occupied, semi-vacant, under notice, booking, overbooked, occupancy, tenant, bed, room). Rewriting "occupied" → "someone living in it" is over-simplifying — it adds noise and talks down.

Plain words — if a plain-vocab, non-native engineer would stumble on a word, replace it. Banned: load-bearing, intrinsic, canonical, denominator, run-rate, altitude, leaf, primitive, cohort, reconcile, downstream, net-new, keystone, overlay, survivorship. Short sentences, no fluff. Bold only the term being defined — if everything is bold, nothing stands out. No code (briefs say WHY, not how). A teammate reads it once and gets it. Full standard: memory `feedback_plain_language_docs`.

**Do not leak the skill's own working vocabulary into the brief body.** These words may help the writer think, but they usually do not help the reader understand. Treat them as banned in the brief body unless the product itself uses them: operator, surface, flow, drill, handoff, contract, synthesis, aggregate, auth, authenticated, framework, artifact, altitude. Replace them with ordinary product words such as user, team member, collection list, payment details page, money that reached the bank, what this page helps people understand.

**Reader target:** a smart first-time reader with plain vocabulary. If they would ask "what does that mean?" after one pass, rewrite the sentence before it reaches the page.

## When to invoke

Any of these:
- "/brief" or "/product-brief" or "/operator-brief"
- "write a brief for X"
- "I want to land what we're building before designing"
- "what's the brief for [feature/screen/product]?"
- Starting a new feature/screen/workflow and the PM intent isn't written down
- An existing PRD or Build Sheet exists for X but there's no upstream brief — offer to retroactively extract one

## What this is — and isn't

**Is:** the doc a single PM writes BEFORE design, BEFORE the PRD. One opinionated bet, in plain language. Position in lifecycle:

```
Brainstorm → Discovery → BRIEF (this) → Design → Build Sheet → Execute
```

The Brief is one rung of a three-altitude ladder — **Brief (why + WHAT) → ground-truth contract (schema-true definitions) → Build Sheet (HOW + tasks)**. Mixing altitudes is the most common authoring failure. For how to write the downstream Build Sheet (and the contract that sits between), and the deferring-vs-hedging line, see `references/build-sheet.md`. Hand any of these off to a human through the `doc-handoff-review` skill.

**Is NOT:**
- A PRD (longer, comprehensive, post-design)
- A Build Sheet (engineering, ticket-ready)
- A design doc (UI prescription — that's designer's job)
- A brainstorm output (still exploratory)
- A research report (no synthesis-of-others; this is one PM's bet)

## Foundational principle (read before every rule)

**The Brief is an artifact for the team. The product is for the users. Don't conflate.**

- The Brief is read by engineers, designers, QA, executives. So the artifact must be clear, cited, actionable, plain-language *for them*.
- The product is *for the user* (operator, end customer, whoever). So every PM decision encoded in the Brief — must-ship priority, cut order, boundaries, traps, why-now, risks — must trace to a user-need or user-pain, not to what's convenient for the team.
- A Brief that's clear to engineering but rooted in engineering convenience = a wrong Brief.
- A Brief that's rooted in user-need but unclear to engineering = an unread Brief.

Both audiences served, neither conflated. The team reads it; the user benefits from it.

If a decision in the Brief reads as "we're skipping this because it's hard to build" or "design doesn't want this" or "this is V2 because we ran out of time" — that's a team-served decision in a user-serving doc. Surface the user-side reason or drop the decision.

Every hard rule below is in service of this principle.

---

## Hard rules (the skill REFUSES to violate these)

1. **No AI slop.** Cut: *leverage, synergy, robust, seamless, frictionless, delight, world-class, comprehensive, holistic, optimize, streamline, empower, facilitate, intuitive.* See `references/vocab-stop-list.md`.
2. **No arbitrary numbers.** Every stat needs a source or footnote. No "increases productivity by 40%" unless 40% has a source.
3. **No jargon from the stop-list.** Replace with plain alternatives.
4. **No hedging in the body.** Every *"worth a check," "TBD," "should we," "might want to"* → decide, schedule, or drop. Never park.
5. **No borrowed-framework name-dropping.** Don't invoke Cagan / Shape Up / Zerodha / Lenny / Razorpay unless the doc is genuinely shaped that way.
6. **No Bollywood-stock personas.** *"Rajesh / Priya / Suresh"* unflagged. If real interviews exist, footnote sources. If invented, say so.
7. **No UI words in the bet section.** *"Button, chart, tab, card, modal"* — that's design territory. Brief stays at information-need altitude.
8. **No mega-doc.** Target 6-8 body sections + footer. ~1500 words max in body. If longer, you're writing a PRD.
9. **Wrong altitude = stop.** If the doc is being written at the wrong altitude (e.g., analytics screen written as a worklist), halt and re-anchor. See `references/altitude-check.md`.
10. **No backend / engineering claim without a codebase citation.** Every "small change," "easy filter widen," "just add a query," or backend-task estimate needs a file:line reference, function name, or graphify node ID. PM intent is fine; PM engineering speculation is not. See Phase 2.
11. **Operator-pain ranking, never engineering-cost ranking.** Must-ship priority, cut order, boundary choices, question ordering — all ranked by *what the operator would miss least*, not by *what's cheapest to drop*. Engineering owns build-cost; PM owns operator-pain. The brief locks operator-pain; build-cost converges during cycle planning. If a cut order reads as "drop the heaviest backend task first," that's an engineering brief in PM clothing — flag it.
12. **Correctness-pushback authority — fix wrong definitions, don't inherit them.** When Phase 2 grounding reveals that the code's own definition is wrong (a misnamed state, a metric computed two ways, a label that lies to the operator), the brief has authority to flag it and recommend the fix — it does not silently inherit the error to "match the code." State the current behavior with its file:line, state the corrected definition, and mark it as a deliberate pushback (not a bug to route around). Example: code names living-tenants-over-capacity "overbooked"; the brief renames it "over-occupied" and files the correction, because shipping the wrong word on a trust screen is itself the risk.
13. **No internal PM/spec vocabulary in the brief body.** If a reader-facing section says "operator", "surface", "flow", "drill", "handoff", "contract", "synthesis", "aggregate", or similar internal writer-language, rewrite it in ordinary product words unless the product itself uses that exact term.

## Workflow (5 phases)

There's a deep separation between **mechanical hygiene** (vocab, hedges, repetition, arbitrary numbers, team-served framings — caught at *write-time*) and **substantive critique** (altitude, user-need rooting, operator-pain ranking, decision quality, codebase grounding — reviewed *after* the draft is clean). Mechanical hygiene runs *inside* Phase 4, never as backfill. Phase 5 stays for substance only.

Backfill scrubs correct but don't teach. Built-in hygiene prevents and normalizes. The PM never sees their slop because it never reaches the page — but a `scrub-log` surfaces what was caught so the PM still learns.

### Phase 1 — Altitude check (BEFORE anything else)

Read `references/altitude-check.md`. Identify whether the thing is:
- **Signal** (homescreen-style — "anything broken right now?")
- **Synthesis** (analytics-style — "what shape is the problem?")
- **Action** (worklist-style — "which exact item do I act on?")
- **Authoring** (editor-style — create/modify entities)

Wrong altitude = wrong brief. Lock altitude before proceeding.

**If altitude = Synthesis,** also load `references/synthesis-patterns.md` — five tactics specific to tiered-question synthesis screens (aspire-for-all-N framing, secondary-cut taxonomy, paired-cut axes, visibility-cost quantification, workaround-state framing). These do NOT apply to Signal / Action / Authoring briefs.

### Phase 2 — Codebase grounding (REQUIRED if a codebase exists)

A brief that's ungrounded in the actual code becomes PM fiction. Engineering reads it, pushes back, and the cycle slips. Catch the constraints before the draft.

**Skip only if:** the feature is greenfield with no existing code (rare for RentOk briefs).

**Two-layer exploration — both layers required, in order:**

This is the most-skipped phase. The failure mode: PM grounds the direct entity but misses the *operator's full mental domain*. Result: a Brief that's correct for the entity but wrong for the operator.

---

**LAYER 1 — Direct entity grounding** (the obvious one)

1. **Check for graphify graph** at `<project-root>/graphify-out/graph.json`.
   - **If present:** run targeted queries on the direct entity:
     - `graphify query "<feature domain keywords>"` — BFS view of the direct module
     - `graphify explain "<key entity>"` — plain-language explanation
     - `graphify path "<entity A>" "<entity B>"` — connection paths if known
   - **If absent:** suggest running `/graphify` first. If user declines, fall back to targeted `grep` on key entity names.

2. **Read the direct module** (entity / service / controller / routes / helpers) for the screen's primary entity. Capture: hidden patterns (partial-paid, soft-delete, status enums, destructive mutations), permission gaps, performance constraints.

---

**LAYER 2 — Domain dependency mapping** (the one that gets skipped)

**The Build Sheet's scope is engineering-bounded. The Brief's scope is operator-bounded. The two are NOT the same.** An operator's mental model usually spans 2-4 engineering modules. LAYER 2 catches the modules LAYER 1 missed.

For every brief, expand outward from the direct entity:

3. **Related services in the same operation-factory pattern.** Many RentOk modules cluster as `TeamPassbook<X>Service` factories — `Expense / Collection / Refund / Reimbursement / Handover`. If the direct entity is one of these (or has any cross-entity transactional behavior), enumerate the siblings:
   - `find src/services/<domain>/ -type f -name "*.ts"`
   - `grep -rn "class.*Service" src/services/<domain>/`
   - Pattern: for any Service X in a factory directory, ALL siblings are candidates for dependency.

4. **Cross-entity flow tables** (transactions, fund balances, lifecycle states, audit snapshots):
   - Transaction tables: `team_member_transactions`, `payment_transactions`, etc.
   - Fund tables: `team_passbook`, `account_details`, `wallet_payouts`, `settlement_scheduler`
   - Audit snapshots: `deleted_<entity>`, `<entity>_history`, `<entity>_audit`
   - State machines: `<entity>_status` enums, `<entity>_state` columns

5. **Lifecycle / settlement tables** (what happens AFTER the entity is created):
   - For payments: `settlement_scheduler`, `wallet_payouts`, `payout`
   - For refunds: gateway-refund tables (if any), `deleted_refunds`
   - For expenses: passbook transactions, reimbursement allocations
   - Search: `grep -rn "<entity>_id.*FK" src/entities/`

6. **Operator-facing endpoints in the same domain** (what operators ALREADY see today):
   - `grep -rn "router\\.\\(get\\|post\\)" src/routes/<domain>*.ts`
   - List every endpoint the operator can hit. Some may be:
     - Adjacent screens (passbook detail, fund balances, admin ledger) — the new Brief should consume or hand off, not duplicate
     - **Dead code** — service body commented out, returns `{}`. Brief shouldn't rely on these (LAYER 1 wouldn't catch this).
     - **Hardcoded-empty UI blocks** — `reimbursement_details: []` returned from controller; the UI shell exists but is never populated.

7. **Graphify community-neighbor expansion.** `graphify query` returns nodes with `community=N`. Same community = code that clusters together. Widen the query set:
   - After the LAYER 1 query, note the dominant `community=N` of returned nodes
   - Re-query for ANY service / entity in that community to find adjacent modules
   - Example: querying "Expenses" returns nodes in `community=8`. Re-query for "team passbook" + filter community 8 → catches the passbook ecosystem.

---

**Capture two outputs:**

8. **LAYER 1 output: codebase-grounded entity facts** with file:line citations. Feeds:
   - **Traps section** in Phase 4 draft (each fact becomes a pre-decided trap)
   - **Risks "Can we build it?"** row (real backend tasks, not speculation)
   - **Boundaries** (things existing code already handles — don't re-build)

9. **LAYER 2 output: Domain Dependency Map** — written as a separate artifact (`<feature> V2 Dependency Map.md` or inline in Phase 3 gather prep). Feeds:
   - **Phase 3 gather questions** — adjacency questions ("Does the operator also think about reimbursement when they think about expenses?") that wouldn't otherwise be asked
   - **V2 scope** — what's deferred from V1 with codebase grounding for why
   - **Anti-duplication** — adjacent endpoints the V1 screen should consume / hand off to, not parallel

---

10. **Block ungrounded backend claims.** Any "small change," "easy widen," "just add a query," "should be straightforward" in the draft without a citation gets flagged in Phase 5 as a BLOCKER until grounded or removed.

11. **Flag LAYER 2 thinness.** If LAYER 2 returns < 3 dependency findings, the skill warns: *"shallow codebase exploration — domain may be wider than the direct entity. Widen LAYER 2 before drafting."* For RentOk financial screens, expected LAYER 2 surface area is 5-10 dependencies (sibling services + cross-entity tables + adjacent endpoints).

### Phase 3 — Gather (interactive, one Q at a time)

Ask 6-8 questions via `AskUserQuestion`. Skip any answer that's obvious from context — don't grind for show.

The eight gather questions:

1. **The operator.** Who's the primary user — real, composite, or invented? Their literacy / language / vocab level?
2. **The trigger.** When / why do they open this thing? What's the frequency?
3. **The symptom.** What do they complain about today?
4. **The root cause.** What's structural underneath the symptom? (Push past surface — don't accept the first answer.)
5. **The current alternate.** What 3-5 things do they do today to solve this, in their own voice?
6. **The time budget.** How long do you have for this build?
7. **The boundaries.** What are 3-5 things you're NOT building this cycle — that someone might actually argue for? (Implicit cuts don't count.)
8. **The hand-offs.** How does this relate to other screens/flows? Where does it drill or hand off to?

### Phase 4 — Draft (with built-in mechanical hygiene)

Produce the 8-section structure. See `references/template.md`. Plain language, decisions not negotiations, operator voice in operator section, question-first information needs in the bet section. Codebase facts from Phase 2 flow into Traps and Risks with file:line citations. **If altitude = Synthesis, also apply `references/synthesis-patterns.md`.**

**Mechanical hygiene is enforced AT WRITE-TIME, never as backfill.** As each section is drafted, these checks run *before* the section reaches the page:

1. **Vocab stop-list** — words from `references/vocab-stop-list.md` (AI slop, jargon, idioms, team-served framings, engineering-effort hedges) never get written. If a stop-listed word is genuinely the right word in context (rare), it's used AND surfaced in the scrub log.
2. **No hedges in the body** — every "worth a check," "TBD," "should we," "might want to" is either resolved (decision) or moved to the footer (scheduled task) at write-time. Hedges never land in the body.
3. **No repetition** — the same phrase / metaphor doesn't appear 3+ times in the draft. Caught at write-time, varied or collapsed.
4. **No arbitrary numbers** — every number in the draft must have a source or footnote. Sourceless numbers don't get written; PM intent is paraphrased instead.
5. **Operator-pain rationales only** — every cut, defer, or boundary is justified from user perspective at write-time. "Out of scope" / "V2" / "defer" without a user-side reason never reaches the draft.
6. **USER vs TEAM trap labels** — traps in the Traps & Risks section are tagged as USER (trust/value/experience) or TEAM (build/migration/design) at write-time, not retro-fitted.

**Output:** the draft + a `scrub-log` block (separate from the brief body) listing what got caught and how it was rewritten. Format:

```
## Scrub log (Phase 4)
- [hedging] "we should probably ship X" → "ship X" (decided)
- [vocab] "leverage" → "use" (3 instances)
- [team-framing] "out of scope" → "operators don't need this in V1 because Y"
- [arbitrary number] "8% leak" → dropped; PM estimate, no source
```

The PM reads the scrub log once. Learns what would have been written wrong. The brief stays clean.

**After write-time hygiene, run the thorough mechanical audit (always, every draft):**

The audit is a deterministic verification pass that systematically checks every mechanical rule against the final draft — catches what the write-time hygiene missed AND tells the skill whether its own discipline held. Same input = same output. No LLM judgment needed for the bulk of it; this is grep / pattern matching against the rule corpus.

**Audit checklist (run every check, every brief):**

1. **Vocab stop-list scan.** Grep the entire draft for every term in `references/vocab-stop-list.md` — AI slop, borrowed framework jargon, PM-speak, idioms, Bharat-context banker words, engineering-effort hedges. Each hit = violation.
2. **Hedging pattern scan.** Search the body (not the footer) for: *"worth a", "TBD", "should we", "might want to", "could be", "perhaps", "let's discuss", "needs further thought"*. Each = violation.
3. **Arbitrary-number scan.** Every number / percentage / time-estimate in the body — does it have a footnote, citation, or "PM estimate, not validated" tag? No anchor = violation.
4. **Repetition scan.** Any phrase or distinctive metaphor appearing 3+ times = violation. Includes "go by gut," "rhythm breaks," "synthesis," etc.
5. **UI-word scan in bet section.** Search the "What we must ship" + "What each question needs" sections for: *button, chart, tab, card, modal, dropdown, slider, toggle, banner, hero, tile, widget, popup*. Each = violation.
6. **Word + section count.** Body word count > ~1500, OR body section count > 8 = violation (mega-doc risk).
7. **Persona-footnote check.** Named personas present (Rajesh, Priya, Meena, etc.)? Footnote about composite-vs-real-vs-invented present? Missing footnote = violation.
8. **Backend-claim citation check.** Engineering claims present ("widen filter," "small backend change," "new query," etc.)? Each one has a file:line / function name / graphify node anchor within 2 lines? Missing citation = violation.
9. **Team-served framing scan.** "Out of scope," "defer to V2 / V3," "design has concerns," "backend is complex" — each instance must be followed by a user-side rationale within the same bullet/sentence. Missing user rationale = violation.
10. **Cut-rationale lens.** Cut order list present? Each rationale phrases as operator-pain ("operators would miss this least because…") not build-cost ("cheap to cut," "easy drop"). Build-cost phrasing = violation.
11. **Private-dictionary scan.** Search the final brief body for internal PM/spec words such as: operator, surface, flow, drill, handoff, contract, synthesis, aggregate, auth, authenticated, artifact, framework. Any hit is a violation unless the product itself uses that exact word and the doc makes that obvious.

**Audit output (always produced):**

```
## Mechanical audit — Phase 4
Checks run: 10
Violations caught at write-time (from scrub log): N
Violations caught at audit (missed by write-time): M
Total violations: N+M

Audit-caught violations (auto-fixed):
- [rule X] [section Y] [violation] → [auto-fix applied]
- ...

Skill discipline read: WRITE-TIME N/(N+M) = X%
```

If M > 0 (audit caught what write-time missed), the skill auto-appends to `~/.claude/skills/brief/learnings.md`:

```
### YYYY-MM-DD: audit-gap — <pattern that slipped through>
**Brief:** <feature ID>
**What slipped:** <specific violation>
**Why write-time missed it:** <hypothesis>
**Tighten:** <which rule in Phase 4 hygiene to sharpen>
```

Over many briefs, this learnings log surfaces which rules consistently slip through — those rules need restating, or new examples, or earlier-in-Phase-4 enforcement. **The audit isn't just a safety net; it's a quality signal for the skill itself.**

**The Phase 4 output (after write-time hygiene + audit) is mechanically clean.** Phase 5 (critique) is exclusively for substance — altitude, ranking, decisions, user-need rooting, traps quality.

### Phase 5 — Critique + apply (substantive only)

Mechanical issues are already gone (handled at write-time in Phase 4). This phase reviews **substance**.

Dispatch one critique sub-agent that applies all 6 lenses from `references/critique-lenses.md`:
- **Linear** — opinion strength, length, decisiveness (NOT hedging — that's already scrubbed)
- **Shape Up** — load-bearing call, scope-cut ladder, fat-marker honesty
- **Stripe** — sentence purpose, structural density (NOT vocab — already scrubbed)
- **Operator** — user-need rooting, persona authenticity, team-vs-user-served decisions, cut-order ranking lens, cross-question redundancy, segment guidance
- **Pre-mortem** — most likely failure mode → trace to weakness
- **Codebase Reality** — backend claims grounded in actual code? overlap with existing? hidden constraints?

Sub-agent returns ranked findings (BLOCKER / HIGH / MEDIUM). Skill shows findings, asks user which to apply, applies them.

A final consistency check (does the brief cohere section-to-section, do wikilinks resolve, does frontmatter match changelog) runs as part of this phase — not a separate phase.

## Output

- **File:** Obsidian vault, under `RentOk/PRDs/<area>/` if a related folder exists; else `RentOk/Briefs/`.
- **Filename:** `<feature-id> Brief.md` (e.g., `DA-02 Brief.md`).
- **Frontmatter:** title, date, tags (`rentok` + `brief` + domain), owner, time_budget, status.
- **Always route writes through the `/obsidian` skill.** Never raw filesystem.

## RentOk-specific defaults

- Default vocabulary level: operator-friendly English (no jargon a 9th-grade ESL speaker wouldn't know)
- Default persona: footnote whether composite, invented, or from real field interviews
- Default location: Obsidian vault under `RentOk/`
- Tag every brief with `rentok` + `brief` + relevant domain (e.g., `dues`, `population`, `staff`)
- Bharat-language coverage flag: if the target user is Hindi-first / Telugu-first / Tamil-first, the doc should acknowledge in Risks

## Canonical exemplar

`/Users/eazypg/Documents/Obsidian Vault/RentOk/PRDs/Homescreen Detailed Analytics/DA-01 Brief.md`

If unsure about format / vocab / structure: read the exemplar before writing.

## Self-learning

Three sources of learning, ordered by frequency:

1. **Phase 4 audit gaps** (every brief — automatic). Whenever the mechanical audit catches a violation the write-time hygiene missed, auto-append to `~/.claude/skills/brief/learnings.md` with the pattern, the slip, and the rule to tighten. Over time this surfaces which rules need restating, new examples, or earlier enforcement.
2. **Phase 5 recurring critique findings** (every few briefs — semi-automatic). When the substantive critique sub-agent flags the same kind of finding across 3+ briefs (e.g., "operator section persona always invented," "Why-now always thin"), append a learning and suggest a template or stop-list update.
3. **Post-ship reality deltas** (per shipped feature — manual trigger). After a brief's feature actually ships, the PM runs a delta check: which claims held, which didn't, what surprised the team. These go to `~/.claude/skills/brief/post-ship-deltas.md` and update the skill's risk-read defaults over time.

Format for learnings:
```markdown
### YYYY-MM-DD: [type] Summary
**Context:** What was happening
**Finding:** What recurred
**Rule:** New rule for next time
**Applies to:** Which phase / lens / file
```

## Anti-patterns (the skill catches these on review)

- Lead with an arbitrary number / stat in the In-One-Line
- "Most users will…" without field evidence
- Borrow famous frameworks performatively (Zerodha doctrine, Shape Up Pitch without breadboard)
- "Worth a 3-user check" — a hedge that needs to become a decision or a scheduled task
- "Open Questions" section anywhere (resolve or schedule)
- Separate Decision Log + Maintenance Log (merge → one Changelog)
- 12+ sections (target 6-8)
- Same metaphor 3+ times
- ASCII art / fat-marker breadboards unless the doc is genuinely Shape Up
- UI words in the bet section (button, chart, tab, modal)
- Personas with named individuals + no footnote on composite-vs-real
- "Small backend change" / "just need to widen the filter" / "easy lift" — engineering speculation without a codebase citation
- Backend-task list in Risks / Traps with no file:line, function name, or graphify node anchor
- "Existing X does Y" claims without confirming X actually does Y in the current code (not Y in old PRD docs)
- **Treating the Build Sheet's scope as the operator's mental scope** — Build Sheet covers the direct entity; the Brief must cover the operator's full domain. Phase 2 LAYER 2 is mandatory to catch this.
- LAYER 2 dependency map returns < 3 findings on a non-greenfield Brief (suggests shallow exploration — widen the queries before drafting)
- Relying on an endpoint that's commented out / dead in production (e.g., `admin-ledger` returns `{}`). LAYER 2 catches this.
- Building a parallel view of data that an existing operator endpoint already shows (e.g., fund-balances widget). LAYER 2 catches this.

## How this skill calls other skills

- **`/graphify`** for codebase grounding (Phase 2) — auto-invoked when `graphify-out/graph.json` exists
- **`/obsidian`** for all vault writes, reads, link-checks
- **`AskUserQuestion`** tool for the gather phase
- **`Agent`** tool (general-purpose subagent) for the critique pass
- Optionally **`/pre-mortem`** if user wants a deeper pre-launch risk pass on the final brief
