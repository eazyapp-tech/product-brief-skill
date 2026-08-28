# Brief skill — learnings log

Append entries as the skill catches recurring issues, slips, or new patterns. Format per SKILL.md self-learning section.

---

### 2026-06-06: audit-gap — treated stale-doc rewrite as a fast formatting task instead of rerunning the full brief workflow

**Context:** Reworked DA-01 Dues Brief, Ground-Truth Formula Map, and Build Sheet after stale drafts. The first rewrite was too skimmed: it referenced Figma / other docs, missed duration and drill-down behavior in the formula map, used the wrong Tenant Status model, and did not match the quality of the Occupancy analytics docs. User explicitly called out rushing, weak research, weak codebase grounding, and failure to execute `brief` + `doc-handoff-review` thoroughly.

**What slipped:** I treated an existing stale doc as something to polish, instead of rerunning the full brief workflow. The right workflow was: altitude check, code/product grounding, product meaning alignment, self-contained rewrite, mechanical audit, then adversarial handoff review. The winning version only landed after multiple corrective passes.

**Why write-time missed it:** I over-weighted the existence of stale docs and Figma screens, and under-weighted the brief skill's phase discipline. I also treated a strong exemplar as optional inspiration instead of a quality bar when it was available.

**Rule:** For any future feature/product doc using `brief` and `doc-handoff-review`, do not draft fast and review later. Start with the full phase gate:
1. Altitude check first: Signal, Synthesis, Action, or Authoring.
2. If a strong prior doc exists, use it as a quality bar for depth, tone, and structure. Do not reference it inside the new doc unless explicitly asked.
3. If no prior doc exists, follow the brief workflow from zero. No exemplar is not permission to skim.
4. If code exists, ground in code before drafting. If truly greenfield, state that and skip code grounding.
5. Lock product meanings before writing: time behavior, totals, drill behavior, top-N + Others rules, permission scope, edge cases, and launch blockers.
6. Keep the Brief at product altitude and self-contained. Code anchors belong in the Build Sheet, not the Brief.
7. Run the Phase 4 mechanical audit and doc-handoff-review fact/readability passes before handoff.
8. If review finds a real defect, fix it and rerun the relevant check.

**Applies to:** SKILL.md Phase 1 through Phase 5; stale-doc rewrites; greenfield product briefs; ground-truth maps; build sheets; any RentOk product-doc handoff where the user invokes `brief` or `doc-handoff-review`.

**Pattern note:** Exemplars are optional; process is not. If no exemplar exists, rigor goes up, not down.

---

### 2026-05-16: discovery — Phase 2 codebase grounding was being done at single-entity depth, missing operator's full mental domain

**Context:** Wrote DA-04 Expenses Brief at V0.2.1. Grounded thoroughly against `src/entities/expenses.ts` + `src/v1/list_screens/expenses/` (helpers, service, routes, filter codes). Verified file:line citations for traps, auth gaps, formula details. Brief audit scored 9/10 on codebase grounding.

