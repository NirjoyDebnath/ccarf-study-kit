# CCAR-F Study Kit

A study kit that turns Claude Code into a tutor for Anthropic's **Claude Certified Architect –
Foundations** exam: all 30 task statements, a vetted source for each, and a fixed 47-unit running
order to work through them in.

**64 of its 67 sources are Anthropic's own documentation** — not blog summaries, not a scraped
question bank. **And the teaching method comes from the exam guide itself**: its sample questions and
answer rationales are what decide how a tutor drills you.

Written against *Exam Guide v1.0, effective July 2026*.

## Running a session

### ① Set it up — once

```bash
git clone https://github.com/NirjoyDebnath/ccarf-study-kit
cd ccarf-study-kit
npx skills add mattpocock/skills --skill teach -g -a claude-code
```

Then open Claude Code in that directory:

```
/teach CCAR-F
```

You get **one question** — how much you've actually built with Claude Code and the Agent SDK — and
then unit 1 starts. Everything generated from here on (lessons, records, notes) stays in your clone
and is gitignored.

> **Every lesson ends with questions.** Not definitions — scenario items written in the exam's own
> shape: a production symptom, four defensible options, and a rationale for each. You're graded on the
> *reason* you picked, not the label, and every miss is logged against its task statement so the holes
> are findable later.

### ② Come back — every session after

```
/teach
```

No arguments, no questions. It reads `lessons/`, finds the first unit you haven't done, and teaches
it. It won't ask how the last one went — a lesson file on disk *is* the answer.

### ③ Jump to any unit — whenever you want

```
/teach task statement 3.4
/teach plan mode vs direct execution
```

The running order is a default, not a rail. Jump when you already know your weak domain, or when a
scenario drill exposes a hole.

### ④ Keep what you asked — after a lesson

```
/highlight
```

A lesson records what was *taught*; it can't record what you *asked*. `/highlight` mines the session
for your questions, the beliefs that turned out to be wrong, and the analogy that finally made it
land — then writes them into that lesson's own file, so re-reading it later returns the struggle as
well as the teaching. Re-run it and the block is merged, not duplicated.

Worth doing on this exam in particular: a boundary you personally found confusing is a preview of a
distractor you'll meet again.

## What's in it

| File | What it holds |
|---|---|
| `EXAM-OBJECTIVES.md` | All 30 task statements across 5 domains — §6 of the exam guide, reproduced verbatim |
| `RESOURCES.md` | What to teach each statement from: two or three vetted sources, plus a written tutor prompt |
| `TEACHING-BRIEF.md` | How to teach it, how to write exam-faithful practice items, and the 47-unit running order |
| `exam/` | The exam guide PDF itself — v1.0, July 2026 |

The running order matters more than it sounds. Thirty of the forty-seven units are lessons; the other
seventeen are the exam guide's own hands-on exercises, activities and scenario drills, placed so you
attempt each one only after the domains it depends on have been taught. Nine of them fall *after* the
last lesson — so finishing the lessons is under two-thirds of the course.

<details>
<summary><strong>How it's built</strong></summary>

**1 · Sources collected.** 67 unique sources, cited 174 times across the 30 statements — **64 of the
67 are official Anthropic documentation**: 31 Claude Code docs, 17 platform docs, 5 engineering posts,
4 Academy courses, 3 claude.com pages, 2 Anthropic GitHub tutorials, 2 MCP spec pages. The other 3 are
write-ups by people who sat the exam. Nothing scraped, everything free. They're filed *per task
statement*, each entry saying what that page teaches for that statement — so you read two or three
pages per unit, not a reading list.

**2 · Teaching tone set to what the exam demands.** The exam scores judgment in production scenarios,
not definitions. `TEACHING-BRIEF.md` encodes that from the exam guide's own sample questions and
answer rationales, read alongside the sources: teach the mechanism beneath a statement, grade on the
reason not the label, write items in the exam's shape, gate the hands-on work until its domains are
taught.

**3 · Objectives kept verbatim.** `EXAM-OBJECTIVES.md` is §6 of the guide, word for word — every task
statement with the knowledge and skills measured against it. Exam items are written against those 30
statements, so a paraphrase would silently edit the syllabus.

**4 · `teach` turns all three into lessons.** Objectives for scope, resources for grounding, brief for
method — one HTML lesson per unit, with questions at the end. The kit supplies the material; `teach`
supplies the session.

</details>

<details>
<summary><strong>Limitations</strong></summary>

**A lesson takes 5–10 minutes to generate** before you can start reading it — it's written for you,
from sources fetched in that session, not served from disk. The first one runs longer still, because
it also builds the shared stylesheet and quiz widget that every later lesson reuses.

**No practice questions ship with it.** Claude writes them during the session. Their structure follows
the exam guide's own sample questions and rationales, plus the format conventions of an existing mock
paper — but the content is written fresh each time and has no vetted answer key. Good for drilling
judgment; not a fixed mock exam.

**Multiple-response items are inferred.** The guide says the exam uses them; no example has ever been
published, so any you're given is a guess at the shape.

**Lessons are generated, not shipped.** Nothing in `lessons/` is a reviewed artefact — two people
working through this kit get two different courses.

</details>

<details>
<summary><strong>Scope, credit, license</strong></summary>

The kit covers what the exam guide says is tested and deliberately stops there — `RESOURCES.md` ends
with the guide's §17 out-of-scope list, reproduced in full, because the most expensive mistake in prep
is studying something that cannot be tested.

It contains no practice questions. `TEACHING-BRIEF.md` §4 teaches a tutor to *write* exam-faithful
items instead, which is the part community banks get wrong.

The `teach` skill is [Matt Pocock's](https://github.com/mattpocock/skills), MIT licensed, and is not
part of this repo.

This kit is MIT licensed — see [`LICENSE`](./LICENSE). That covers the writing in this repo only. The
PDF in `exam/` is Anthropic's *Claude Certified Architect – Foundations Exam Guide*, v1.0 (July 2026),
included for convenience and distributed free through the Anthropic Partner Academy — it remains
Anthropic's work and the MIT grant does not extend to it.

</details>
