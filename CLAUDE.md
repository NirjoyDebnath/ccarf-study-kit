# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A study kit for the **Claude Certified Architect – Foundations (CCAR-F)** exam. It is documentation
only — there is no source code, no package manifest, and no build, test, or lint command. Don't go
looking for one.

The repo doubles as a teaching workspace for the
[`teach`](https://github.com/mattpocock/skills/blob/main/skills/productivity/teach/SKILL.md) skill
— Matt Pocock's, MIT licensed, not part of this repo. `/teach` treats this directory as the
workspace root and generates `MISSION.md`, `NOTES.md`, `lessons/`, `reference/`, `assets/`, and
`learning-records/` here.

## The three kit files

| File | Owns |
|---|---|
| `EXAM-OBJECTIVES.md` | What is tested — all 30 task statements across 5 domains |
| `RESOURCES.md` | What to teach from — sources and a tutor prompt per statement |
| `TEACHING-BRIEF.md` | How to teach, how to write items, exam format and scoring, the running order |

**The running order is fixed** by `TEACHING-BRIEF.md` §7 — 40 units, gates included. Follow it; don't
invent units, reorder them, or decide what to teach next ad hoc.

**Read order before teaching anything** (`TEACHING-BRIEF.md` §3): the target statement in
`EXAM-OBJECTIVES.md` → that statement's sources in `RESOURCES.md` → §1 and §2 of `TEACHING-BRIEF.md`.

**Each fact has exactly one owner.** Exam format, item counts, timing and scoring live in
`TEACHING-BRIEF.md` §1 and are deliberately not repeated in `RESOURCES.md`. Don't copy a fact from one
file into another — cross-reference it, so the two can't drift apart.

## Editing the kit files

- **`EXAM-OBJECTIVES.md` is verbatim.** It reproduces §6 of the exam guide word for word; the only
  additions are documented in its own header. Never edit, reword, reorder, or "improve" it — doing so
  breaks the provenance claim that makes it load-bearing.
- **`RESOURCES.md` and `TEACHING-BRIEF.md` are editable, but only when asked.** Don't rewrite them as
  a side effect of another task.
- **No unsourced facts, in any of them.** Every claim needs an anchor — an exam guide `§n` or an
  official doc URL. Never add something from memory, and never from a scraped question bank
  (`TEACHING-BRIEF.md` §4).

## Study output is personal, not repository

`MISSION.md`, `NOTES.md`, `PROGRESS.md`, `lessons/`, `learning-records/`, `reference/`, and `assets/`
are gitignored on purpose. They are one learner's progress, not the kit. Never commit them and never
propose removing them from `.gitignore` — the kit stays a clean template that can be `git pull`ed into
without conflicts.

## Before answering from memory

The docs moved and terminology has drifted; `RESOURCES.md` has a **Terminology drift** section listing
the specific facts that stale sources get wrong, a **Fetching notes** section with the current doc
hosts, a **Gaps** section for concepts no public doc covers, and an **Out of scope — do not study**
section (exam guide §17). Read the relevant one before teaching a topic rather than answering from
parametric knowledge — this is exactly where a confident wrong answer costs the learner exam points.
