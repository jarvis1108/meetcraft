---
name: event-debrief
description: Sediment what happened at an event before it fades. Use when the user mentions a meetup, workshop, conference, cohort session, or 1-on-1 networking conversation just ended (or pastes a transcript from one). Routes single-session events to event-debrief.md and multi-session cohorts/conferences to cohort-debrief.md folder convention. Captures takeaways, people met, feedback dispositions, and follow-up actions in a structured way that survives the 24-hour memory cliff.
---

# Event Debrief

Most networking memory dies within 24 hours of the event. This skill catches it before it fades — converting raw notes or a transcript into a structured debrief that you can re-read in 3 months and still recognize what happened.

## When to invoke

Auto-trigger when the user:

- Mentions "I just got back from {meetup, workshop, conference, mixer, cohort session}"
- Pastes a Krisp / Whisper / Otter transcript and asks for synthesis
- Says "let's debrief {event}" or "process the {event} transcript"
- Mentions a cohort week wrapped (recurring multi-session event)
- Pastes notes from a 1-on-1 networking call and asks how to capture them

Manual invocation: `/event-debrief` or just paste materials and ask for a debrief.

## Decision: which template

Run this branch BEFORE writing anything:

| Signal | Template |
|--------|----------|
| Single session, ≤4 hours, one location/day | [`templates/event-debrief.md`](templates/event-debrief.md) |
| Multi-day or multi-week (cohort, conference, recurring weekly meetup), 5+ sessions, significant material per session | [`templates/cohort-debrief.md`](templates/cohort-debrief.md) folder convention |
| 1-on-1 networking call (informational chat, coffee, intro call) | Use single-session template, scope to that one conversation |

If unclear, ask: "Single-session debrief, or multi-session cohort folder?" Don't guess — the wrong choice forces a restructure later.

## Workflow

### Phase 1 — Material check

1. Recording? → guide through pre-processing first:
   - **Krisp** for live speaker diarization (free, has speaker labels, but ~5-9 critical word errors per 3 min)
   - **Whisper-medium or larger** post-hoc for word accuracy if you'll quote specific phrases
   - **Speaker label + cleanup BEFORE analysis** — analyzing raw transcripts produces compounding errors that contaminate every downstream section
2. No recording? → ask the user to brain-dump their top 3 memorable moments before the cliff hits; use that as the seed for Sessions / Conversations

### Phase 2 — Apply template

Single-session: write [`templates/event-debrief.md`](templates/event-debrief.md) sections in order:
1. Event metadata (date, location, duration, format)
2. Top Takeaways — 3-5 cross-cutting lessons (NOT a session-by-session summary). If more than 5 surface, the user is capturing details, not takeaways.
3. People Met — table with follow-up status per person
4. Sessions / Conversations — chronological, one block per substantive interaction
5. Feedback Received — table with disposition per item (adopt / consider / pass)
6. Follow-up Actions — concrete checkboxes

Multi-session cohort: instantiate the folder structure from [`templates/cohort-debrief.md`](templates/cohort-debrief.md):
- `index.md` (summary + status + per-session links)
- `takeaways.md` (living doc — append as cohort progresses, never rewrite)
- `participants.md` (cross-session people)
- `per-week/week-N.md` (uses single-session template inline, scoped to that week)
- `archive/transcripts/` (raw + cleaned recordings)

### Phase 3 — Disposition + propagate

Each piece of captured info has a downstream home. Flag these at end of debrief:

| Captured | Goes to | Why |
|----------|---------|-----|
| New contacts in People Met table | Cross-event contact database (e.g. `connections.md` or your CRM) | Event-scoped table is for context; long-term network lives elsewhere |
| Feedback marked **adopt** | Backlog (P1/P2 item with area + estimate) | Otherwise it dies in the debrief file |
| Feedback marked **consider** | Workflow insights / friction log | Pattern detection over time |
| Feedback marked **pass** | Stays in debrief, no further action | Worth recording for pattern detection |
| Backlog spawn from a session | Backlog with link back to debrief file | Cross-reference both directions |

Don't complete the debrief without listing these propagation actions explicitly. If the user has no system for the downstream homes, suggest creating one — but don't silently swallow these signals.

## Anti-patterns

- **Skipping transcript pre-processing** — analyzing raw Krisp output produces speaker-attribution errors that compound through every section. Always speaker-label + cleanup first.
- **Mixing sessions and feedback** — Feedback received DURING a session belongs in the Feedback table, not in the session block. Otherwise dispositions get lost.
- **Rewriting `takeaways.md`** in cohort case — append-only. Past entries stay even if the user's view shifts. The trail is the point.
- **Closing without propagation list** — if the contacts / backlog / friction items aren't named for follow-up, the debrief is a dead artifact.
- **Forcing single-session template on cohort material** — single file overflows above 5+ sessions. Use the folder convention.

## Skill output format

End every debrief run with:

```
## Debrief complete

- File created: {path}
- Propagation pending: {N} actions
  - [ ] Add {N} new contacts to {your contact db}
  - [ ] File {N} adopt feedback as backlog items
  - [ ] File {N} consider feedback as workflow insights
- Recommended next session: within 24h while memory is hot
```

The list at the end is not optional. It's what keeps the debrief from becoming a dead file.

## Reference

- [`templates/event-debrief.md`](templates/event-debrief.md) — single-session template
- [`templates/cohort-debrief.md`](templates/cohort-debrief.md) — multi-session folder convention
- [`examples/portfolio-critique-workshop.md`](examples/portfolio-critique-workshop.md) — anonymized example showing a 2-day workshop debrief with feedback dispositions and propagation
