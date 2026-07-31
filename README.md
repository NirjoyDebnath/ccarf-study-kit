# CCAR-F Study Kit

Three files that turn Claude Code into a tutor for Anthropic's **Claude Certified Architect –
Foundations** exam.

Written against *Exam Guide v1.0, effective July 2026*.

**This is not a question bank.** No leaked items, no answer keys, nothing to memorise. It is the
exam's 30 task statements, a vetted source for every one of them, a fixed 40-unit running order, and
a brief that tells a tutor how to drill judgment rather than definitions — because judgment is what
the exam scores.

## What's in it

| File | What it holds |
|---|---|
| `EXAM-OBJECTIVES.md` | All 30 task statements across 5 domains — §6 of the exam guide, reproduced verbatim |
| `RESOURCES.md` | What to teach each statement from: two or three vetted sources, plus a written tutor prompt |
| `TEACHING-BRIEF.md` | How to teach it, how to write exam-faithful practice items, and the 40-unit running order |

The running order matters more than it sounds. Thirty of the forty units are lessons; the other ten
are the exam guide's own hands-on exercises and scenario drills, gated so you attempt each one only
after every domain it depends on has been taught. Seven of them fall *after* the last lesson — so
finishing the lessons is about four-fifths of the work.

## Start here

```bash
git clone https://github.com/NirjoyDebnath/ccarf-study-kit
cd ccarf-study-kit
npx skills add mattpocock/skills --skill teach -g -a claude-code
```

Then open Claude Code in that directory and run:

```
/teach CCAR-F
```

It will ask why you're taking the exam, write a `MISSION.md`, and start at unit 1. Everything it
generates — lessons, records, notes — stays in your clone and is gitignored.

**Later sessions.** Run `/teach` with no argument to continue where you left off — it reads your
learning records to work out what's next.

**Jumping to a unit.** Name it: `/teach task statement 3.4`, or `/teach plan mode vs direct
execution`. Useful when you already know your weak domain, or when a scenario drill exposes a hole.
The running order is a default, not a rail.

**Steering the session.** Ask for what the exam actually does to you — scenario questions rather than
definitions, and multiple-response items when you want them. The guide says the exam uses them but no
published example exists, so treat the shape of any you're given as an educated guess.

You also need **your own copy of the exam guide PDF**. It's free with a signup at Anthropic's Partner
Academy, and the kit is deliberately built to sit alongside it rather than replace it: the four
preparation exercises and seven shorter activities live in the guide's §8 and §7, and the kit points
at them rather than paraphrasing them.

## Why trust it

Because you can check it. `EXAM-OBJECTIVES.md` reproduces §6 of the exam guide **verbatim** — not
paraphrased, not "based on" — so you can diff it against your own copy. Every source is first-party
and free. Nothing came from a scraped question bank.

`RESOURCES.md` also carries a **Terminology drift** section listing what popular guides currently get
wrong about this exam — the task-statement count, the fee, the item formats, renamed tools. Some of
them are quite popular guides.

<details>
<summary><strong>Using it without the <code>teach</code> skill</strong></summary>

The kit works on its own. `RESOURCES.md` opens with a five-step loop that needs nothing installed:

1. Pick one task statement — your weakest domain, or the running order in `TEACHING-BRIEF.md` §7
2. Paste that statement's tutor prompt plus its two or three most relevant sources into a session
3. Demand scenario questions, not definitions
4. Ask for multiple-response items when you want them
5. Log every miss by **task statement number**, not by topic — that's how you find the hole

You lose the lesson files, the progress records and the spacing that `teach` manages. You keep the
sources, the prompts and the running order, which is most of the value.

</details>

<details>
<summary><strong>What it generates, and why none of it is committed</strong></summary>

`teach` treats your clone as a workspace and writes `MISSION.md`, `NOTES.md`, `lessons/`,
`reference/`, `assets/` and `learning-records/` into it. All of that is gitignored on purpose — it's
one learner's progress, not the kit. Keeping it out means you can `git pull` kit updates into a clone
you've been studying in for a month without conflicts.

The repo also ships a `/highlight` skill. Run it after a lesson and it folds the session's questions
and corrected misconceptions back into that lesson's own HTML file, so re-reading the lesson later
returns the struggle as well as the teaching.

</details>

## Scope, credit, license

The kit covers what the exam guide says is tested and deliberately stops there — `RESOURCES.md` ends
with the guide's §17 out-of-scope list, reproduced in full, because the most expensive mistake in prep
is studying something that cannot be tested.

It contains no practice questions. `TEACHING-BRIEF.md` §4 teaches a tutor to *write* exam-faithful
items instead, which is the part community banks get wrong.

The `teach` skill is [Matt Pocock's](https://github.com/mattpocock/skills), MIT licensed, and is not
part of this repo.

This kit is MIT licensed — see [`LICENSE`](./LICENSE).
