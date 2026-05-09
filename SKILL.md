---
name: meetcraft
description: Prep before professional conversations and sediment what came out after — for events (meetups / workshops / conferences / cohort sessions) and 1-on-1s (informational interviews / coffee chats / mentor catch-ups). Routes to four templates by situation. Captures intent before, signal after, and enforces a propagation list so captured signal doesn't die in the file. Use when the user mentions an upcoming or just-finished networking conversation, or pastes a Krisp/Whisper/Otter transcript and asks for synthesis, or asks to "prep for" / "write up" / "recap" a conversation.
---

# meetcraft

Most networking memory dies within 24 hours of an event — and most underprepared conversations leak the value they could have created. This skill catches both ends:

- **Prep mode** — for upcoming conversations, surface intent + named priorities + lines of inquiry before you walk in
- **Debrief mode** — for just-finished conversations, sediment people / takeaways / feedback / follow-ups before the 24-hour memory cliff hits

Output is a structured markdown file you can re-read in 3 months and still recognize what happened (or what you set out to learn), plus an explicit propagation list so the captured signal doesn't die in the file.

## When to invoke

Auto-trigger when the user:

**Prep signals**
- "I have a {meetup / workshop / conference / coffee chat / informational interview} coming up"
- "I'm meeting {person} tomorrow / next week"
- Asks to "prep for {event}" or "get ready for {call}"
- Mentions an upcoming cohort session and wants to plan their angle

**Debrief signals**
- "I just got back from {meetup, workshop, conference, mixer, cohort session}"
- Pastes a Krisp / Whisper / Otter transcript and asks for synthesis
- Says any of: "let's debrief {event}" / "process the {event} transcript" / "write up the {event}" / "recap {event}" / "summarize what came out of {event}"
- Mentions a cohort week wrapped (recurring multi-session event)
- Mentions "notes from yesterday's {event}" or similar past-tense framing
- Pastes notes from a 1-on-1 networking call, coffee chat, or informational interview and asks how to capture them

Manual invocation: `/meetcraft` or just paste materials and ask for prep / debrief.

## Routing — which template

Run this branch BEFORE writing anything. Wrong choice forces a restructure later.

| Situation | Template | Mode |
|-----------|----------|------|
| Multi-person event coming up (meetup / workshop / conference / cohort session) | [`templates/event-prep.md`](templates/event-prep.md) | Prep |
| Multi-person event just happened, ≤4 hrs single session | [`templates/event-debrief.md`](templates/event-debrief.md) | Debrief |
| Multi-day or multi-week (cohort / conference / recurring weekly), 5+ sessions | [`templates/cohort-debrief.md`](templates/cohort-debrief.md) folder convention | Debrief |
| 1-on-1 coming up (coffee chat / informational interview / mentor catch-up / intro call) | [`templates/one-on-one-prep.md`](templates/one-on-one-prep.md) | Prep |
| 1-on-1 just happened | [`templates/one-on-one-debrief.md`](templates/one-on-one-debrief.md) | Debrief |

If unclear → ask: "Prep or debrief? Group event or 1-on-1?" — don't guess.

## Workflow — Prep mode

### Phase 1 — Context gathering

Pull these out of the user before writing anything. Skip nothing — the gaps are what the prep is meant to surface:

- **Who are you meeting / who's the audience?** Names + roles + (if 1-on-1) prior interaction history
- **What's the format?** Group event vs 1-on-1 / video vs in-person / time budget / venue constraints
- **What's your primary intent?** Pick one — relationship-building / intel-gathering / specific ask / specific offer. Mixed intents need explicit ranking
- **What do you already know?** Past notes, outreach-log entries, prior calls — paste them in if they exist

If the user can't answer any of these, the prep is the wrong medium for the gap — they need to do the research first.

### Phase 2 — Template fill

Hand the right prep template to the user with their context already woven in. Claude drafts; user edits inline. Use the user's actual names / context throughout — generic prep files don't survive the moment of truth.

### Phase 3 — Output location

Single output file at user's chosen path. Suggest one of:
- Co-located with the event/contact's record (e.g., `events/{date}-{slug}-prep.md` or `contacts/{name}-prep.md`)
- A dedicated `prep/` folder if the user batches multiple per week

If the user has no convention yet, propose one and ask. Don't silently pick a path and hope they like it.

## Workflow — Debrief mode

### Phase 1 — Material check

1. Recording? → guide through pre-processing first:
   - **Krisp** for live speaker diarization (free, has speaker labels, but ~5-9 critical word errors per 3 min)
   - **Whisper-medium or larger** post-hoc for word accuracy if you'll quote specific phrases
   - **Speaker label + cleanup BEFORE analysis** — analyzing raw transcripts produces compounding errors that contaminate every downstream section
2. No recording, multi-person event? → ask the user to brain-dump their top 3 memorable moments + names of who they spoke to before the cliff hits; use that as the seed for Sessions / Conversations
3. No recording, 1-on-1 call? → ask the user to recall the 2-3 substantive things the other person said + anything they committed to follow up on; use that as the seed for the single Session block

### Phase 1.5 — Output location

