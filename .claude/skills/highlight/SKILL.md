---
name: highlight
description: >-
  Capture the tutor conversation about a finished CCAR-F lesson and write it into
  the end of that lesson's own HTML file, so the questions asked and misconceptions
  corrected are preserved alongside the teaching instead of being lost with the chat.
  Use this whenever the user invokes /highlight, or says anything like "capture what
  I asked", "save this conversation into the lesson", "record the clarifications",
  "add my questions to the lesson", "don't lose this discussion", or otherwise wants
  the back-and-forth from a study session kept. Trigger even if they don't say
  "highlight" — any request to preserve the Q&A from a completed lesson is this skill.
  Edits the lesson file in place; never creates a new file.
---

# Highlight — fold the conversation back into the lesson

## Why this exists

A lesson HTML file records what was *taught*. It cannot record what was *asked*.

The most valuable part of a study session is usually not in the lesson at all: the
question the learner asked because something didn't fit, the belief they held that
turned out to be wrong, the analogy that finally made it land. On this exam that
matters more than usual — items are built from exactly those confusable boundaries,
so a misconception the learner already hit and corrected is a preview of a distractor
they will meet again.

When the session ends, all of that disappears. `/highlight` writes it into the lesson
itself, so re-reading the lesson later returns the whole picture — the teaching and
the struggle.

**This skill edits an existing lesson file. It never creates a new one.** Revision
digests, key-point summaries and recall quizzes are deliberately not part of it; the
lesson already teaches, and a separate digest file is one more thing to keep in sync.

## Locating things

Run from a CCAR-F study workspace — the directory containing `lessons/`. Resolve it
with Glob for `lessons/*.html` if the working directory is ambiguous.

- Lessons: `<workspace>/lessons/NNNN-slug.html` ← you edit these in place
- Stylesheet: `<workspace>/assets/course.css` (already linked from each lesson)

## Steps

### 1. Pick the lesson

If the user passed an argument — a number like `2`, or a slug or title — use it.
Otherwise use the lesson this conversation has been about. If genuinely ambiguous,
list `lessons/*.html` and ask rather than guessing; writing into the wrong lesson is
worse than one extra question.

### 2. Mine the conversation

Read the lesson HTML first, so you can tell what is already taught there. Then go
through the conversation and collect only what the lesson does **not** already say:

- **Questions the learner asked** — in their own framing, not rewritten into textbook
  phrasing. The way someone asks a question is a record of how they were thinking.
- **Misconceptions that surfaced and were corrected** — state the wrong belief *and*
  the correction. The contrast is the thing that sticks; a bare correction loses it.
- **Clarifications and analogies that landed** — whatever actually made it click.
- **Facts or citations pulled in during the chat** that aren't in the lesson.

If the learner corrected *you*, record that too. It is still what they now understand.

Skip anything that merely restates the lesson. An empty section is more honest than a
padded one — if the conversation was all teaching and no back-and-forth, say so in one
line and stop.

### 3. Write it into the lesson

Append the block below just before the closing `</div>` of the lesson's `.wrap`
container, after the lesson's own content.

The marker comments matter: **on a re-run, find `<!-- highlight:start -->` and
`<!-- highlight:end -->` and replace everything between them** with a merged version
carrying both the earlier entries and the new ones. Appending a second block instead
would leave the learner with duplicate, drifting records of the same lesson.

Use the classes already defined in `course.css` (`callout`, `callout.key`, `label`)
so the section reads as part of the lesson rather than bolted on.

```html
<!-- highlight:start -->
<h2>What you asked</h2>
<p class="subtitle">Captured from the tutor session on {{date}}. Not part of the original
lesson — this is what came up while working through it.</p>

<div class="callout key">
  <span class="label">Corrected along the way</span>
  <ul>
    <li><strong>{{topic}}:</strong> you first thought {{the wrong belief, stated plainly}} —
        actually {{the correction}}. {{the hook or analogy that made it land, if there was one}}</li>
  </ul>
</div>

<h3>Questions raised</h3>
<ul>
  <li><strong>{{the question, in the learner's framing}}</strong> — {{the answer, one or two sentences}}</li>
</ul>

<h3>Picked up in conversation</h3>
<ul>
  <li>{{fact, citation or detail that came from the chat rather than the lesson}}</li>
</ul>
<!-- highlight:end -->
```

Drop any of the three headings that have nothing real in them. A single honest
subsection beats three padded ones.

### 4. Confirm

Tell the user which lesson file you edited and, in one line, what was captured. Don't
open the file unless they ask — they were just reading it.

## Quality bar

The test is whether the learner, reading this lesson in three weeks with no memory of
the session, recovers not just the content but *why they found it hard*. Record the
misconception, not a tidied-up version of it. "You thought a subagent ignored the
findings because its context window was smaller — actually they were never put in its
prompt" is worth keeping. "Subagents have isolated context" is already in the lesson.
