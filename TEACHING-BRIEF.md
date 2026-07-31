# CCAR-F — Teaching Brief

How to teach someone to pass the **Claude Certified Architect – Foundations** exam. Learner-agnostic —
holds no personal progress.

`§n` refers to the *Claude Certified Architect – Foundations Exam Guide, v1.0, July 2026*. Companion
files: `EXAM-OBJECTIVES.md` (what is tested) and `RESOURCES.md` (what to teach from).

---

## 1. The exam in one page

| | |
|---|---|
| **Items** | 60 |
| **Item format** | Multiple-choice **and multiple-response**; each item states how many to select |
| **Structure** | 4 scenarios, drawn from a bank of 6 |
| **Time** | 120 minutes — about 2 minutes per item |
| **Pass** | Scaled score **720** on a 100–1,000 scale |
| **Fee / validity** | $125 USD · 12 months |

**On multiple-response items.** §3 states they exist and that each item says how many responses to
select — but **no example has ever been published.** All 12 of the guide's own sample questions are
single-answer, community mocks contain none, and the most detailed passer account doesn't mention
meeting one. So the format is confirmed; its shape is not. Anything generated is an educated guess, not
a reproduction — say so when you offer one.

**Scoring (§10).** Criterion-referenced — measured against a fixed standard, not ranked against others.
Per-domain percentages on the report are informational only; pass/fail is the total scaled score. You
cannot fail a section.

**Where the points are (§4)** — approximate item counts out of 60:

| Domain | Weight | ≈ items |
|---|---|---|
| 1 · Agentic Architecture & Orchestration | 27% | ~16 |
| 3 · Claude Code Configuration & Workflows | 20% | ~12 |
| 4 · Prompt Engineering & Structured Output | 20% | ~12 |
| 2 · Tool Design & MCP Integration | 18% | ~11 |
| 5 · Context Management & Reliability | 15% | ~9 |

**The six scenarios (§5)** — study all six; four appear:

1. **Customer Support Resolution Agent** — Agent SDK, MCP tools (`get_customer`, `lookup_order`, `process_refund`, `escalate_to_human`), 80%+ first-contact resolution → D1, D2, D5
2. **Code Generation with Claude Code** — slash commands, CLAUDE.md, plan mode vs direct → D3, D5
3. **Multi-Agent Research System** — coordinator delegating to search / analysis / synthesis / report subagents, cited output → D1, D2, D5
4. **Developer Productivity with Claude** — unfamiliar codebases, built-in tools, MCP → D2, D3, D1
5. **Claude Code for Continuous Integration** — automated review, test generation, minimising false positives → D3, D4
6. **Structured Data Extraction** — JSON-schema validation, edge cases, downstream integration → D4, D5

---

## 2. What the exam rewards

**Judgment, not recall.** §1 says items are *"grounded in realistic scenarios drawn from actual
customer use cases"* and require *"not only conceptual knowledge but practical judgment about
architecture, configuration, and tradeoffs in production deployments."*

Three consequences for every lesson:

**Usually every option works — but check that first.** The characteristic item gives four plausible
mechanisms and asks which is *best*; the difference is reliability, cost, latency or context
efficiency, not correctness. **Some items are not like that.** The guide's own sample rationales
describe distractors as *"a configuration mechanism that doesn't exist in Claude Code"* (Q4) and
*"non-existent features (CLAUDE_HEADLESS environment variable, `--batch` flag)"* (Q10) — in Q10 three
of four options are simply wrong.

So the order matters: **first eliminate anything that isn't real, then choose the most reliable of
what's left.** A learner drilled only on "they're all plausible, weigh the tradeoffs" will overthink
the recall items and second-guess a correct elimination.

**Many stems ask for a superlative.** The guide's samples ask for the *most effective* change, the
*most effective **first** step*, the *most maintainable* way, the *most likely **root cause***. When
that word is present, let it decide between two options that both work. But roughly a third of the
sample stems have no superlative at all — *"Where should you create this command file?"*, *"Which
approach should you take?"*, *"How should you evaluate this proposal?"* — so treat it as a strong
signal when present, not a rule that's always available.

**Definitions are rarely asked directly.** Not *"what is hub-and-spoke?"* but *"given this situation,
what do you do?"* — though some items are closer to configuration recall than to judgment.

---

## 3. Session protocol

Generic pedagogy — one concept per lesson, always quiz, balanced option lengths, retrieval and spacing
— is handled by the `teach` skill. Only these four are CCAR-F-specific:

1. **Read order.** Target statement in `EXAM-OBJECTIVES.md` → its sources in `RESOURCES.md` → §1 and §2 of this file.
2. **The bullets are the scope boundary and the floor — never the lesson plan.** They are terse scope
   statements for a candidate with 6+ months hands-on experience. Teach the mechanism beneath them,
   drill judgment above them, use them at the end as a completeness audit.