Decide where the debrief lives before writing. Suggest one of:
- Single-session: alongside any pre-event prep file, e.g., `events/{YYYY-MM-DD}-{slug}-debrief.md`
- Multi-session cohort: a folder, e.g., `events/{YYYY-MM}-{slug}/` per the cohort-debrief.md convention
- 1-on-1 call: alongside the contact's record, or in a `calls/` folder

If the user has no convention yet, propose one and ask. Don't silently pick a path and hope they like it.

### Phase 2 — Apply template

**Multi-person single session**: write [`templates/event-debrief.md`](templates/event-debrief.md) sections in order:
1. Event metadata (date, location, duration, format)
2. Top Takeaways — 3-5 cross-cutting lessons (NOT a session-by-session summary). If more than 5 surface, the user is capturing details, not takeaways
3. People Met — table with follow-up status per person
4. Sessions / Conversations — chronological, one block per substantive interaction
5. Feedback Received — table with disposition per item (adopt / consider / pass)
6. Follow-up Actions — concrete checkboxes

**Multi-session cohort**: instantiate the folder structure from [`templates/cohort-debrief.md`](templates/cohort-debrief.md):
- `index.md` (summary + status + per-session links)
- `takeaways.md` (living doc — append as cohort progresses, never rewrite)
- `participants.md` (cross-session people)
- `per-week/week-N.md` (uses single-session template inline, scoped to that week)
- `archive/transcripts/` (raw + cleaned recordings)

**1-on-1 call**: write [`templates/one-on-one-debrief.md`](templates/one-on-one-debrief.md) — different shape from the multi-person template (deep on one thread, not broad across many people):
1. Topic threads — what we covered, depth not breadth
2. Their POV — what the other person actually thinks about X (substantive parts)
3. My asks vs offers — balance check
4. Surprises — anything that updated my model of them or the topic
5. Next-touch cadence — when + bridge
6. Sediment for cross-company tracker — 1-line summary
7. Propagation list

### Phase 3 — Disposition + propagate

Each piece of captured info has a downstream home. Flag these at end of every debrief:

| Captured | Goes to | Why |
|----------|---------|-----|
| New contacts in People Met table | Cross-event contact database (e.g. `connections.md` or your CRM) | Event-scoped table is for context; long-term network lives elsewhere |
| Feedback marked **adopt** | Backlog (P1/P2 item with area + estimate) | Otherwise it dies in the debrief file |
| Feedback marked **consider** | Workflow insights / friction log | Pattern detection over time |
| Feedback marked **pass** | Stays in debrief, no further action | Worth recording for pattern detection |
| Backlog spawn from a session | Backlog with link back to debrief file | Cross-reference both directions |
| 1-on-1 contact post-debrief | `connections.md` row + (if substantial) outreach-log update | Otherwise the relationship trajectory is invisible to future-you |

Don't complete the debrief without listing these propagation actions explicitly. If the user has no system for the downstream homes, suggest creating one — but don't silently swallow these signals.

## Anti-patterns

- **Skipping transcript pre-processing** — analyzing raw Krisp output produces speaker-attribution errors that compound through every section. Always speaker-label + cleanup first.
- **Mixing sessions and feedback** — Feedback received DURING a session belongs in the Feedback table, not in the session block. Otherwise dispositions get lost.
- **Rewriting `takeaways.md`** in cohort case — append-only. Past entries stay even if the user's view shifts. The trail is the point.
- **Closing without propagation list** — if the contacts / backlog / friction items aren't named for follow-up, the debrief is a dead artifact.
- **Forcing single-session template on cohort material** — single file overflows above 5+ sessions. Use the folder convention.
- **Forcing event-debrief structure on a 1-on-1 call** — 1-on-1s are deep on one thread, not broad across many people. Use `one-on-one-debrief.md` instead.
- **Skipping prep mode for "just a coffee chat"** — most low-stakes-feeling conversations are exactly where intent gets fuzzy and signal gets lost. The prep is what catches it.

## Privacy note

Prep and debrief files often contain real names, employers, and specific feedback that other people gave the user. If the file lives in a private repo (most cases), this is fine. If the user plans to publish or share — for example, as a public skill example, blog post, or LinkedIn write-up — anonymize names (single letters or role-only references) and replace company names with category descriptors. The included [`examples/portfolio-critique-workshop.md`](examples/portfolio-critique-workshop.md) demonstrates this anonymization pattern in practice.

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

End every prep run with:

```
## Prep ready

- File created: {path}
- Open questions remaining: {N}
- Recommended re-read: morning-of, before walking in
```

The lists are not optional. They're what keep prep from being aspirational and debriefs from becoming dead files.

## Reference

- [`templates/event-prep.md`](templates/event-prep.md) — multi-person event prep
- [`templates/event-debrief.md`](templates/event-debrief.md) — single-session multi-person debrief
- [`templates/cohort-debrief.md`](templates/cohort-debrief.md) — multi-session folder convention
- [`templates/one-on-one-prep.md`](templates/one-on-one-prep.md) — 1-on-1 conversation prep
- [`templates/one-on-one-debrief.md`](templates/one-on-one-debrief.md) — 1-on-1 conversation debrief
- [`examples/portfolio-critique-workshop.md`](examples/portfolio-critique-workshop.md) — anonymized end-to-end example showing a 2-day workshop debrief with feedback dispositions and propagation
