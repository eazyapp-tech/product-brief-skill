# Vocab Stop-List

Words / phrases that mean nothing, leak jargon, or signal AI slop. The skill catches these on the polish pass and replaces with plain alternatives.

## AI slop (cut entirely — these are red flags)

- leverage, synergy, robust, best-in-class, world-class, comprehensive
- next-generation, intuitive, seamless, frictionless, delight
- holistic, paradigm, scalable (when vague), powerful (when vague)
- optimize (vague), streamline, empower, facilitate
- empowerment, enablement, transformation, journey (in the marketing sense)
- "at scale," "in the wild," "in production" (when not actually about production deployment)
- "users want X" without evidence
- "we believe X will" — what's the evidence?

If a sentence contains 2+ from this list, rewrite it.

## Borrowed framework jargon

| Avoid | Use instead |
|------|-------------|
| TL;DR | In one line |
| load-bearing | must ship / core |
| stretch | nice to have |
| fat marker | the must-ship priority call |
| top of stack | describe what the list actually is (e.g., "biggest items still owed") |
| kill criteria / kill signals | when to stop and reconsider |
| appetite | time budget |
| MoM | vs last month |
| GAAP | cut entirely (no PG owner says this) |
| Hero / hero number | the main number / headline |
| pre-V1 | before launch |
| synthesis altitude | cut entirely |
| per-tenant mode / per-X mode | one X at a time |
| diagnostic / diagnostic scan | checking |
| rabbit holes | traps we've thought through |
| breadboard | (only use if you actually drew one) |
| pitch | brief (if you don't have a breadboard) |

## Industrial-strength PM-speak

| Avoid | Use instead |
|------|-------------|
| operationalize | put into practice |
| stakeholder alignment | get everyone agreed |
| north-star metric | the one number that matters |
| value prop | what the user gets |
| user delight | cut — no one says this for real |
| friction (as a noun) | cut or specify exactly what slows the user |
| solutioning | designing / building |
| circle back | come back to |
| double-click | look deeper at |
| boil the ocean | try to do too much |
| reach out | message / call |
| touch base | check in |
| moving forward | next / from here |
| at the end of the day | (cut — meaningless) |
| low-hanging fruit | the cheap wins |

## Name-dropping (drop unless earned)

| Don't invoke | Unless... |
|--------------|-----------|
| "Zerodha doctrine" / "don't interrupt the user" | the doc actually applies their thinking, not just the name |
| "Cagan's Four Big Risks" | you're explicitly running the risk framework |
| "Shape Up Pitch" | you have a breadboard + scope-cut ladder + appetite |
| "Lenny 1-pager" | reference the structure, don't invoke the name |
| "Razorpay Concept Note" | same |
| "Marty Cagan" | same |
| "Linear's project brief" | same |

Rule: famous-PM name-drops are only allowed if the doc earns the borrow. Otherwise they're decoration.

## Engineering-effort hedges (REQUIRE codebase citation)

These are PM-speculation tells. Every one of them needs a file:line, function name, or graphify node ID — or it gets cut.

| Phrase | What it usually means | What to do |
|--------|----------------------|------------|
| "just need to" / "we just" | PM hasn't read the code | Cite the function being changed |
| "small change" | PM is guessing | Cite the file:line — if > 1 file, it's not small |
| "easy filter widen" | Filter logic is rarely simple in real codebases | Cite the actual filter code |
| "should be straightforward" | Hasn't been verified | Cite or drop |
| "easy lift" | Sales pitch to engineering | Cite scope |
| "quick fix" | Famous last words | Cite the file:line |
| "trivial query" | PM doesn't know what queries already exist | Cite via graphify or grep |
| "we already have X" | Maybe — needs verification | Cite where X lives |
| "the backend already does this" | Maybe — half-truth common | Cite the function |
| "minor migration" | All migrations are non-minor in production | Cite the table + estimated row count |

**Rule:** if the brief makes an engineering claim and the codebase grounding (Phase 2) didn't verify it, either ground it now or drop the claim.

## Team-served vs user-served decision tells

When a decision in the brief is being justified, the rationale should reference user-need or user-pain. If it references team convenience without user logic, flag.

| Team-served phrasing (flag) | User-served rephrasing (correct) |
|------------------------------|-----------------------------------|
| "out of scope" | "[user need not met by this] — [bigger user pain we're addressing first]" |
| "defer to V2 / V3" | "[user-side reason they accept not having it in V1]" |
| "design has concerns" | "[user impact of those concerns; if no impact, drop the line]" |
| "backend is complex, start simple" | "[user-need met by the simple version; what they get on day one]" |
| "we'll come back to it" | "[is the user okay without this? if yes, say why; if no, this isn't a cut]" |
| "lower priority for engineering" | "[lower priority for users because X]" |
| "nice to have for the team" | "[users would notice this missing but not block on it because Y]" |

Brief is read by the team; product is for the user. Every justification in the Brief — even cuts and defers — should be the *user's* justification, not the *team's*.

---

## Cut-order language tells (engineering-leaning vs operator-leaning)

If you see these in a cut-rationale, the cut order is build-cost ranked, not operator-pain ranked — flag (see Operator critique lens).

| Engineering-leaning rationale (flag) | Operator-leaning rationale (correct) |
|--------------------------------------|--------------------------------------|
| "cheap to cut" | "operator would miss this least because [pain logic]" |
| "easy to drop" | "rare event / already in another flow" |
| "low build cost" | "informational, not action-driving" |
| "drops one backend task" | "monthly/strategic, not the daily pull" |
| "trivial implementation" | "covered by another question's tooltip / hover" |
| "closest to free" | "operator would miss this least because [pain logic]" |
| "light to build" | "operator would miss this least because [pain logic]" |
| "smallest backend lift" | "operator would miss this least because [pain logic]" |
| "least eng cost" | "operator would miss this least because [pain logic]" |
| "the smallest task" | "operator would miss this least because [pain logic]" |
| "lowest-effort cut" | "operator would miss this least because [pain logic]" |

Rationales should read as "the operator would miss this least because X" — not "this is the cheapest task to remove."

## Hedging language (decide, schedule, or drop)

| Hedge | What to do |
|------|-----------|
| "worth a check" | Schedule the check in the footer with owner + week, OR decide now |
| "TBD" | Replace with owner + week, or drop the line |
| "should we" | Decide |
| "might want to" | Decide |
| "let's discuss" | Discuss now and write the answer |
| "open question" | Move to footer's validation tasks OR resolve |
| "we'll see" | Decide |
| "needs further thought" | Think now, then decide |
| "depends on" | Spell out the dependency + the decision under each branch |

## Idiom / metaphor traps

| Avoid | Use instead |
|------|-------------|
| Bleeding (money) | Losing |
| Plug the leak | Fix the problem |
| Move the needle | Make a difference |
| Drinking from a firehose | Too much at once |
| Boil down to | Comes down to |
| Wear two hats | Do two jobs |
| The whole nine yards | Everything |
| Cut to the chase | Get to the point |
| Hit the ground running | Start fast |

## Bharat-context vocabulary (RentOk specifically)

These are English-banker words that don't translate to operator Hindi/Tamil/Telugu/Kannada — replace with plain equivalents.

| Avoid | Use instead |
|------|-------------|
| Defaulters | Tenants who haven't paid |
| Outstanding (amount) | Unpaid |
| Bleeding worst | Losing the most money |
| Aging (banking) | How-long-overdue |
| Receivables | Money owed |
| Reconciliation | Matching the numbers |
| Tenure (subscription) | How long they've been on |

## Repetition rule

If the same phrase / metaphor appears 3+ times in the doc, it's a refrain — collapse to one canonical instance. Common offenders from past drafts:
- "go by gut" (used 3× in DA-01 v0.5 → collapsed to 2)
- "synthesis altitude" (over-used, then cut entirely)
- "rhythm of reminders breaks" → "reminders stop going out on time"
- "what shape the problem is" — OK to use up to twice; not three times

## What to do when you catch one

Don't apologize, don't explain — replace silently in the polish pass. If the word is genuinely the right one (rare), keep it AND footnote why.
