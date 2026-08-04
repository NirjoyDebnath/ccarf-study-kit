---
name: quiz
description: >-
  Use when the learner wants to be tested on CCAR-F material they have already
  completed — any request like "quiz me", "test me on what I've covered",
  "practice questions", "practice exam", "mock exam", or /quiz (optionally with
  a count and the words "exam" or "multi", e.g. /quiz 10 exam). Also use when
  the learner pastes a "CCAR-F QUIZ RESULTS" block back into the session — that
  paste is this skill's scoring step, even hours later. Not for the in-lesson
  quizzes the teach flow builds, and never a reason to write into lessons/.
argument-hint: "[count] [practice|exam] [multi]"
---

# Quiz — fresh practice exams from completed lessons only

## Why this exists

Lesson quizzes drill one statement at a time, and their scores never reach the
tutor (`NOTES.md` has logged that gap since lesson 1). `/quiz` builds a
cross-statement quiz as an HTML page in `practice/`, in the exam's own style,
then folds the learner's results back into `learning-records/` — so the next
quiz, and the next teach session, can aim at real weaknesses. Items are
generated fresh every run: repeating a seen bank measures memory, not judgment
(`TEACHING-BRIEF.md` §8).

**The quiz is a page, not a chat transcript.** Don't run it as questions in the
conversation, and don't print items, answers, or statement tags into chat —
the page handles delivery, grading, and tag reveal. Chat is for the setup
message and, later, scoring the pasted results.

## Read before generating — every run, never from memory

1. `lessons/` listing → what's completed (next section).
2. Those statements' bullets in `EXAM-OBJECTIVES.md` — the scope boundary and floor.
3. `TEACHING-BRIEF.md` §1 (format, weights, the six scenario worlds), §2 (what
   the exam rewards), §4 (**how to write an item — the rulebook; this file only
   adds quiz-level rules on top**).
4. `RESOURCES.md` → **Terminology drift** and **Out of scope — do not study**.
   An item contradicting either is discarded, not patched.
5. `practice/.quiz-index.json` — the learner-history cache: seen stems (from
   lesson `window.QUIZ` arrays and prior practice pages), `highlight`-block
   misconceptions (the best distractor material), and logged misses by
   statement. Bring it current per **The quiz index** below; fully read only
   files it hasn't folded in yet.

Steps 1–4 are kit facts and are never cached — read those files fresh every
run. The index only spares re-reading *derived* learner history from HTML.

## The quiz index — flat-cost reads

`practice/.quiz-index.json` caches what the expensive re-reads exist to
extract, so a run's reading cost stays flat as `lessons/` and `practice/`
grow. Shape: `built_from` (every lesson, practice, and learning-record file
already folded in), `seen_stems`, `misconceptions`, and `misses`, the last
three keyed by statement number.

Bringing it current, every run:

1. Read the index; list `lessons/`, `practice/`, `learning-records/`; diff
   the listings against `built_from`.
2. Fully read only the files not yet folded in and merge their stems,
   highlights, and misses.
3. Index missing or unparseable → rebuild from a full scan. It is derived
   data; deleting it is always safe and never loses anything.

Write the updated index — including the new quiz's own stems — in the same
step that writes the quiz page. Scoring never touches the index; a scoring
run's learning-record is folded in by the next run's diff. Never cache kit
facts (format, weights, worlds, drift, out-of-scope) in it — those stay owned
by their files.

## Coverage: lessons/ decides

A file `lessons/NNNN-*.html` means that lesson is done (`TEACHING-BRIEF.md` §3
rule 6). **Each lesson declares its own statement in its kicker line**
("… · statement **N.M**") — read coverage from that line, one cheap pass:
`grep -o 'statement <strong>[0-9]\.[0-9]' lessons/*.html`. Never derive the
statement from a file's ordinal position in the running order: lesson numbers
follow the order lessons were *taken*, and learners jump — as of 2026-08-05,
lesson 0005 is statement 2.1 while units 5–8 (1.5, 1.6, 1.7 and an activity)
remain untaught. A filename-position mapping would mark 1.5 covered and 2.1
missing, quizzing exactly the wrong material. Only lessons produce files —
activities and drills don't, and don't affect coverage.

