# meetcraft

> Some skills mine the moment. This one crafts the conversation — before and after.

A Claude Code skill for **prepping** before professional conversations and **sedimenting** what came out after — for events (meetups, workshops, conferences, cohort sessions) and 1-on-1s (informational interviews, coffee chats, mentor catch-ups).

**Input**: an upcoming conversation, or rough notes / a transcript from one that just happened.
**Output**: a structured markdown file you can re-read in 3 months and still recognize what you set out to do — or what actually happened — plus an explicit propagation list (new contacts → your contact db, adopt-feedback → your backlog, etc.) so the captured signal doesn't die in a markdown file.

---

## What it does

The skill loads four templates and a workflow that picks one based on situation:

**Prep templates** (intent + context before walking in)
- **`templates/event-prep.md`** — multi-person events (meetups, workshops, conferences, cohort sessions)
- **`templates/one-on-one-prep.md`** — 1-on-1 conversations (coffee chats, informational interviews, mentor catch-ups, intro calls)

**Debrief templates** (signal + propagation after)
- **`templates/event-debrief.md`** — single-session multi-person events (meetups, workshops, evening mixers)
- **`templates/cohort-debrief.md`** — folder convention for multi-week cohorts and multi-day conferences (5+ sessions, where a single file overflows)
- **`templates/one-on-one-debrief.md`** — 1-on-1 conversations (deep on one thread, not broad across many people)

**Each prep template enforces:**
- a **single primary intent** (relationship / intel / ask / offer) — mixed intents need explicit ranking
- **named priority contacts** with "why they matter" justification
- **specific lines of inquiry** instead of fishing prompts
- a **disposition** for the room before walking in

**Each debrief template enforces:**
- 3-5 cross-cutting **Top Takeaways** (capped to force prioritization, not a session-by-session log)
- a **People Met** table with explicit follow-up status per person (debrief mode)
- chronological **Sessions / Conversations** or **Topic Threads** blocks
- a **Feedback Received** table with required **disposition** per row (`adopt` / `consider` / `pass`)
- explicit **Follow-up Actions** as checkboxes
- a **transcript pre-processing** reminder (Krisp for speaker diarization, Whisper for word accuracy, never analyze raw transcripts)

The skill enforces a **propagation list at the end of every run** — naming the contacts, backlog items, and workflow insights that need to leave the file to live in their long-term homes. Without that step, the prep is aspirational and the debrief is a dead artifact.

---

## Why this exists

Most networking memory dies within 24 hours. The 7th conversation overwrites the 3rd. The piece of feedback that would have changed your approach gets remembered as "someone said something about my landing page" instead of the specific actionable critique.

And most underprepared conversations leak the value they could have created. "Just a coffee chat" walks in unranked, drifts through three topics, leaves no follow-up bridge. Two weeks later the relationship goes dormant.

Prep in the morning + debrief in the next 24 hours, with structured templates, catches both ends. Free-form attempts usually don't — important pieces fall into the cracks between sections you didn't think to write.

This skill is the templates + workflow extracted from real use across in-person workshops (~30 contacts, 7 transcripts) and recurring multi-week cohorts. See [`examples/portfolio-critique-workshop.md`](examples/portfolio-critique-workshop.md) for an anonymized end-to-end example of the debrief side.

---

## Install

This is a [Claude Code](https://docs.claude.com/en/docs/claude-code) skill. To install:

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/jarvis1108/meetcraft.git ~/.claude/skills/meetcraft
```

Or, for a project-scoped install:

```bash
git clone https://github.com/jarvis1108/meetcraft.git .claude/skills/meetcraft
```

Restart Claude Code (or open a new session). The skill will auto-trigger when you mention an upcoming or just-finished conversation, paste a transcript, or invoke `/meetcraft` directly.

You can also use the templates directly without Claude — they are plain markdown.

---

## When the skill auto-triggers

Claude Code loads this skill when it detects:

**Prep signals**
- "I have a {meetup / workshop / conference / coffee chat / informational interview} coming up"
- "I'm meeting {person} tomorrow / next week"
- "Help me prep for {event}" or "get me ready for {call}"
- An upcoming cohort session mention with planning intent

**Debrief signals**
- "I just got back from {meetup, workshop, conference, mixer, cohort session}"
- A pasted Krisp / Whisper / Otter transcript with a request to synthesize
- "let's debrief {event}" or "process the {event} transcript"
- A cohort-week wrap mention
- 1-on-1 networking call notes pasted with a request to capture them

You can also invoke manually: `/meetcraft` or just paste materials and ask for prep / debrief.

---

## Routing — which template

The skill picks based on situation:

| Situation | Template | Mode |
|-----------|----------|------|
| Multi-person event coming up | `templates/event-prep.md` | Prep |
| Multi-person event just happened, ≤4 hrs | `templates/event-debrief.md` | Debrief |
| Multi-day or multi-week cohort, 5+ sessions | `templates/cohort-debrief.md` (folder convention) | Debrief |
| 1-on-1 coming up (coffee chat / informational interview / catch-up) | `templates/one-on-one-prep.md` | Prep |
| 1-on-1 just happened | `templates/one-on-one-debrief.md` | Debrief |

If unclear, the skill asks before writing. Wrong choice forces a restructure later.

---

## File structure

```
meetcraft/
├── SKILL.md                    # The skill definition (loaded by Claude Code)
├── README.md                   # This file
├── LICENSE                     # MIT
├── templates/
│   ├── event-prep.md           # Multi-person event prep
│   ├── event-debrief.md        # Single-session multi-person debrief
│   ├── cohort-debrief.md       # Multi-session folder convention
│   ├── one-on-one-prep.md      # 1-on-1 conversation prep
│   └── one-on-one-debrief.md   # 1-on-1 conversation debrief
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
| 1-on-1 sediment | cross-company contact tracker (long-term relationship trajectory) |

If you don't yet have downstream homes, the skill will suggest creating them rather than silently swallowing the signals.

---

## Anti-patterns the skill is designed to prevent

- Skipping transcript pre-processing → speaker-attribution errors compound through every section
- Mixing session notes and feedback → dispositions get lost
- Rewriting the cohort `takeaways.md` mid-cohort → trail of evolving understanding is the point
- Closing a debrief without an explicit propagation list → captured signal dies in the file
- Forcing the single-session template on cohort material → file overflows above 5+ sessions
- Forcing event-debrief structure on a 1-on-1 → 1-on-1s are deep on one thread, not broad
- Skipping prep mode for "just a coffee chat" → low-stakes-feeling conversations are exactly where intent gets fuzzy

The skill flags these in real time and prompts a fix.

---

## Provenance

Extracted from a designer's job-search workflow where conversation capture started failing at scale (5+ networking events in a quarter, 3 multi-week cohorts, recurring 1-on-1s). Named contributions and quotes were getting attributed to the wrong people; feedback items were being captured but never reaching the backlog; coffee chats were drifting without follow-up bridges. The templates and workflow rules in this skill are what stopped that.

If your conversations are not currently leaking signal, you don't need this skill. If they are, the time to install was yesterday.

---

## License

MIT — see [LICENSE](LICENSE).

---

> *Repo aesthetic owes a debt to long evenings of Minecraft and even longer evenings of professional conversations.*
