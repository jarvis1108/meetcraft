# Multi-Session Event / Cohort — Folder Convention

> Use this folder structure for events spanning multiple sessions across weeks (cohorts, multi-day conferences, recurring meetups where you attend regularly). Single-session template ([`event-debrief.md`](event-debrief.md)) doesn't scale — too much material per session, hard to find anything.

---

## When to use this pattern

- 5+ sessions across weeks (e.g., a 5-week design cohort)
- Multi-day conferences with significant material per day (e.g., a 2-day workshop with 30+ contacts and 7 transcripts)
- Recurring meetups where you attend regularly enough to want a single index

---

## Folder structure

```
{your-events-dir}/{YYYY-MM}-{slug}/
├── index.md                  # Top-level summary, links to per-session files, current status
├── takeaways.md              # Cross-cutting lessons (living doc — append as cohort progresses, don't rewrite)
├── participants.md           # All people met across all sessions, current relationship status (scoped to this cohort)
├── per-week/                 # OR per-day/ for multi-day single-event
│   ├── week-1.md             # One file per session — uses event-debrief.md structure inline
│   ├── week-2.md
│   └── ...
├── archive/
│   ├── transcripts/          # Raw + cleaned audio transcripts
│   └── (any historical materials, e.g. day1-feedback.md if you split mid-stream)
└── artifacts/                # Anything you brought (slides, demos, screenshots from cohort tools)
```

> Slug naming: prefer descriptive (`design-cohort-q2`, `portfolio-marathon`) over date-only. Date prefix gives chronological sort in your events listing.

---

## File templates

### `index.md` (folder root)

```markdown
# {Cohort / Event Name}

> {1-line summary} · {start date} → {end date or "ongoing"} · {N} sessions · {format}

## Quick links
- [Cross-cutting takeaways](takeaways.md)
- [Participants](participants.md)
- Per-session notes: [Week 1](per-week/week-1.md) · [Week 2](per-week/week-2.md) · ...

## Status
- **Sessions completed**: {N}/{total}
- **Next session**: {date} — {topic if known}
- **Active follow-ups**: {brief list, link to backlog if filed}

## Why I'm here
{1 paragraph — what made you join, what you're hoping to get out of it. Refresh occasionally so you stay honest about whether it's delivering.}
```

### `per-week/week-N.md`

Use [`event-debrief.md`](event-debrief.md) structure inside this file — same sections (Top takeaways, People met, Sessions, Feedback, Follow-ups, Transcript note), but scoped to this single session. The cross-cutting `takeaways.md` and `participants.md` at the folder root aggregate across all weeks.

### `takeaways.md`

```markdown
# {Cohort} — Cross-Cutting Takeaways

> Living doc. Append new insights at the bottom as cohort progresses. Don't rewrite history — past entries stay even if your view shifts.

## {YYYY-MM-DD} (Week 1)
- Insight 1
- Insight 2

## {YYYY-MM-DD} (Week 2)
- Insight ...

## Themes (write at end of cohort)
> After all sessions done, identify 3-5 cross-cutting themes that span multiple weeks. These are the durable lessons worth remembering in 6 months.

1.
2.
3.
```

### `participants.md`

```markdown
# {Cohort} — Participants

> All people met across the cohort. Scoped to this event. Promote good participants to your cross-event contact database at end of cohort with relationship source noted.

| Name | Role / Company | Met in | Notes | Status |
|------|----------------|--------|-------|--------|
|      |                | Wk 1 / Wk 3 etc | brief context | active / follow-up sent / migrated to contact db / pass |
```

---

## Workflow per session

1. **Pre-session**: Pre-event prep — strategy, attendee research, conversation playbook (use whatever pre-event template fits your workflow)
2. **During**: Take notes, capture transcripts if applicable
3. **Post-session (within 24h)**:
   - Create `per-week/week-N.md` (use event-debrief.md structure)
   - Append cross-cutting insights to `takeaways.md`
   - Update `participants.md` with new contacts
   - File any spawned actions in your backlog
4. **End of cohort**:
   - Migrate top participants to your contact database with relationship source
   - Identify cross-cutting themes in `takeaways.md`
   - Optional: write a short retrospective in `index.md` "Status" section

---

## Reference example

See [`examples/portfolio-critique-workshop.md`](../examples/portfolio-critique-workshop.md) for an anonymized 2-day workshop debrief showing this folder convention applied — including transcript pre-processing notes, feedback dispositions, and propagation actions.