- **Never test material no completed lesson taught.** The one nuance: if a
  completed lesson explicitly taught something that previews a later statement
  (e.g. lesson 0003's enforcement blast-radius section), it is fair game — it
  was taught — but say so honestly **inside its `tag`** ("… · previews 1.4 ·
  taught in lesson 0003"), which stays hidden until grading. Never in the
  mid-quiz `note` field: that renders during the quiz and would telegraph the
  statement.
- State the detected coverage inside the setup message; the learner corrects it
  in their reply if it's wrong. Don't ask for confirmation.
- The learner may narrow a quiz to a subset of completed statements — their
  call. If they name an uncompleted section, say it has no lesson yet and offer
  the lesson instead; never quiz it.
- `lessons/` empty or unmappable → ask which sections are finished. That is the
  only time coverage is asked.

## One setup message, then build

State coverage, then ask — one message, not several: **how many items**
(5/10/15, any number; required — don't generate without it), **multi-response
items?** (default no), **practice or exam mode?** (default practice; exam =
no feedback until submit, timed at 2 min × count). Arguments pre-fill answers:
`/quiz 10 exam multi` leaves nothing to ask — but the setup message is still
sent, question-free, so the learner sees the coverage statement and what's
about to be built. Anything unaddressed in the reply takes its default.

## Writing the items — §4 plus these quiz rules

- **Every item lives in one of the six official scenario worlds** in
  `TEACHING-BRIEF.md` §1 (guide §5) — pick a world whose domain mix fits the
  statement. The world's name goes **only in the hidden `tag`**, never in the
  visible `scenario` or `stem` text (learner rule, 2026-08-05): a label on the
  question clutters the stem and telegraphs the domain mix. Never invent a
  scenario world; community sets add nonexistent ones ("Conversational AI
  Architecture Patterns" is not real).
- **Distribution:** §1's blueprint weights renormalized over covered domains —
  not recency, not gut feel. Within a domain, spread evenly across its covered
  statements. Then, when records exist, aim roughly a third of items at
  statements with logged misses. Cross-statement items are welcome; tag every
  statement they draw on.
- **No stem the learner has already faced**, in any lesson or practice page.
- **Multi-response only if opted in:** about 1 in 5 items; state the exact
  count in the stem ("Select TWO" of 5 options, occasionally THREE of 5–6) —
  never "select all that apply", since the guide says each item states how
  many; all-or-nothing scoring; each carries the kit's inferred-shape caveat
  in its `note` field.
- Each item's data carries `tag` ("Domain N · N.M · <scenario world> ·
  judgment: …"), `ref` (statement numbers), and `heuristic` (the one-line
  deciding instinct, per §8's honest test). The widget keeps all three hidden
  until the end — the tag is where the scenario world is named.
- It's **CCAR-F**, guide numbering: D2 = Tool/MCP 18%, D3 = Claude Code 20%,
  D4 = Prompt 20%. Any source ordering domains by weight is wrong.

## Big quizzes: one subagent per domain

Inline generation stays the default up to 10 items and 2 covered domains.
Beyond either, fan out one subagent per covered domain, in parallel:

- Compute each domain's item count first — §1 blueprint weights renormalized,
  the distribution rule above.
- Each agent's prompt is a trimmed brief, not the corpus: its domain's
  statement bullets verbatim from `EXAM-OBJECTIVES.md`, the §4 rulebook and
  six scenario worlds (with the rule that the world's name goes in `tag` only,
  never in the visible `scenario`/`stem`), the relevant drift/out-of-scope
  entries, and — from the index — that domain's seen stems, misconceptions,
  and misses. Agents read no files, write no files, and return items as JSON.
- The coordinator dedupes stems across agents and against the index, discards
  (never patches) items violating drift or out-of-scope, regenerates
  shortfalls inline, and assembles the page. Returned items are never echoed
  into chat.

## Building the page

Write `practice/quiz-NNNN-YYYY-MM-DD.html` (create `practice/` if missing;
NNNN = next number in that directory). **Never write into `lessons/`** — a
file there marks a unit complete and corrupts every future session's resume.

Page skeleton — same bones as a lesson quiz:

```html
<!doctype html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>CCAR-F practice quiz NNNN — 1.1–1.3</title>
<link rel="stylesheet" href="../assets/course.css"></head>
<body>
<h1>Practice quiz NNNN</h1>
<p class="subtitle">Covers 1.1–1.3 · 10 items · exam mode</p>
<div id="quiz"></div>
<script>
window.QUIZ_PAGE = { mode: "exam", minutesPerItem: 2 };  // or mode: "practice"
window.QUIZ = [ /* items: tag, ref, heuristic, scenario, stem, select, note, options */ ];
</script>
<script src="../assets/quiz.js"></script>
</body></html>
```

`assets/quiz.js` must support `QUIZ_PAGE` (check: `grep QUIZ_PAGE
assets/quiz.js`). If this clone's copy predates it, upgrade the widget to this
contract before generating, keeping legacy lesson behavior untouched when the
flag is absent: under `QUIZ_PAGE`, hide per-item tags entirely, letter the
options, and on completion render a copyable "CCAR-F QUIZ RESULTS" block (one
line per item: ref, hit/miss/unanswered, picked vs correct letters, tag +
heuristic for non-hits); in exam mode open as a start gate (item count and
time budget shown, items hidden, clock stopped until Start is pressed), keep
answers changeable, grade only on Submit (confirm once if items are
unanswered — they score as misses), and run a countdown of minutesPerItem ×
items that auto-submits at zero.

Then tell the learner: the file path to open, and to paste the results block
back here when done.

## Scoring the paste-back

When a results block arrives (this session or a later one), reply with: the
score; per-domain and per-statement lines; a revisit list, worst first, each
miss paired with its deciding heuristic; and §8's caveat — a practice
percentage is not the exam's scaled 720, and passers report wanting ~90%
before booking. Then write `learning-records/NNNN-quiz-YYYY-MM-DD.md` (next
NNNN there): mode, item count, score, statements covered, misses by statement
number with the judgment and heuristic, unanswered items, and the practice
page path. Outcomes only — never store items for reuse.

No paste by the time the conversation moves on → remind once, then let it go.

## Never

- Never deliver the quiz in chat, print tags/answers before the page is done,
  or re-serve a seen stem.
- Never put a scenario world's name in an item's visible `scenario` or `stem`
  text — it lives in the hidden `tag` only, revealed with the grading.
- Never write to `lessons/`, `NOTES.md`, or any kit file
  (`EXAM-OBJECTIVES.md`, `RESOURCES.md`, `TEACHING-BRIEF.md`, `CLAUDE.md`).
  This skill writes to `practice/` and `learning-records/` only.
- Never fetch URLs, consult question banks, or take exam facts from memory —
  the kit files are the only source, read fresh each run.

## Quality bar

Opened cold, an item should be indistinguishable in shape from the exam
guide's own samples: a scenario world chosen from the six (named in the hidden
`tag`, invisible until grading), a situation with a number, a stem carrying one
decision, four defensible balanced options, a rationale for every option naming
the real-world failure — and nothing anywhere on the page that tells the
learner which statement or world it tests until the quiz is over.
