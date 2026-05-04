# event-debrief-skill

A Claude Code skill for sedimenting what happened at an event — meetup, workshop, conference, cohort session, or 1-on-1 networking call — before the 24-hour memory cliff hits.

Input: rough notes or a transcript.
Output: a structured debrief that you can re-read in 3 months and still recognize what happened, plus an explicit propagation list (new contacts → your contact db, adopt-feedback → your backlog, etc.) so the captured signal doesn't die in a markdown file.

---

## What it does

The skill loads two opinionated templates and a workflow that decides which one applies:

- **`templates/event-debrief.md`** — single-session events (meetups, workshops, 1-on-1 calls, evening mixers)
- **`templates/cohort-debrief.md`** — folder convention for multi-week cohorts and multi-day conferences (5+ sessions, where a single file overflows)

Each template enforces:

- 3-5 cross-cutting **Top Takeaways** (capped to force prioritization, not a session-by-session log)
- a **People Met** table with explicit follow-up status per person
- chronological **Sessions / Conversations** blocks
- a **Feedback Received** table with a required **disposition** per row (`adopt` / `consider` / `pass`)
- explicit **Follow-up Actions** as checkboxes
- a **transcript pre-processing** reminder (Krisp for speaker diarization, Whisper for word accuracy, never analyze raw transcripts)

The skill also enforces a **propagation list at the end of every debrief** — naming the contacts, backlog items, and workflow insights that need to leave the debrief file to live in their long-term homes. Without that step the debrief is a dead artifact.

---

## Why this exists

Most networking memory dies within 24 hours. The 7th conversation overwrites the 3rd. The piece of feedback that would have changed your approach gets remembered as "someone said something about my landing page" instead of the specific actionable critique.

A debrief in the next 24 hours, with a structured template, catches the signal. A free-form debrief usually doesn't — important pieces fall into the cracks between sections you didn't think to write.

This skill is the templates + workflow extracted from real use across an in-person 2-day workshop (~30 contacts, 7 transcripts) and recurring cohort sessions. See [`examples/portfolio-critique-workshop.md`](examples/portfolio-critique-workshop.md) for an anonymized end-to-end example.

---

## Install

This is a [Claude Code](https://docs.claude.com/en/docs/claude-code) skill. To install:

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/jarvis1108/event-debrief-skill.git ~/.claude/skills/event-debrief
```

Or, for a project-scoped install:

```bash
git clone https://github.com/jarvis1108/event-debrief-skill.git .claude/skills/event-debrief
```

Restart Claude Code (or open a new session). The skill will auto-trigger when you mention an event, paste a transcript, or invoke `/event-debrief` directly.

You can also use the templates directly without Claude — they are plain markdown.

---

## When the skill auto-triggers

Claude Code loads this skill when it detects:

- "I just got back from {meetup, workshop, conference, mixer, cohort session}"
- A pasted Krisp / Whisper / Otter transcript with a request to synthesize
- "let's debrief {event}" or "process the {event} transcript"
- A cohort-week wrap mention
- 1-on-1 networking call notes pasted with a request to capture them

You can also invoke manually: `/event-debrief` or just paste materials and ask for a debrief.

---

## Decision: which template

The skill picks the template based on signal:

| Signal | Template |
|--------|----------|
| Single session, ≤4 hours, one location/day | `templates/event-debrief.md` |
| Multi-day or multi-week (cohort, conference, recurring weekly meetup), 5+ sessions, significant material per session | `templates/cohort-debrief.md` (folder convention) |
| 1-on-1 networking call (informational chat, coffee, intro call) | `templates/event-debrief.md`, scoped to that one conversation |

If unclear, the skill asks before writing. Wrong choice forces a restructure later.

---

## File structure

```
event-debrief-skill/
├── SKILL.md                  # The skill definition (loaded by Claude Code)
├── README.md                 # This file
├── LICENSE                   # MIT
├── templates/
│   ├── event-debrief.md      # Single-session template
│   └── cohort-debrief.md     # Multi-session folder convention
└── examples/
    └── portfolio-critique-workshop.md  # Anonymized end-to-end example
```

---

## Adapting to your system

The templates reference generic homes for downstream propagation — "your backlog", "your contact database", "your workflow insights / friction log". The skill output explicitly names propagation actions but does not assume what your downstream systems are.

Common downstream homes (substitute as appropriate):

| Captured | Possible downstream |
|----------|---------------------|
| New contacts | `connections.md`, Notion CRM, Airtable, Google Contacts |
| Adopt-feedback | `backlog.md`, Linear, GitHub Issues, Notion task list |
| Consider-feedback | `friction-log.md`, a workflow-insights file, weekly retro doc |
| Adopt action items | calendar reminder, follow-up email, task tracker |

If you don't yet have downstream homes, the skill will suggest creating them rather than silently swallowing the signals.

---

## Anti-patterns the skill is designed to prevent

- Skipping transcript pre-processing → speaker-attribution errors compound through every section
- Mixing session notes and feedback → dispositions get lost
- Rewriting the cohort `takeaways.md` mid-cohort → trail of evolving understanding is the point
- Closing a debrief without an explicit propagation list → captured signal dies in the file
- Forcing the single-session template on cohort material → file overflows above 5+ sessions

The skill flags these in real time and prompts a fix.

---

## Provenance

Extracted from a designer's job-search workflow where event capture started failing at scale (5+ networking events in a quarter, 3 multi-week cohorts). Named contributions and quotes were getting attributed to the wrong people; feedback items were being captured but never reaching the backlog. The templates and workflow rules in this skill are what stopped that.

If your debriefs are not currently leaking signal, you don't need this skill. If they are, the time to install was yesterday.

---

## License

MIT — see [LICENSE](LICENSE).
