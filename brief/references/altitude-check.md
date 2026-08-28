# Altitude Check

Wrong altitude = wrong brief. Run this BEFORE the gather phase. Most brief failures come from writing at the wrong altitude — we hit this on DA-01 (started as a daily call-list worklist, was actually a periodic synthesis screen). Catch it now.

## The four altitudes

| Altitude | Job | Frequency | Example |
|----------|-----|-----------|---------|
| **Signal** | At-a-glance "anything broken right now?" | Daily / continuous | Homescreen tiles, top-bar badges, login dashboard |
| **Synthesis** | "What shape is the problem?" — multiple cuts of the same data, in one place | Periodic / triggered (2-3×/week or before a meeting) | Analytics screens, dashboards, executive views |
| **Action** | "Which exact item do I act on right now?" — per-item granular | Daily / continuous | Worklist, inbox, task queue, call list |
| **Authoring** | Create / modify entities | As-needed / event-driven | Editor, form, settings, onboarding wizard |

## The five altitude-locking questions

Ask before the main gather phase:

1. **What does the user do BEFORE this thing opens?** (their entry trigger / signal)
2. **What does the user do AFTER they're done with it?** (their next action / handoff)
3. **What does THIS thing do in between?** (the actual job)
4. **Are there already screens/flows that overlap?** (e.g., if a worklist already exists, this CAN'T be a worklist)
5. **What's the frequency of use?** (daily / weekly / triggered / one-off)

The answer to these five collapses the altitude:
- Entry = signal + this = synthesis + exit = action → this is **Synthesis** (DA-01's actual altitude)
- Entry = nothing + this = signal + exit = synthesis or action → this is **Signal** (homescreen)
- Entry = synthesis + this = action + exit = done → this is **Action** (worklist)
- Entry = trigger + this = create entity + exit = done → this is **Authoring** (form)

## Red flags by altitude

### Signal
- More than ~3 numbers/visible elements → too much; this should be synthesis
- Detailed lists → wrong altitude; that's worklist territory
- "Daily ritual" framing → fine
- Notifications / push → defensible (signal IS the interruption layer)

### Synthesis
- "Top N to call" framing → leakage from worklist; refactor as awareness or handoff
- "Daily ritual" → wrong; synthesis is periodic, not daily
- Per-item action buttons in the spec → leakage from worklist
- Notifications / push → fail the chai test; synthesis is operator-initiated
- "Build a call list" or "work my way through" → that's worklist talk

### Action / Worklist
- Aggregated views / charts → that's synthesis leaking down
- "Get a picture of" framing → wrong altitude; worklist gives a flat list
- Trend signals → that's synthesis
- "Once a week" frequency → wrong; worklist is continuous

### Authoring
- "View" or "see" framing → wrong; authoring is about creating/modifying
- Aggregated metrics → wrong altitude
- Multi-user views → probably synthesis or signal

## What to do if you suspect wrong altitude

**Stop the gather.** Re-ask the user:

> "What is the user actually doing when they open this — checking [signal], understanding [synthesis], acting [action], or creating [authoring]?"

If the answer doesn't match the brief draft so far, the brief is lying. Re-anchor before continuing.

## The diagnostic example (DA-01)

V0.2 of DA-01 Brief was written as if the screen was a daily call-list builder:
- "Rajesh opens the app over chai at 8:30am" — daily ritual framing
- "Wants to know: who do I call first?" — worklist question
- "Goes by gut, calls his staff" — per-item action framing
- Success metric: "spot top 3 to call in 5 seconds" — action altitude
- Q7: "Top 5 to call right now" — call-list spec

But the screen sits at **synthesis altitude**. The worklist already exists. The homescreen already exists. This screen answers "what shape is the problem?" — periodic, not daily; awareness, not action.

V0.4 caught the altitude mistake and refactored:
- Frequency shifted: daily ritual → 2-3×/week or trigger-based
- Trigger shifted: build call list → check the state of dues
- Success metric: spot biggest pocket of stuck money (synthesis) → drill to worklist (action handoff)
- Q7: "Top 5 to call" → "Top of the stack" + drill-out to worklist

The structural problem (worklist-coded altitude) had to be fixed BEFORE any vocab / hedging polish would matter. Polish a worklist-coded brief and you still have a worklist-coded brief.

## Multi-altitude features

Some features span altitudes (e.g., a tile on the homescreen + a detailed analytics view + a worklist). Write **one brief per altitude**, not one mega-brief. They're different jobs even if the data is shared.

If the brief covers more than one altitude, the brief is wrong — split it.

## Output of altitude check

Before phase 2 starts, the skill should state:

> Altitude locked: **[Signal / Synthesis / Action / Authoring]**.
> Entry: [...]  → This screen: [...] → Exit / handoff: [...]
> Frequency: [...]

If unsure, ask the user explicitly with these 5 questions before proceeding.