**What slipped:** The Brief missed the entire Team Passbook ecosystem — `TeamPassbookExpenseService`, `TeamPassbookReimbursementService`, `HandoverService`, fund types (AF / PF / NPNAF / **EF** — 4th fund type I didn't even know existed), reimbursement state machine (`getOpenPfForExpenseUuid`, `autoAllocatePf`, `reimburseMoney`), handover-cash flow with OTP, admin-ledger endpoint (**dead — service body commented out, returns `{}`**), give-money to staff. User flagged the blind spot — *"the data points or those questions that a user would ask are missing throughout"* — and pointed out the same gap likely applies to DA-02 (settlement state + bank-wise bifurcation) and DA-03 (refund lifecycle completeness).

Subsequent dependency-map sub-agents confirmed:
- **DA-02:** full settlement lifecycle (Payment → `SettlementFlowService.scheduleSettlement` → `settlement_scheduler` → `WalletPayoutService` → `wallet_payouts` → Razorpay webhook); 3-level bank routing (DueTypeBankMapping → Room → Property); **`controllers/settlements.ts` entirely commented out**; **no bank-wise summary endpoint exists**.
- **DA-03:** refunds have **NO state machine** (no `status`, no `gateway_refund_id`); refund detail screen has `reimbursement_details: []` HARDCODED EMPTY; DA-06 Liability cross-screen is live but never surfaced in the Brief.
- **DA-04:** EF = "Exchange Funds" (bookkeeping contra-account, never expose to operators); reimbursement-owed-to-staff has zero aggregate endpoint despite weekly operator pain.

**Why write-time / phase-2 missed it:** Phase 2 (Codebase grounding) asked for graphify queries on the direct entity. I queried "Expenses" and got the Expenses module — but did not WIDEN to adjacent operation-factory siblings (`TeamPassbookExpenseService` sits next to `TeamPassbookCollectionService`, `RefundService`, `ReimbursementService`, `HandoverService` in the same factory directory). The Build Sheet anchored my exploration to engineering-scope; the Brief should anchor to operator-scope, which is wider.

**Rule:** Phase 2 expanded to **two-layer exploration**:
- **LAYER 1: Direct entity grounding** (the obvious one — what I did)
- **LAYER 2: Domain dependency mapping** (the one that gets skipped — what I missed)

LAYER 2 mandatory steps:
1. Enumerate sibling services in the same operation-factory directory
2. Walk cross-entity flow tables (transactions, fund balances, lifecycle states, audit snapshots)
3. Map lifecycle / settlement tables (what happens AFTER the entity is created)
4. List operator-facing endpoints in the same module (some adjacent, some dead, some hardcoded-empty)
5. Use graphify community-neighbor expansion to find adjacent modules
6. Flag thinness: if LAYER 2 returns < 3 findings on a non-greenfield Brief, widen.

**Applies to:** SKILL.md Phase 2 (now expanded with LAYER 1 + LAYER 2 sub-steps). Anti-patterns list (added: "Treated Build Sheet scope as operator mental scope," "Relied on a commented-out endpoint," "Built parallel view of data already shown elsewhere").

**Artifacts produced from this discovery:**
- `[[DA-02 V2 Dependency Map]]` — settlement lifecycle + bank-routing
- `[[DA-03 V2 Dependency Map]]` — refund lifecycle + passbook + liability bridge
- `[[DA-04 V2 Dependency Map]]` — passbook ecosystem + reimbursement + fund-flow
- V2 scope pointer sections added to DA-02 / DA-03 / DA-04 V1 Briefs (acknowledging the gap honestly, listing top 3 deferred questions each, naming V1→V2 architecture decisions)

**Going forward:** DA-05 / DA-06 / DA-07 Briefs run Phase 2 LAYER 2 mandatorily before Phase 3 Gather. No exceptions. Phase 3 gather questions should explicitly reference LAYER 2 findings — if they don't, the LAYER 2 exploration was probably shallow.

---

### 2026-05-18: anti-pattern — added new V1 scope without fitting it into the operator-ranked cut order discipline

**Context:** DA-05 Brief at v0.3. User added a real V1 scope item mid-flight: new "Has discount" / "No discount" filter chips on DA-02 Collections list + a cross-screen drill from DA-05 Q6 "Used" to that filtered list.

**What I did:** Wrote the addition in 3 places (Operator drill chain, TEAM trap, Key Decisions Locked). Verified tech facts with a sub-agent (caught wrong SQL JOIN, stale line range, snake_case violation — all fixed in v0.3.1). Declared satisfaction.

**What I missed (caught by user "Why am I still skeptical?"):**
1. **Drill direction wasn't stress-tested for operator mental model.** Q6 "Used: ₹15K" is a discount-amount number. Natural operator question = "show me those used discounts" = discount-centric drill. I wrote a payment-centric drill (Collections list) because that's what the user proposed — without testing the mental fit. User clarified: Collections-list IS a V1 bridge because the mobile discount worklist doesn't support `status=1` filtering yet; correct framing is "temporary scaffolding, migration path named."
2. **New scope wasn't placed in the existing operator-pain-ranked cut order.** Original cut order: Q9 → Q8 → Q7 → Q5. I added "chips + drill" as an orphan with a Day-3 cut-line about *filter-code allocation* (TEAM concern), never asking "if time tightens, do the chips cut or does the drill cut?"
3. **Operator-side cost of cutting wasn't named, only team-side cost.** I quantified "~2-3 dev-days to build." I didn't quantify "if you cut chips, operator's reconciliation workflow breaks at month-end on high-volume properties."
4. **MUST-SHIP vs CUTTABLE was inverted.** Chips have standalone DA-02 governance value (cutting = open each collection one by one). Drill is convenience (cutting = 2 extra taps). I wrote the Day-3 cut-line as "defer chips to V1.1" — treating chips as the cuttable piece.
5. **Rhythm fit wasn't named.** When does Rajesh tap this drill? Weekly review + monthly audit, ~4-5×/month. Not naming the rhythm hides whether it's a daily-anchor (must-ship) vs niche-stretch (cut-first).

**Rule:** Every new V1 scope item added mid-flight must answer 4 operator-lens questions BEFORE the tech-fact pass:
1. **Mental-model fit:** is the drill direction / interaction operator-correct, or is it convenient for the team but off-model for the user?
2. **Cut-order placement:** where in the existing operator-pain-ranked cut order does this sit? (Must-ship / stretch / cut-first?) If it doesn't fit anywhere, the scope addition is hand-waved.
3. **Operator-side cost of cutting:** what does the operator lose? Quantify in workflow terms (extra taps / minutes / reconciliation breaks), not just engineering cost.
4. **Rhythm fit:** when in the operator's day/week/month does this matter? (Daily-anchor / weekly / monthly / event-triggered?) Hidden rhythm = hidden priority.

A scope addition that passes tech-fact verification but skips these 4 = **operator-lens orphan**. Catch with: "Why am I still skeptical?" gut-check after declaring satisfaction.

**Applies to:** SKILL.md Phase 4 (write-time discipline) + Phase 5 (critique pass). Add the 4 operator-lens questions to any mid-flight scope-addition workflow. Anti-patterns list (add: "Operator-lens orphan — added scope without placing it in cut order, naming operator-side cost of cutting, or naming rhythm fit").

**Artifacts produced from this discovery:**
- DA-05 Brief v0.3 → v0.3.1 (tech facts) → v0.4 (operator lens)
- DA-02 Brief v0.7.2 → v0.7.3 (tech facts) → v0.7.4 (operator lens — chips elevated to MUST-SHIP in DA-02's own line)
- This learning entry

**Commitment for DA-06 / DA-07:** any mid-flight scope addition pauses for the 4 operator-lens questions BEFORE I touch the file. If the user adds scope and I can't answer all 4 from operator perspective, I ask before writing.

---

### 2026-05-18: anti-pattern — drill-audit format that doesn't enforce "check shipped capability before proposing work" propagates false-premise scope across every drill

**Context:** Post-Brief drill-audit work for the financial DA suite (DA-01 through DA-07). After all 7 Briefs signed off, user requested operator-first drill audits per section per DA — verify every tap-able element drills to the operator-correct destination, with codebase evidence + V1/V1.x/V2/V3 scope tags. First session = DA-07 Section A Inflows (universal pattern audit + subtotal audit).

**What I did (v0.1 of session 1a):** Built a per-element prose audit. For Element 1 (inflow row tap), I:
1. Stated operator's next action ✓
2. Stated current state — but with "verified via grep" hand-wave, no inline file:line citation per claim
3. Proposed ideal state
4. Tagged "V1.x scope, ~3-5 dev-days backend filter work" — claiming Collections list had no due_type filter

**What I missed (caught by sub-agent verification pass):** Collections list HAS shipped due_type filtering today via TWO layers:
- 6 hardcoded filter codes (`CURRENT_MONTH_RENT/ELECTRICITY/MESS_FOOD/DEPOSIT/LATE_FINE/ADVANCE = 1204-1210`) applied at `src/v1/list_screens/collections/helpers.ts:229-263` with smart clustering (e.g., `ILIKE '%electricity%'` automatically catches 'Electricity Bill' + 'Electricity Recharge' variants)
- Generic `due_types: z.array(z.string())` request body param at `schemas.ts:28` via `applyDueTypes()` helper at `helpers.ts:357-365` (parameterized, SQL-injection-safe)

The "V1.x ~3-5 dev-days backend work" was a **false alarm**. Actual delta is **frontend-only routing logic ~0.5-1 dev-day, already V1 scope**.

**Root cause:** my drill-audit format went from "Operator next action" → straight to "Current state" → "Ideal state" → "Delta + scope" without an explicit step that asked *"is this capability already shipped?"*. Without that step, I jumped from "operator wants X" to "we need to build X" without checking the codebase for X first.

**Blast radius if uncaught:** the same false-premise pattern would have propagated across ~50 sessions (DA-07 has 10+ sections; DA-06 has 8+; DA-05/04/03/02/01 collectively 30+). Every drill audit would have proposed redundant V1.x backend scope. Engineering would have built filters that already exist. Weeks of wasted work.

**Rule (drill-audit format v2 — applies to ALL drill audits across ALL projects):**

Per-element prose audit MUST follow this exact sequence:
1. **Operator next action** (user confirms)
2. **EXISTING CAPABILITY CHECK (MANDATORY)** — single 30-second grep/graphify against entity / service / helper / route / schema / filterCodes / middleware layers for SHIPPED capability that already serves the operator's next action. Cite findings inline with file:line + clause text. NO "Delta + scope" can be proposed until this step completes.
3. **Current state with inline file:line per claim** — NOT "verified via grep" hand-wave. Each claim about what exists must have a file:line citation + the relevant clause text (so verifier can spot-check atomically + clause text survives line-number drift from future code churn)
4. **Ideal state (operator-first)** — what would operator-correctly serve the next action
5. **Delta + scope** — ONLY if Existing Capability Check confirms a real gap. Backend delta vs frontend delta distinguished separately. Scope tag: V1 (in current scope) / V1.x (next patch) / V2 (next major) / V3 (long-term)
6. **Edge cases (verified against shipped behavior)** — verify edge case handling against helper code, not against prescriptive intent. Don't propose new behavior the helper already provides differently.
7. **Cross-Brief impact** — what other Briefs / Build Sheets need updates

**Plus mandatory verification cadence:** sub-agent verification AFTER EVERY SECTION (not every DA), even when format feels routine. Sub-agent uses graphify-first + grep-fallback. The Session 1a v0.1 false alarm was caught only because verification ran immediately, not after multiple sections accumulated errors.

**Applies to:** any drill-map audit work; `/brief` skill's Phase 2 codebase grounding (the Existing Capability Check sub-step generalizes the LAYER 1+2 discipline); Phase 5 critique (sub-agent must verify "is this shipped?" before "is this missing?"); any audit-class artifact where the writer is proposing scope.

**Artifacts produced from this discovery:**
- `DA-07 Drill Map.md` v0.1 → v0.2 → v0.2.1 (v0.1 kept in changelog as learning record)
- Protocol v2 documented in this learning + applied to all future drill audits
- Sub-agent verification cadence locked at "after every section" for the duration of this audit work

**Pattern note:** this is the same class of issue as the 2026-05-18 operator-lens-orphan learning — a format gap that allowed a class of error to propagate. Both are caught by the same fix: enforce the discipline at protocol level, not at goodwill level. "Existing Capability Check" + "inline file:line" + "sub-agent every section" are non-negotiable steps for the same reason "4 operator-lens questions on new scope" + "cut-order sweep across all references" were locked in earlier.

---

### 2026-05-18: anti-pattern — drilling from an N-entity aggregate to a single-entity detail screen makes no operator sense ("aggregate-to-single-entity")

**Context:** DA-07 Drill Map audit, table format v0.3. Row #12 "Tenant refunds (non-deposit)" had drill destination "DA-03 Refunds list — does NOT exist; V1 fallback: Tenant Passbook (refund tab)". Similar pattern at row #18 ("Deposits returned" → "Tenant Passbook per-tenant deposit refunds").

**What I did wrong:** treated "Tenant Passbook" as a generic V1 fallback for any tenant-related drill that lacks a proper list screen. Reasoned: "tenant passbook shows refunds, so when DA-03 list doesn't exist, fallback to passbook."

**What the user caught:** the cash flow line "₹X tenant refunds" is an aggregate across N tenants. Drilling to ONE tenant's passbook requires picking which one — but the aggregate doesn't name a specific tenant. Operator has no way to know which tenant to navigate to; the system has no way to pick. The drill is logically broken.

**Code verification:** `CollectionFilterCode.REFUND = 1209` (filter code 1209) exists in the enum + has a label "Refunds" at `helpers.ts:1091`, but its actual handler at `helpers.ts:256-258` is a STUB with comment `// TODO: no additional filter (matches original behaviour)` — does nothing today. The "Has refund" chip Brief v0.2.2 commits to building (Collections list with LEFT JOIN refunds + WHERE refunds.id IS NOT NULL) is the operator-correct V1 fallback — but it requires the chip to actually ship.

**Rule (anti-pattern check #1 — apply on EVERY drill audit row, across every DA):**

An aggregate "₹X across N entities" MUST drill to a LIST of those N entities, NEVER to a SINGLE entity, UNLESS the aggregate label itself names ONE specific entity.

**Valid drills:**
- Aggregate "Rent collected ₹15K" → Collections LIST filtered to Rent ✓ (drills to N receipts)
- Aggregate that names entity "Vendor: Acme received ₹5K (45% of outflow)" → Expenses LIST filtered to `paid_to=Acme` ✓ (label names the entity)
- Operator-picked specific row "Suresh — ₹3K" (in a Top-5 list) → Tenant Detail for Suresh ✓ (operator chose WHICH entity)

**Invalid drills:**
- Aggregate "Tenant refunds ₹8K" → single Tenant Passbook ❌ (which of N tenants?)
- Aggregate "Deposits returned ₹15K" → single tenant's deposit page ❌ (same)
- Subtotal "Total Inflows ₹12L" → any single record ❌ (ambiguous which subset)

**Anti-pattern checklist (mandatory per drill audit row):**

1. Aggregate → single entity? (this anti-pattern)
2. Label names the entity? (if yes, single-entity drill is valid)
3. Operator-picked specific row? (if yes, single-entity drill is valid)
4. Subtotal tap = ambiguous drill? (subtotals should be non-tappable)
5. Drill destination exists in code? (Existing Capability Check from v0.2 protocol)
6. Cross-screen sibling V1 dependency? (check sequencing + degraded-launch ladder)

**Blast radius if uncaught:** every drill audit across DA-01 through DA-07 (~50 sessions) has dozens of aggregate rows. Without this check, lazy "drill to legacy single-entity screen" fallbacks would propagate. Designers would build illogical drill paths; engineers would wire them up; operators would tap and land on the wrong screen.

**Applies to:** any drill-map audit; `/brief` skill Phase 5 critique (sub-agent must verify "is this aggregate drilling to a list or to a single entity?"); design-handoff conversations (designers proposing drill destinations should be challenged with this anti-pattern check).

**Artifacts produced:**
- `DA-07 Drill Map.md` v0.4 — rows #12 and #18 corrected; row #26 flagged as mild concern; "Anti-pattern checklist" section added at top of file (6 checks per row mandatory for all future audits)
- This learning

**Pattern note:** this is the THIRD class of error caught at protocol level during DA suite work, after:
1. 2026-05-16: LAYER 2 codebase grounding (Brief drafts assumed direct entity = full operator domain)
2. 2026-05-18 (earlier): operator-lens-orphan (added scope without operator-pain-rank fit)
3. 2026-05-18 (this entry): aggregate-to-single-entity drill (lazy fallback to single-entity screen)

All three errors share a root cause: format/protocol gaps that allow a class of error to propagate. All three fixes share the same shape: codify the check at protocol level (Phase 2 LAYER 2 / 4 operator-lens questions / 6-check anti-pattern checklist) so the discipline doesn't depend on goodwill or attention.

---

### 2026-05-18: anti-pattern — "ideal destination doesn't exist → degraded/greyed fallback" without first exhausting smart combinations of existing screens + filters

**Context:** DA-07 Drill Map v0.4. Row #20 "View deposit balance →" had drill destination "DA-06 Liabilities (NEW BUILD, doesn't exist) → V1 fallback: greyed CTA + tooltip." Five rows total marked 🔧 (real gap) with similar lazy-fallback framing.

**What I did wrong:** for each drill where the ideal destination is a NEW BUILD that won't ship in V1, defaulted to "fallback: greyed/disabled/single-screen-stub." Treated "the ideal screen doesn't exist" as "no drill can answer this in V1."

**What the user caught:** the operator's actual question often CAN be answered by a smart combination of EXISTING list screens + EXISTING filters. Example for "View deposit balance →": instead of greyed CTA, render as three-CTA fan-out (Active / Bookings / Old Tenants) each linking to DA-02 Collections list filtered by deposit due_types + `tenant_types: [0/1/2]`. Operator's actual question (which tenants hold my deposits?) IS answerable today.

**Code verification (the discovery):** `applyTenantTypes` filter at `src/v1/list_screens/collections/helpers.ts:380-388` + `tenant_types: z.array(z.number())` schema at `schemas.ts:30` already ships. Canonical homepage Deposits-Held query at `homepage/service.ts:2414-2456` already computes active/booking/old splits. **Three-CTA fan-out is zero backend work.** Five other rows had similar shipped-capability discoveries:
- Row #12: filter code 1209 (REFUND) exists but is a stub at `helpers.ts:256-258` (`TODO: no additional filter`); wiring it to actually INNER JOIN refunds = 10-line hot-add, fixes #12 cleanly.
- Row #15: `Expenses` entity has NO `is_capex` column — Build Sheet's `is_capex=true` filter was invented. Real fallback: `categories: ['Other']` + sort by amount DESC. IS the correct destination, not a compromise.
- Row #16: Mode 213 = RentOk-funded discount payments. `payment_mode: [213]` filter ships. Owner-funded portion needs small hot-add (1 filter + 1 SUM).
- Row #18: deposit refunds are recorded as Expense rows (not Refund records). `CURRENT_MONTH_DEPOSIT = 1309` on Expenses already filters `expense_type ILIKE 'Deposit%'`. V1 ready.
- Row #21: no FK from Expense to Refund — unmatched-only filter can't be computed. V1 fallback: show full list, drop unmatched framing.

**Rule (smart-fallback discovery — apply on EVERY 🔧 row):**

When a drill's ideal destination is a NEW BUILD that won't ship in V1, BEFORE tagging "greyed/disabled/V2," exhaust this checklist:

1. **What's the operator's actual question?** (in their voice — not the screen label's voice)
2. **Which existing list screens could answer it?** (Collections / Expenses / Dues / Tenant / Bookings / Old Tenants / Rooms / Tenants-with-tags)
3. **Which existing filters on those screens get closest?** (filter codes + generic params like `tenant_types[]`, `due_types[]`, `payment_mode[]`, etc.)
4. **Is the answer a SINGLE screen with one filter, MULTI-CTA fan-out (one per category), or a hot-add (≤1 dev-day) to an existing filter code?**
5. **What's the honest operator-side fidelity gap vs the ideal?** (e.g., "list shows gross-paid, not held-balance — overstates by refund+adjustment amount")

If after this checklist the answer is genuinely "no existing screen can serve any version of the operator's question," THEN greyed/disabled is the right call. Until then, it's lazy.

**Concrete patterns to look for in shipped code:**
- Filter that EXISTS but isn't wired (stub like `TODO: no additional filter`) → hot-add wires it
- Filter that EXISTS but isn't surfaced as a UI chip → frontend just calls the param
- Existing aggregator on homepage/service computing the splits the new screen would have — repurpose for fan-out CTAs
- Filter on adjacent screen (Expenses vs Collections vs Dues) might actually be the operationally-correct destination

**Sub-agent prompt template (use for smart-fallback investigation):**
"For each drill row marked 🔧 (real gap), find the SMARTEST V1 fallback using existing screens + existing filters. Output per row: operator's actual question / shipped capability found (file:line) / smartest V1 fallback (specific screen + filter combo) / operator-side honest framing / scope (V1 ready / V1 hot-add / V2)."

**Applies to:** any drill-map audit; any "V1 fallback" decision; `/brief` skill Phase 4 draft + Phase 5 critique (critique sub-agent should challenge any "greyed/disabled" with "what's the smart combination of existing screens?").

**Artifacts produced:**
- `DA-07 Drill Map.md` v0.5 — 6 rows rewritten; 🔧 count went 5 → 0
- This learning

**Pattern note:** this is the FOURTH class of error caught at protocol level during DA suite work:
1. 2026-05-16: LAYER 2 codebase grounding gap
2. 2026-05-18 earlier: operator-lens-orphan
3. 2026-05-18 mid: aggregate-to-single-entity drill
4. **2026-05-18 (this entry): lazy "greyed/V2" fallback without exhausting existing-capability combinations**

All four share root cause: format/protocol gaps allowing a class of lazy thinking to propagate. All four fixes: codify the check at protocol level. The smart-fallback checklist (5 questions) is now mandatory for every 🔧 row.

---

### 2026-05-18: anti-pattern #5 — choosing drill destination based on code-layer data path instead of operator mental model

**Context:** DA-07 Drill Map v0.6. Row #18 "Deposits returned" was drilling to Expenses list (filtered to expense_type='Deposit Refund'). Sub-agent investigation in v0.5 had surfaced an operational truth: "some operators record deposit refunds as Expense rows (workflow quirk) — filter code 1309 on Expenses list filters those rows directly." I followed the code-layer data path.

**What the user caught:** "Deposits are also collections. The payment came in. I marked the refund against that payment. So it's the same as any other refund. Why are there such mistakes throughout?"

**The mistake:** I optimized for the data layer (where deposit refunds happen to be recorded in the database) instead of the operator mental model (operator thinks "a refund is a refund — show me the original payment that got refunded, regardless of what the original payment was for").

**Operator-correct fix:** Row #18 should mirror row #12 (Tenant refunds non-deposit). Both drill to Collections list with the same refund filter mechanism + different type filters. Symmetric. Operator's mental model: "refund = refund."

**Side note (don't conflate):** Row #21 (unmatched-deposit-refund-expense warning) STAYS on Expenses list because THAT warning is specifically about the workflow-quirk Expense entries themselves — different operator question, intentionally different drill. The Expenses-list drill IS correct for that specific warning.

**Rule (anti-pattern check #7 — symmetric-concept check, mandatory per drill row):**

When the operator's mental model treats two (or more) things as the same concept, the drills for those things MUST be symmetric: same screen, same filter mechanism, only the type/category/scope filter differs. Asymmetric drills for the same operator concept = operator confusion + UI inconsistency.

**Common operator-concept groupings to check across the drill map:**
- **Refunds** ("money I gave back") — all refund drills should be symmetric regardless of what was originally refunded (rent / utility / deposit / etc.)
- **Collections** ("money I received") — all collection-type drills should be symmetric
- **Discounts** ("money I gave away as discount") — should not be split arbitrarily by funding source unless operator explicitly asks
- **Expenses** ("my operating spend") — outflow rows should drill consistently to Expenses list
- **Aggregates / subtotals** — should be consistently non-tappable

**Process fix (apply per drill audit row):**

1. Before locking a drill destination, identify the operator-concept this row belongs to.
2. Look at other rows in the same drill map for the SAME operator-concept.
3. Check: are the drills for the same concept SYMMETRIC (same screen, same filter mechanism, only type differs)?
4. If asymmetric, one of them is wrong — usually the one optimizing for code-layer reality.
5. The right deciding question: *"would the operator think of these as the same concept and expect them to behave the same way?"* If yes → make symmetric.

**Sub-agent verification step (new — mandatory after each drill audit section):**

Sub-agent verification pass now includes:
> "State each drill's destination in pure operator-voice (no code paths). Then group rows by operator-concept (refunds, collections, deposits, discounts, expenses, etc.). For each concept group, check: are the drills symmetric? If not, identify the asymmetry and flag whether one is wrong."

**Blast radius if uncaught:** every drill audit across DA-01 through DA-07 has multiple symmetric-concept groupings. Without this check, sub-agent code-layer optimizations (e.g., "deposit refunds are technically Expense rows, so drill to Expenses") will silently break operator consistency. Operator will tap "Rent refunds" and land on Collections list, tap "Deposit refunds" and land on Expenses list — same conceptual action, different screen. UI feels broken.

**Applies to:** any drill-map audit; `/brief` skill Phase 4 draft + Phase 5 critique (critique sub-agent must run symmetric-concept check); design-handoff conversations (designer should be able to challenge "why is this drill different from that one?" with a clear answer rooted in operator mental model, not code path).

**Artifacts produced:**
- `DA-07 Drill Map.md` v0.7 — row #18 corrected; row #21 framing tightened to make the intentional Expenses-list drill clear; 7th anti-pattern check added to engineering reference checklist; status summary updated
- This learning

**Pattern note:** this is the FIFTH class of error caught at protocol level during DA suite work:
1. 2026-05-16: LAYER 2 codebase grounding gap (single-entity-grounding missed full operator domain)
2. 2026-05-18 (earlier): operator-lens-orphan (new scope added without operator-pain-rank fit)
3. 2026-05-18 (mid): aggregate-to-single-entity drill (lazy fallback to single-entity screen)
4. 2026-05-18 (later): lazy "greyed/V2" fallback without exhausting existing-capability combinations
5. **2026-05-18 (this entry): code-data-model drill destination instead of operator-mental-model drill destination**

All five share a root cause: format/protocol gaps allowing a class of operator-confusing decisions to propagate. All five fixes: codify the discipline at protocol level + mandatory sub-agent verification step. The symmetric-concept check is now the 7th of 7 mandatory anti-pattern checks for every drill audit row.

---

### 2026-05-18: anti-pattern #6 — assuming a filter-code does what its name suggests, without verifying the actual predicate

**Context:** DA-07 Drill Map v0.7. Multiple rows used filter codes like `CURRENT_MONTH_DEPOSIT (1207)`, `CURRENT_MONTH_RENT (1204)`, `CURRENT_MONTH_OTHER (1308)`, `CURRENT_MONTH_DEPOSIT (1309)` etc. Assumed these filter codes were period-aware (would respect operator's selected period in conjunction with `start_date`/`end_date` params).

**What sub-agent caught (and user's gut-check forced):** verified at `src/v1/list_screens/collections/helpers.ts:226-247` and similar in Expenses/Dues — every `CURRENT_MONTH_*` filter code hardcodes the predicate `EXTRACT(MONTH FROM ...) = currentMonth` (Collections) or `paid_date >= monthStart AND paid_date < nextMonth` (Expenses) where `currentMonth`/`monthStart` are computed server-side from current calendar date. **They IGNORE the operator's `start_date`/`end_date` request params silently.**

**Real bug:** if frontend passes `filter_code = 1207` + `start_date = '2026-04-01' end_date = '2026-04-30'` (Last Month for an operator in May), backend returns deposits from MAY (current month), not April (operator's chosen period). Operator sees data they didn't ask for, with no warning.

**Why this slipped past previous protocols:** the name `CURRENT_MONTH_DEPOSIT` SOUNDS period-agnostic (just a type filter). Without reading the actual case-statement predicate in `applyFilterCodes`, the assumption "filter code = type filter + period-respect" looked safe. Past audits verified filter code EXISTS in the enum + has a label; didn't read the predicate logic.

**Rule (anti-pattern check #6 — codify in protocol):**

For any drill row that uses a `filter_code` value, the audit MUST verify the actual predicate logic in the helper file (not just confirm the enum value exists). Specifically:

1. Open the `applyFilterCodes` (or equivalent) helper file
2. Find the `case <FILTER_CODE>:` branch
3. Read EVERY `query.andWhere(...)` clause in that branch
4. Identify: does the predicate lock period server-side? Lock category to specific string? Apply other implicit filters?
5. If filter code locks period → use TYPE-filter + period-range helpers separately for any drill that needs operator-selected period

**Architectural truth (suite-wide pattern verified):**

| Filter approach | Period behavior |
|----------------|----------------|
| `CURRENT_MONTH_*` filter codes (any list endpoint) | LOCKED to current month server-side |
| `due_types[]` / `categories[]` + `applyDateRange` / `applyPaidDateRange` | Period-aware |
| `applyTenantTypes` / `applyPaidTo` / `applyNotCategories` | Independent — combine with period helpers |

**For DA-07 drills (operator picks period): NEVER use `CURRENT_MONTH_*` filter codes. Always use type-filter + period-range helpers.**

**Sub-agent verification step (new — adds to row-by-row check):**

For every drill row with a filter_code citation, sub-agent must verify the predicate logic at file:line — not just confirm the enum value. Output: "Filter code X at <file:line> applies these clauses: [...]. Period behavior: [period-respects / period-locked]."

**Applies to:** all drill audits; `/brief` skill Phase 2 codebase grounding (filter-code enum verification must include predicate verification); Phase 5 critique (sub-agent must verify predicate logic for any filter_code claim).

**Artifacts produced:**
- `DA-07 Drill Map.md` v0.8 — rows #4, #6-#11, #15, #17 reframed to use type-filter + period helpers instead of filter codes
- Architectural truth section added at top of file + engineering reference
- This learning

**Pattern note:** this is the SIXTH class of error caught at protocol level during DA suite work:
1. 2026-05-16: LAYER 2 codebase grounding gap
2. 2026-05-18 (earlier): operator-lens-orphan
3. 2026-05-18 (mid): aggregate-to-single-entity drill
4. 2026-05-18 (later): lazy "greyed/V2" fallback
5. 2026-05-18 (this morning): code-data-model vs operator-mental-model drill destination
6. **2026-05-18 (this entry): assuming filter-code semantics from name without verifying predicate**

All six share root cause: format/protocol gaps allowing a class of code-or-operator confusion to propagate. All six fixes: codify the check at protocol level + mandatory sub-agent verification step. The 7 anti-pattern checks are now augmented with the predicate-verification discipline for any filter_code claim.

---

### 2026-05-18: anti-pattern #7 — applying a newly-discovered fix asymmetrically (catching it in one case, missing parallel cases in the same audit pass)

**Context:** DA-07 Drill Map v0.8.1. v0.8.1 caught and fixed a real bug — `applyNotCategories` helper exists in code BUT isn't wired to any request body param (only callable via 'JATIN' magic string hack). Reclassified rows #11 + #15 to V1 hot-add. Added the lesson to the v0.8.1 changelog: "helper presence ≠ body-param plumbing — always verify both layers."

**What I missed (caught by 4th-round sub-agent verification):** in the SAME drill map, row #12 (and #18 by reference) uses `applyNotDueTypes` on Collections list — the EXACT same plumbing gap. Only callable via 'JATIN' magic string at `collections/service.ts:60-74`. No `due_types_not_in` body param in schemas.ts. I codified the lesson in the changelog but didn't apply it to the symmetric case in the same audit pass.

**Root cause:** when I catch a bug for one helper, I'm so focused on fixing-it-properly-and-documenting-it that I don't STOP to ask "where else does this pattern repeat in the same document?" The lesson goes in the changelog; the parallel cases stay broken.

**Rule (anti-pattern check #8 — codify in protocol):**

When a fix is applied based on a newly-discovered codebase truth (e.g., "helper X is unwired"), the audit MUST search the same document for ALL parallel cases of the same pattern before declaring the fix complete. Specifically:

1. Once a fix is applied to one row, search the document for: same helper name / same pattern of "uses helper Y" / same data layer (Collections vs Expenses vs Dues)
2. For each parallel case found, verify the same gap doesn't apply
3. If the same gap DOES apply, fix the parallel case in the SAME version bump, not the next one
4. Document the SCAN itself in the changelog ("Searched for parallel cases of `applyNotX` plumbing gap across: Collections / Expenses / Dues — found N cases at rows #X, #Y; all fixed")

**Sub-agent verification step (extended):**

For every newly-discovered bug fix, sub-agent verification must:
1. Verify the fix landed correctly for the originally-found case
2. **Search the entire document for parallel cases of the same pattern** — not just check the originally-found rows
3. Flag any parallel cases that still have the unfixed pattern

**Applies to:** any audit work; `/brief` skill Phase 5 critique (sub-agent must scan for parallel patterns of any fix); any document with structurally-symmetric content (drill maps, build sheets, briefs across a suite).

**Artifacts produced:**
- `DA-07 Drill Map.md` v0.8.2 — row #12 + #18 hot-add scope corrected to include `due_types_not_in` body-param wiring
- This learning

**Pattern note:** this is the SEVENTH class of error caught at protocol level during DA suite work:
1. 2026-05-16: LAYER 2 codebase grounding gap
2. 2026-05-18 (earlier): operator-lens-orphan
3. 2026-05-18 (mid): aggregate-to-single-entity drill
4. 2026-05-18 (later): lazy "greyed/V2" fallback
5. 2026-05-18 (this morning): code-data-model vs operator-mental-model drill destination
6. 2026-05-18 (mid-afternoon): assuming filter-code semantics without verifying predicate
7. **2026-05-18 (this entry): applying a newly-discovered fix asymmetrically (catching in one case, missing parallel cases)**

All seven share root cause: format/protocol gaps allowing a class of audit blind spot to propagate. All seven fixes: codify the check at protocol level + mandatory sub-agent verification step. The "scan-for-parallel-cases-after-any-fix" discipline is now the 8th mandatory check.

**Meta-pattern note:** **The user's "are you 100% sure?" pattern is the right discipline.** Even after 4 rounds of verification, each round catches something — sometimes a fundamentally new class of error, sometimes a self-blind-spot in the previous round's fix. Honest answer: declarations of "100% sure" should be EARNED through ZERO-finding rounds, not assumed.

---

### 2026-05-18: anti-pattern #8 — declaring codebase-verified as PM-quality-approved (two different bars; both required)

**Context:** DA-07 Drill Map reached v0.8.3 after 5 rounds of codebase verification, each round catching real codebase bugs. The 5th round returned ZERO substantive findings. I declared APPROVED. User pushed back: "Be very critical, systematic, coverage" — dispatched a PM-grade operator-first audit. **PM audit caught 4 CRITICAL cross-Brief + operator-fidelity issues that 0/5 codebase rounds caught.**

**What codebase verification catches:** wiring (does helper exist? is it body-param-wired? does filter code do what its name says? do auth gates exist?). All correctness within the file's own codebase context.

**What codebase verification can't catch:**
1. **Cross-Brief consistency** — DA-03 says "no Refunds worklist in V1." I framed DA-07 as "drills to DA-03 worklist." Phantom destination. Required reading sibling Brief, not code.
2. **Cross-DA operator-mental-model symmetry** — DA-06 sends operator to Current/Bookings/Old Tenant lists for deposit drill. DA-07 was sending to Collections list. Same concept, different drills across screens. Required cross-Brief reading + operator empathy.
3. **Operator-fidelity quantification** — caveat "may overstate" might actually mean 30% overstatement → operator plans refunds against wrong number. Required real-property data calibration, not code.
4. **UX choice consistency** — DA-05 ships "Has discount" chip on DA-02 as MUST-SHIP V1. DA-07 was inventing a different filter for the same concept. Required reading sibling Brief.

**Rule (anti-pattern check #8 — codify in protocol):**

For any drill audit or PRD review, codebase verification (helper-wiring, filter-code-predicate-check, auth-gate-check) is ONE dimension. PM-quality verification is a separate dimension. BOTH required before declaring approved.

**Specific PM-quality checks (mandatory after codebase verification completes):**

1. **Cross-Brief consistency scan:** for any drill destination referenced as "ships with DA-X V1," read DA-X's Brief to confirm the destination actually ships there. Phantom destinations are real.

2. **Cross-DA operator-mental-model symmetry:** for any concept that appears across multiple DAs (deposits, refunds, discounts, etc.), the drill destinations and chip/filter patterns must be CONSISTENT. Operator switching between screens for the same concept should see the same drill behavior.

3. **Operator-fidelity quantification:** for any drill with a caveat like "may overstate" / "approximate" / "directionally correct," quantify the gap. If the gap is action-altering (operator plans against wrong number), it needs either a pre-launch calibration test OR a hot-add to close the gap.

4. **UX choice reuse vs invention:** if a sibling DA V1 ships a chip/filter for a related concept, REUSE it before inventing a new filter on the same destination. Invention = cross-DA inconsistency.

5. **Edge-case sweep:** new property (<30 days), sparse data, multi-property scope, non-standard categories, period change mid-drill, empty drill, permission-denied — verify each row handles these gracefully.

6. **Pre-mortem:** "It's 3 months post-launch. Operators are complaining. What's the #1 complaint?" Trace to a row + ideally what could have prevented it.

**Sub-agent dispatch template (PM-quality audit):**

"PM-grade operator-first audit. Apply 10-part methodology: (1) Operator instinct check per row, (2) Edge case coverage per row (a-g), (3) Operator-fidelity quantification for rows with caveats, (4) Cross-DA pattern consistency by reading sibling Briefs, (5) Symmetric-concept check, (6) Operator-misleading risks, (7) Performance/scale, (8) Accessibility/language, (9) PM decisions deferred, (10) Pre-mortem top 3. Use sibling DA Briefs not just code. Be ruthless. Find what codebase verification missed."

**Applies to:** any drill audit; any PRD/Brief sign-off; any "approved" declaration. Codebase-approved is necessary-not-sufficient.

**Artifacts produced:**
- `DA-07 Drill Map.md` v0.8.4 — 4 CRITICAL PM-quality issues fixed (rows #12, #16, #18, #20, #16a) + pre-launch calibration mandate added
- This learning

**Pattern note:** this is the EIGHTH class of error caught at protocol level during DA suite work:
1-7: previous protocol gaps (codebase-correctness focused)
8: **this entry — codebase-correctness ≠ PM-quality; both required before approval**

The user's "be very critical, systematic, coverage, think like a PM, operator-first" pressure is THE right standard. Codebase verification gets you to "wires don't break"; PM-quality verification gets you to "operator gets what they expect across the suite, with quantified fidelity." Approval requires both bars cleared.

**Meta-meta-pattern: the user has been training the discipline that "approval is earned through zero-finding rounds across BOTH dimensions, not declared after codebase rounds clear." Honest practice: any future declaration of "approved" must explicitly cite "codebase-verified ZERO findings + PM-grade audit ZERO findings + zero-finding rounds across both."**

---

### 2026-05-18: anti-pattern #9 — cross-Brief verification that skips the doc's OWN parent Brief

**Context:** DA-07 Drill Map v0.8.4. I applied a PM-grade fix referencing DA-03 + DA-05 + DA-06 sibling Briefs (correctly catching cross-DA inconsistencies). 2nd PM-grade audit on v0.8.4 caught: **I didn't re-verify against DA-07's OWN Brief.** v0.8.4 contradicted DA-07 Brief v0.2.2 in 2 critical places:
- Row #16: DA-07 Brief commits to DA-05 worklist as canonical with chip as fallback. v0.8.4 inverted this — made chip canonical.
- Rows #12 + #18: DA-07 Brief commits to "Has refund" chip on DA-02 as MUST-SHIP V1. v0.8.4 invented a parallel mechanism (filter 1209 wiring + `due_types_not_in` body param).

**Root cause:** when applying a cross-Brief consistency fix, I treated the "sibling Briefs" as DA-03/DA-05/DA-06 (the OTHER DAs). I didn't think of DA-07 Brief as a sibling — but it's the parent the Drill Map is companion to. The parent Brief's commitments are arguably MORE authoritative than other DAs' Briefs.

**Rule (anti-pattern check #9 — codify in protocol):**

For any companion-doc audit (Drill Map / Build Sheet / Architecture spec), cross-Brief sweep MUST include the doc's OWN parent Brief, not just sibling/input Briefs. Specifically:

1. List ALL Briefs referenced by the doc
2. Include the doc's own parent Brief in that list
3. For each Brief, identify:
   - What commitments does it make that this doc must honor?
   - Are there canonical-vs-fallback structures the doc should mirror?
   - Are there MUST-SHIP commitments the doc must reuse (not invent parallel mechanisms for)?
4. Audit the doc against ALL referenced Brief commitments BEFORE declaring "cross-Brief consistent"

**Specific patterns to look for:**

- **Canonical-vs-fallback inversion:** parent Brief defines a degraded-launch ladder (e.g., "canonical = X; fallback = Y if X slips"). Companion doc inverts (e.g., uses Y as primary).
- **Duplicative mechanism invention:** parent Brief commits to a MUST-SHIP feature; companion doc invents a parallel mechanism that achieves the same outcome with more eng work.
- **Scope claim drift:** parent Brief says "Z is V2"; companion doc assumes Z is in V1 scope.
- **Permission/access drift:** parent Brief specifies permission gate X; companion doc references different gate.

**Sub-agent verification step (extended):**

For every "verified against sibling Briefs" claim, sub-agent must explicitly verify the doc's PARENT Brief too, line-by-line on commitments. Output: "Parent Brief commitments cross-checked: [list with file:line]. Companion doc consistent with parent: ✓/✗ per commitment."

**Applies to:** any companion doc audit; any "approved" declaration on artifacts that have a parent Brief; especially drill maps, build sheets, and architectural references.

**Artifacts produced:**
- `DA-07 Drill Map.md` v0.8.5 — rows #12, #16, #18 reframed to honor DA-07 Brief commitments
- This learning

**Pattern note:** this is the NINTH class of error caught at protocol level during DA suite work. **All nine errors caught share a root cause:** I keep optimizing for "fix what's in front of me" without zooming out to ask "what does the parent doc commit to that my fix must honor?" The discipline: every fix → re-check against ALL parent + sibling Brief commitments before declaring done.

**Meta-meta-meta-pattern:** the user has been training increasingly sophisticated audit discipline:
- Codebase-correctness rounds (5 rounds)
- PM-grade cross-Brief audit (catches what code can't)
- Parent-Brief consistency check (catches what cross-Brief sweep misses)

Each level adds a dimension. "Approved" = zero findings across ALL dimensions in a single verification round.

---

### 2026-05-18: meta-anti-pattern — codifying a lesson in skill learnings is necessary but NOT sufficient; mechanical discipline at every fix is what actually prevents the bug

**Context:** DA-07 Drill Map work. Anti-pattern #7 ("applying newly-discovered fix asymmetrically — catching in one case, missing parallel cases") was caught and codified in v0.8.2. The lesson said: "every fix that touches one row in a structurally-symmetric pair MUST verify the parallel row in the same diff."

**What happened:** v0.8.5 caught me again — fixed Eng Reference for #16 + #20 but missed parallel pair #12 + #18. Codified the lesson AGAIN (in v0.8.6). Then v0.8.7 caught me a THIRD time — applied H-5 reframe to row #21 in main table but missed Eng Reference twin.

**Root cause:** I keep WRITING the lesson but not APPLYING the discipline. The codified rule is words on paper. The actual prevention requires mechanical action at every fix: after every row-level change, immediately grep the doc for `Row #N` to find ALL parallel sections (main table, Engineering Reference, Cross-Brief impact, changelog) and update each OR explicitly note "intentional non-update."

**Rule (meta-anti-pattern — codify in protocol):**

Codifying a lesson in skill learnings is the FIRST step. The SECOND step — mechanical application at every fix — is what actually prevents the bug. Without the mechanical step, codified lessons remain words on paper.

**Specific mechanical discipline for any row-level fix:**

1. Apply the fix to the row in the main table
2. **Immediately grep the doc for `Row #N` or `#N` references** — find ALL sections that mention this row (Engineering Reference, Cross-Brief impact, status summary, changelog)
3. For each parallel section found:
   - Either update it to match the new fix
   - Or explicitly add a note: "intentional non-update — this section reflects [different aspect]"
4. Only after step 3 is complete, mark the fix as done
5. Verification sub-agent must explicitly scan for parallel-section consistency, not assume the writer applied step 3

**For structurally-symmetric content** (e.g., refunds rows #12/#18, deposits rows #17/#18/#20, callouts #14/#15/#16/#16a):

1. When a fix is applied to one member of a symmetric pair/group, the OTHER members must be checked and either:
   - Fixed in the same diff (default)
   - OR explicitly noted as "intentional asymmetry — [reason]"
2. Never just "trust" that the lesson will be applied — mechanical check required

**Apply to:** brief skill, drill map work, any audit work, code review, anything where parallel cases exist.

**Pattern note:** this is the 10TH class of error caught at protocol level — but it's at a META level. It's about how lessons get applied, not what the lessons are.

**Cumulative protocol discipline (as of 2026-05-18):**

1-9: codebase + PM-quality anti-patterns (existing)
10. **Meta: codifying a lesson ≠ applying a discipline. Mechanical parallel-case-scan required at every row-level fix.**

The 10-point discipline list is now what every drill audit must apply. Verification sub-agent must explicitly run the mechanical check, not just verify substance.

---

### 2026-05-19: DA-01 Drill Map work — 9 new anti-patterns (#11-#19)

**Context:** DA-01 Drill Map progressed through v0.1 → v0.5.3 across one session. Each version was caught with errors that prior verification missed. Cumulative learnings:

**#11 — Sub-agent verification ceiling.** Sub-agents (codebase + PM-grade) verified v0.3 as PASS but missed 4 criticals + 3 highs that PM caught on personal re-read. Sub-agents trust the document's own claims rather than independently verifying every load-bearing assertion against ground truth.
- **Mitigation:** for drill maps, treat sub-agent PASS as "no obvious errors found" not "no errors exist"; PM personal verification is non-negotiable on drill-destination + auth claims.

**#12 — Section-count math drift.** v0.3 said "of 13 sections" when Build Sheet has 19 (12 UI + 7 reference). Symbol-level fact unchecked.
- **Mitigation:** every section-count assertion must reference a verified canonical (Build Sheet section list) at write-time.

**#13 — Build Sheet vs code drift goes silent.** v0.2 M1 fix REMOVED a `filter_meta.mode` reference because sub-agent said it was "invented" — but Build Sheet still cites it. Drift between Build Sheet and code went un-flagged.
- **Mitigation:** when removing a drill-map claim because code disagrees with Build Sheet, add an explicit "Build Sheet drift" note rather than silently diverging.

**#14 — Enumeration incompleteness via "not tappable" cherry-pick.** [SUPERSEDED by #19.] v0.3 enumerated SOME non-tappable rows but skipped others. Original mitigation ("enumerate all for consistency") created clutter in v0.5.2 — wrong fix; #19 supersedes.

**#15 — Deferred-section list ≠ comprehensive audit.** v0.4's "scope of this drill map" listed all 8 deferred sections by name but never enumerated their rows. User noticed multiple sections were missing — the scope statement showed WHAT was deferred but didn't surface the SIZE of what was deferred (~30-40 more rows).
- **Mitigation:** when deferring sections, give a row-count estimate ("deferring §5-§12 = ~50 rows") so partial-coverage scale is visible upfront.

**#16 — Drill-destination resolved by canonical PRD, not Build Sheet alone.** v0.3 carried 4 rounds of debate about whether §1-§4 drills are CSB-2-affected. Resolved by reading Analytics PRD line 533 (canonical "why" doc), which together with Build Sheet line 96 made the chain explicit: first-level drill = new endpoint (auth OK), second-level = CSB-2.
- **Mitigation:** drill-destination questions should read the canonical PRD's drill-down matrix FIRST (Part A in Analytics PRD style), not infer from Build Sheet section headers.

**#17 — Dual tap targets per UI element missed.** Many UI elements have multiple tap surfaces (bar segment + label row in urgency bar; entire card vs CTA in Action Card; accordion header + content; tooltip ⓘ + parent metric). v0.4 collapsed each into a single row. v0.5 enumerated per Analytics PRD line 228.
- **Mitigation:** for every interactive element, ask: "what are ALL the tap surfaces an operator might tap to get this outcome?"

**#18 — Permission-driven action hiding is a Truth, not a per-row note.** Every "Primary row action" in a drill map (Send Reminder, View Bill Detail, Adjust from Deposit) is a CONDITIONAL surface — hidden if operator lacks permission. v0.5 enumerated primary actions per row without flagging the permission gate. v0.5.2 codified as a suite-wide Truth with inline `(permission-gated)` reference on every drill row.
- **Mitigation:** when listing actions per drill row, treat the action availability as a function of role + permission, not a constant.

**#19 — Drill maps must not become Build Sheet duplicates.** v0.5.2 enumerated 27 non-tappable rows (51% of total) including QA invariants, sort orders, color rules, visual styles, render rules — all of which already live in Build Sheet. This cluttered the drill map and distorted its status summary. User flagged: "things that are not tappable shouldn't come as they make it cluttered & unnecessary."
- **Mitigation:** drill maps enumerate ONLY (a) tap targets, (b) situational display states that affect operator UX (e.g., zero-data placeholders, ÷0 guards, Setting-Up overlays), (c) section visibility rules. Everything else — display labels, QA invariants, sort orders, visual specs, color rules — stays in Build Sheet, optionally cross-referenced from drill map section headers as "Also rendered (display only): …". Lean drill map = ~22-35 rows per ~8-section audit, not 50+.

**Apply to:** brief skill, drill map work for ALL DA suite (DA-01 through DA-07), companion docs broadly, any audit work where Build Sheet + Analytics PRD + canonical "why" doc all coexist.

**Artifacts produced:**
- `DA-01 Drill Map.md` v0.5.3 — 33 rows (was 53 in v0.5.2 — 38% reduction via #19 application)
- Anti-patterns #11-#19 codified in DA-01 drill map AND here

**Pattern note:** these are the 11th-19th anti-pattern classes caught at protocol level. The DA-01 work surfaced TWO meta-level patterns: (#11) sub-agent verification has a ceiling that PM personal reading exceeds; (#19) more enumeration is not always better — drill map's purpose is tap behavior, not Build Sheet completeness. Going forward (DA-02 onwards), drill maps start with the lean structure from kickoff, applying all 19 patterns proactively.

**Cumulative protocol discipline (as of 2026-05-19):** 19 anti-pattern classes codified. Every drill map audit must apply all 19 from kickoff. Verification protocol: PM personal verification primary; sub-agents secondary (3-angle: correctness, plain-English, coverage). Convergence target: 2-3 rounds to PASS (DA-01 took 7 versions; DA-02 should converge faster with patterns active from kickoff).

---

### 2026-05-24: Landlord Module LM-00 Master Brief — v1.4→v1.5 stress-test learnings

**Brief:** LM-00 Landlord Module Master Brief
**Session:** CPO-grade stress-test of master brief after v1.4 edits. Found 7 issues (2 BLOCKER, 3 HIGH, 2 MEDIUM), all resolved in v1.5.

**#20 — Visibility matrix cells must be homogeneous.** LM-00 v1.4 had a "To Pay" cell reading "Default: visible (when amount owed)" — a data-conditional rule in a matrix where every other cell was a permission default. The inconsistency was subtle but broke the pattern: operators configure defaults at setup time, not at runtime. Every cell in a visibility/permission matrix must be the same type of value.
- **Mitigation:** before finalizing any matrix, scan every cell and confirm they're all the same category (permission default, data state, capability flag — pick one). Mixed categories = flag as violation.

**#21 — MVP floor must be dependency-complete.** LM-00 v1.4 listed "Booking Approval" in MVP but not "Bookings" or "Active Tenants." Booking Approval requires Bookings to exist, and Bookings requires Active Tenants to be visible (you can't approve a booking if you can't see who's booking). PM caught this after two rounds.
- **Mitigation:** after writing MVP floor, trace each item's prerequisite chain. If Feature X depends on Feature Y being visible/present, Y must also be in MVP. Run this check explicitly — dependency gaps aren't obvious when items come from different briefs.

**#22 — Absolute competitive claims are falsifiable landmines.** LM-00 v1.3 stated "No Indian competitor has a dedicated landlord product." A web search found KIPINN Business and NestAway Owners within minutes. The claim was wrong, and a CPO reading this would lose trust in the entire brief.
- **Mitigation:** never write "no competitor has X" without a verification step. Either (a) run a competitive scan before making the claim, or (b) reframe as differentiation ("competitors show numbers; we add actionability"). The differentiation framing is almost always stronger anyway — it says what YOU do, not what others don't.

**#23 — Success metrics must pass the instrumentation test.** LM-00 v1.3 had "operator-reported reduction in landlord calls" as a success metric. You can't instrument phone calls from inside an app. Every metric must answer: "what in-app event or API call measures this?" If the answer is "survey" or "external behavior," it's a counter-metric at best, not a north star.
- **Mitigation:** for each proposed metric, ask "how do I write the analytics event that fires?" If you can't describe the event, the metric is unmeasurable. Behavioral metrics (view rate within time window, task completion rate) > self-reported metrics (survey, call volume).

**#24 — Gatekeeper products need dual-sided value propositions.** LM-00's Problem section pitched value to the landlord (Ramesh sees his money) but not the operator (Vikram activates the module). The operator is the gatekeeper — if the brief doesn't explain why Vikram would turn this on, the product never reaches Ramesh.
- **Mitigation:** when the product has an activation gatekeeper (operator, admin, manager), the Problem section must pitch value to BOTH the end-user AND the gatekeeper. Phase 3 gather should include: "Why does the gatekeeper activate? What's their incentive?"

**#25 — "All sections hidden" is a real edge case when operators control visibility.** LM-00's universal operator override (Decision #19) means an operator could theoretically hide every section, leaving the landlord with an empty app. The brief must address this explicitly — either block activation, warn, or accept the consequence.
- **Mitigation:** whenever a brief introduces a permission/visibility system that can hide content, add a specific trap: "What happens when everything is hidden?" Decide: hard block, soft warning, or accept. LM-00 chose soft warning (activation guard that warns but doesn't block).

**#26 — Data readiness risk is standard for actor-dependent products.** LM-00's landlord sees data entered by the operator. If the operator activates but hasn't entered settlement data, the landlord sees empty screens and calls the operator — the exact behavior we're eliminating. This risk recurs in any product where Actor A sees data entered by Actor B.
- **Mitigation:** add "Data readiness" as a standard risk prompt in the Traps & Risks template. The brief must specify: (a) what happens when data isn't ready (empty state copy with timing), (b) whether activation is blocked until data exists or empty states handle the gap.

**#27 — "Operator override" as a reusable design pattern.** LM-00 landed on a clean pattern: system sets visibility defaults per commercial model → operator overrides any default per property → no section is enforced-visible. This pattern (default + override + no enforcement) recurs across permission systems, notification preferences, feature flags. Naming it upfront prevents re-debating it in every section brief.
- **Mitigation:** during Phase 3 gather, ask: "Does this feature have defaults that someone can override? If so, who overrides, and is anything enforced (non-overridable)?" Lock the override philosophy before drafting — it shapes every capability's framing.

**Apply to:** all Landlord Module section briefs (LM-01 through LM-10), and any future brief with: visibility matrices (#20), MVP floor lists (#21), competitive claims (#22), success metrics (#23), gatekeeper activation (#24), permission systems (#25), cross-actor data dependency (#26), or default+override patterns (#27).

**Cumulative protocol discipline (as of 2026-05-24):** 27 anti-pattern classes codified. #1-#10 from DA suite Phase 3-5 work. #11-#19 from DA-01 drill map audit. #20-#27 from LM-00 master brief stress-test.

---

### 2026-06-04: DA-08 Occupancy — three behavioral learnings extracted into skill changes (net-new only)

**Context:** After the DA-08 Brief → Ground-Truth Formula Map → Build Sheet cycle, harvested the *feature-agnostic* behavioral learnings (not occupancy facts). Most candidates were already covered by this skill (Phase 2 grounding, operator-pain ranking, WHAT-not-HOW altitude, AI-slop hygiene, Phase 5 critique). Three were genuinely net-new and applied; four "sharpen existing" edits (refine-don't-overcorrect, define-semantic-before-spec, honest-degradation-over-fake-data, defer-vs-hedge as a Phase rule) were deferred by the user to a later pass.

**#28 — Correctness-pushback authority (applied → new hard-rule 12).** When Phase 2 grounding reveals the code's own definition is wrong (misnamed state, metric computed two ways, a label that lies to the operator), the brief flags it and recommends the fix — it does not silently inherit the error to "match the code." Verified example: code names living-tenants-over-capacity `OVERBOOKED_BEDS`; the right word is "over-occupied," and shipping the wrong word on a trust screen *is* the risk. State current behavior + file:line, state the corrected definition, mark as deliberate pushback.

**#29 — The document-altitude ladder + Build Sheet method (applied → `references/build-sheet.md`).** Brief (why + WHAT) → ground-truth contract (schema-true, file:line-cited, with a Time × Number coverage matrix) → Build Sheet (HOW + tasks). Build Sheet method: data-foundation section first, one task table (Task · Why · Files/queries · Acceptance check), **"Why" = one sentence with citations kept in their own column**, concrete testable acceptance cells, de-bold. Mixing altitudes is the most common authoring failure.

**#30 — Deferring ≠ hedging (documented in `references/build-sheet.md`).** Hard-rule 4 says resolve/schedule/drop every hedge — but a genuine owner's-call decision, scoped + recommended + dated, is a *decision handed off*, not a hedge. Hedging is unowned ("worth a check"); deferring is owned ("you decide X at kickoff; I recommend Y because Z"). Don't force-resolve a real owner's-call, and don't dress a hedge up as a deferral.

**Also produced (cross-skill):** new top-level `doc-handoff-review` skill — the dual-dimension adversarial pass (fact-check against sources + readability) run before any doc handoff; logged in `skill-router/learnings.md`. The brief skill's Phase 5 and the new skill compose: Phase 5 is substantive critique during authoring; `doc-handoff-review` is the final gate, and it re-verifies *derived* docs against their parent contract (the silent-drift failure mode Phase 5 doesn't cover).

**Applies to:** SKILL.md hard-rule 12 (new), lifecycle ladder pointer (new), `references/build-sheet.md` (new). Deferred for a later pass per user: refine-don't-overcorrect, define-semantic-before-spec, honest-degradation, defer-vs-hedge-as-Phase-rule.
## 2026-06-06 - Private-dictionary words can survive "plain language" unless scanned explicitly

Problem:
- A brief can look clean and still read in internal PM/spec language.
- Words like `operator`, `surface`, `flow`, `drill`, `handoff`, `contract`, and `aggregate` help the writer think, but they make reader-facing docs feel foreign.
- Generic readability review is not enough if it does not check for this exact failure mode.

Fix:
- Ban the skill's own working vocabulary from the brief body unless the product itself uses that word.
- Run an explicit private-dictionary scan before handoff.
- Keep code references and implementation anchors out of reader-facing briefs and formula maps; keep them in the build sheet.

## 2026-06-06 - DA-05 audit gap: concise sections can still exceed the brief word limit

Problem:
- The DA-05 Brief looked section-clean after write-time hygiene, but the mechanical audit still found the body over the target length.
- The bloat came from small explanatory sentences across many sections, not one obvious long section.

Fix:
- Run word count before Phase 5 critique, not only at final handoff.
- After drafting each brief section, ask whether the sentence changes a product decision. If not, cut it or move it to the Build Sheet.