3. **Grade on the reason, not the label.** A right answer for a wrong reason is a miss.
4. **Log misses by task statement number, not topic.** "Weak on hooks" is not actionable. "1.4 and 1.5,
   both times the question was about *where* to enforce" is.

---

## 4. How to write a CCAR-F item

**The shape:**

1. A short production scenario — a system, a symptom, and a number if possible ("12% of cases", "40%
   latency increase", "14 files").
2. A stem that names the decision. Most often a **superlative** — most effective / most effective
   *first* step / most maintainable / most likely root cause — but a plain "where should this go?" or
   "how should you evaluate this?" is equally faithful; see §2 of this file.
3. Options that are **each defensible on their face**. Usually all four genuinely work and the
   difference is reliability, cost, latency or context efficiency. Occasionally one names something
   that doesn't exist — the guide does this too, so it's fair, just not the default.
4. A rationale for **every** option, saying what real-world failure the wrong choice causes.
5. A tag: domain, task statement, and the judgment the item turns on.

**Rules:**

- **Balance option lengths.** A visibly longer or more detailed option reads as correct regardless of
  content.
- **Offer multiple-response items each lesson; don't impose them.** The exam guide's §3 says the exam
  uses them, so ask the learner whether they want some in this lesson's quiz. Default to single-answer
  when they don't — and whenever you do produce one, flag that its shape is inferred from the guide's
  one-line description, since no published example exists (§1).
- **Distractors should be plausible mechanisms, mostly.** An option that is obviously silly tests
  nothing. But the guide's own items do sometimes use a named feature that doesn't exist, so an
  occasional "does this flag/setting actually exist?" item is faithful to the exam — just don't let it
  become the pattern.
- **Anchor every claim** to the docs or the exam guide — never to a scraped question bank.
- **Name the statement.** An item that cannot be tagged to one of the 30 is off-blueprint — discard it.
- **Check scope.** The exam guide's §17 out-of-scope list is explicit: rate limits, quotas, prompt-caching internals,
  vision, computer use, streaming, tokenization, cloud provider config and auth protocols are *not
  tested*.

**Borrowing from existing question sets.** Take the format — per-option rationales, statement tags,
provenance lines. Never the content, answers, or taxonomy. Community mocks misattribute statements — a widely-used free
one tags CLAUDE.md as 3.2 (it is 3.1) and hooks as 3.3 (they are 1.5), and includes an item on rate
limiting, which the guide's §17 lists as out of scope. **Anthropic's §9 rationales are the model to
imitate.** Any conflict resolves against the third party.

---

## 5. Schedule the hands-on work

This exam rewards people who have built the things, and the guide's four preparation exercises are the
part learners skip. Schedule them; don't leave them optional.

| # | Exercise | Reinforces | Attempt after |
|---|---|---|---|
| 1 | Multi-tool agent with escalation logic | D1, D2, D5 | D5 |
| 2 | Claude Code for a team development workflow | D3, D2 | D3 |
| 3 | Structured data extraction pipeline | D4, D5 | D5 |
| 4 | Multi-agent research pipeline | D1, D2, D5 | D5 |

Full steps are in the exam guide's §8; the seven shorter activities are in its §7. Don't paraphrase
them from memory — read them there. Exercise 2 unlocks earliest; the rest need Domain 5.

An exercise attempted before every domain it reinforces has been taught teaches frustration, not
judgment. Gate them.

**Gate these.** An exercise is attempted only once every domain it reinforces has been taught.
Exercise 2 unlocks earliest (D3 + D2); 1, 3 and 4 all need D5.

---

## 6. Scenario drill gating

Drill a scenario block **only after every domain it depends on is taught.** Drilling early produces
guessing, not judgment.

| Scenario | Needs | Earliest |
|---|---|---|
| 4 · Developer Productivity | D2, D3, D1 | after D3 |
| 5 · Claude Code in CI | D3, D4 | after D4 |
| 1 · Customer Support Agent | D1, D2, D5 | after D5 |
| 2 · Code Generation with Claude Code | D3, D5 | after D5 |
| 3 · Multi-Agent Research | D1, D2, D5 | after D5 |
| 6 · Structured Data Extraction | D4, D5 | after D5 |

---

## 7. The running order

**40 units — 30 lessons, 4 exercises, 6 scenario drills.** Decided once, here, so it is not
re-decided every session. A tutor follows this order; it does not invent units or reorder them.

**The course does not end at the last lesson.** Ten of the forty are exercises and drills, and
seven of those fall after lesson 30 because they need Domain 5. Finishing the lessons is about
four-fifths of the work — say so early, or the learner will stop at 30 believing they are done.

Gates below are the ones already set in §5 and §6; this section places them, it does not add any.
The domain order is blueprint order, which is the default for a learner with no diagnostic — an
individual who already knows their weakest domain should start there instead (`RESOURCES.md`).

| # | Unit | Type | Ref |
|---|---|---|---|
| **Domain 1 — Agentic Architecture & Orchestration (27%)** ||||
| 1 | Agentic loop / `stop_reason` | lesson | 1.1 |
| 2 | Coordinator–subagent orchestration | lesson | 1.2 |
| 3 | Subagent invocation & context passing | lesson | 1.3 |
| 4 | Enforcement & handoff (gates vs prompts) | lesson | 1.4 |
| 5 | Agent SDK hooks | lesson | 1.5 |
| 6 | Task decomposition (chaining vs dynamic) | lesson | 1.6 |
| 7 | Session state: resume & fork | lesson | 1.7 |
| **Domain 2 — Tool Design & MCP Integration (18%)** ||||
| 8 | Tool descriptions & boundaries | lesson | 2.1 |
| 9 | Structured MCP errors | lesson | 2.2 |
| 10 | Tool distribution & `tool_choice` | lesson | 2.3 |
| 11 | MCP server integration | lesson | 2.4 |
| 12 | Built-in tools | lesson | 2.5 |
| **Domain 3 — Claude Code Configuration & Workflows (20%)** ||||
| 13 | CLAUDE.md hierarchy & modular organization | lesson | 3.1 |
| 14 | Slash commands & skills | lesson | 3.2 |
| 15 | Path-specific rules | lesson | 3.3 |
| 16 | Plan mode vs direct execution | lesson | 3.4 |
| 17 | Iterative refinement | lesson | 3.5 |
| 18 | Claude Code in CI/CD | lesson | 3.6 |
| **▸ Gate — D1, D2, D3 taught** ||||
| 19 | Claude Code for a team development workflow | exercise | §5 · Ex 2 |
| 20 | Developer Productivity with Claude | scenario | §6 · Sc 4 |
| **Domain 4 — Prompt Engineering & Structured Output (20%)** ||||
| 21 | Explicit criteria / false positives | lesson | 4.1 |
| 22 | Few-shot prompting | lesson | 4.2 |
| 23 | Structured output via `tool_use` + JSON schema | lesson | 4.3 |
| 24 | Validation, retry & feedback loops | lesson | 4.4 |
| 25 | Batch processing | lesson | 4.5 |
| 26 | Multi-instance / multi-pass review | lesson | 4.6 |
| **▸ Gate — D4 taught** ||||
| 27 | Claude Code in CI | scenario | §6 · Sc 5 |
| **Domain 5 — Context Management & Reliability (15%)** ||||
| 28 | Preserve critical info across long context | lesson | 5.1 |
| 29 | Escalation & ambiguity resolution | lesson | 5.2 |
| 30 | Error propagation across multi-agent | lesson | 5.3 |
| 31 | Context in large codebase exploration | lesson | 5.4 |
| 32 | Human review & confidence calibration | lesson | 5.5 |
| 33 | Provenance & uncertainty in synthesis | lesson | 5.6 |
| **▸ Gate — D5 taught. Everything remaining unlocks here.** ||||
| 34 | Multi-tool agent with escalation logic | exercise | §5 · Ex 1 |
| 35 | Structured data extraction pipeline | exercise | §5 · Ex 3 |
| 36 | Multi-agent research pipeline | exercise | §5 · Ex 4 |
| 37 | Customer Support Agent | scenario | §6 · Sc 1 |
| 38 | Code Generation with Claude Code | scenario | §6 · Sc 2 |
| 39 | Multi-Agent Research | scenario | §6 · Sc 3 |
| 40 | Structured Data Extraction | scenario | §6 · Sc 6 |

**Separately, the seven shorter activities** in the exam guide's §7 are ungated and not placed
here — the guide states no prerequisites for them. Read them there and fit them where they help.

**On numbering.** These are positions in a sequence, not filenames. Only the 30 lessons produce a
lesson file, so unit numbers and lesson numbers diverge after unit 19 — log misses by **task
statement**, per §3, and the discrepancy never matters.

**On splitting.** Statements 1.7 and 2.2 each carry two concepts (resume *and* fork; two error
mechanisms *and* four categories). Split either into two lessons if it runs long — that is a
session judgment, made by the tutor who can see how it is going, not fixed here.

---

## 8. Readiness

Practice scores are the most convenient evidence, and the easiest to misread.

Repeating the same question bank measures memory, not judgment; a rising score on a bank you have seen
before is not evidence. Community answer keys can be wrong — where a key disagrees with the exam guide
or the official docs, the key is wrong. People who have passed report wanting **roughly 90% on practice
before booking**; that is community guidance, not official, and it accounts for the real exam being
more verbose and less forgiving than practice sets. Note that a practice percentage is not the same
thing as the exam's 100–1,000 *scaled* score, which is equated across exam forms — a third-party set
cannot produce one, so don't read 720 as "72% correct".

**The honest test.** Give the learner a production symptom they have never seen and a set of
plausible fixes. If they can name the heuristic that decides it before they pick, they are ready.
