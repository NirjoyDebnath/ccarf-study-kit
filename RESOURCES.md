# CCAR-F Resources

Trusted sources for the Anthropic **Claude Certified Architect – Foundations** exam (code CCAR-F).

Domain weights (§4): **D1 27% · D2 18% · D3 20% · D4 20% · D5 15%** — they set how much time each
domain deserves, which is why they appear here. **The exam's format and scoring live in
`TEACHING-BRIEF.md` §1**, which owns those facts; they are not repeated here so they cannot drift.
One that matters while you study, though: the guide says items are **multiple-choice *and*
multiple-response**, each stating how many responses to select — so don't assume every item is
pick-one. No example of a multiple-response item has been published anywhere, so its exact shape is
unknown; see `TEACHING-BRIEF.md` §1.

Organised **by domain → task statement (all 30, in blueprint order)**. Every entry is free
or free-with-signup. Each entry says what it covers *for that statement* — the same source
appears under several statements because it teaches different things in each place.

For how to *teach* from these — session protocol, item-writing rules, exercise scheduling — see
`TEACHING-BRIEF.md`.

**Where a statement has no `Exam guide §17 also names` box, that means no note was written, not that
there is nothing more to know.** Always read the statement in `EXAM-OBJECTIVES.md` alongside this file.

## How to use this file

1. Pick one task statement (start with your weakest domain, not the top of the file). If you
   don't know which is weakest yet, follow the running order in `TEACHING-BRIEF.md` §7.
2. Paste that statement's **Tutor prompt** plus its sources into a tutor session — the two or three
   most relevant, not all of them. Entries marked **the** page or **best source** come first.
3. Demand scenario questions, not definitions. The exam is judgment.
4. **Ask for multiple-response items when you want them.** The guide says the exam uses them, but no
   published example exists, so a tutor will default to pick-one. Requesting a few is worth doing —
   just treat their shape as an educated guess, not a reproduction.
5. Log every miss by **task statement number**, not by topic. That is how you find the hole.

## Fetching notes

**The docs moved.** API and prompt-engineering pages are on `platform.claude.com/docs/en/…`;
Claude Code **and all Agent SDK** pages are on `code.claude.com/docs/en/…`. Old
`docs.anthropic.com` / `docs.claude.com` links redirect. `platform.claude.com/docs/en/agent-sdk/*`
issues a cross-host 307 that some fetchers won't follow — use the `code.claude.com` form.

**Large pages** — pass a narrow prompt or they eat the tutor's context: `skills` (~72KB),
`batch-processing` (~71KB), `search-results` (~81KB), prompting best practices (~58KB).

**Page inventories** — `code.claude.com/docs/llms.txt` and `platform.claude.com/docs/llms.txt`
are complete current page lists. Use them to confirm a doc exists instead of guessing a URL.

---

# Knowledge

## Domain 1 — Agentic Architecture & Orchestration (27%)

### 1.1 — Agentic loop / stop_reason

- [Handling stop reasons](https://platform.claude.com/docs/en/build-with-claude/handling-stop-reasons) — Anthropic Platform Docs — all seven values (`end_turn`, `max_tokens`, `stop_sequence`, `tool_use`, `pause_turn`, `refusal`, `model_context_window_exceeded`) and correct handling for each. The single most exam-critical API page in Domain 1. — 15 min
- [Handle tool calls](https://platform.claude.com/docs/en/agents-and-tools/tool-use/handle-tool-calls) — Anthropic Platform Docs — the mechanics of one loop turn: read `tool_use` id/name/input, run it, reply with `tool_result` blocks FIRST in the content array. — 20 min
- [How the agent loop works (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/agent-loop) — Claude Code Docs — the loop as a programmable object: turn definition, `ResultMessage.subtype` termination signals, `max_turns` counting tool-use turns only. — 25 min
- [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works) — Claude Code Docs — gather context → take action → verify results, and how the harness supplies each. — 25 min
- [Loop engineering: getting started with loops](https://claude.com/blog/getting-started-with-loops) — Claude blog — four loop types, each with an explicit trigger and STOP CONDITION. Cleanest framing of "what ends a loop". — 10 min
- [I passed CCAR-F with 893/1000](https://medium.com/@kishorkukreja/i-passed-anthropics-claude-certified-architect-foundations-exam-with-a-score-of-893-1000-2206c27efd6c) — Medium: Kishor Kukreja — names the #1 exam anti-pattern: parsing assistant text for completion instead of keying off `stop_reason`. — 10 min

**Tutor prompt:**
> Teach me task statement 1.1. Enumerate every `stop_reason` value with its correct handling, then contrast three ways an engineer could decide "the agent is done": (a) parsing the assistant's text for phrases like "task complete", (b) an iteration counter, (c) `stop_reason`. Explain exactly why (a) and (b) fail and what production failure each causes. Then quiz me on `pause_turn` vs `max_tokens` vs `model_context_window_exceeded` — which are truncation, which are continuation, and what you send back for each. Finally: what is the difference between the API's `stop_reason` and the Agent SDK's `ResultMessage.subtype`, and when would an exam item use one to distract from the other?

### 1.2 — Coordinator–subagent orchestration

- [Run agents in parallel](https://code.claude.com/docs/en/agents) — Claude Code Docs — the decision table plus three discriminating questions: who coordinates, do workers need to talk to each other, do tasks touch the same files. — 12 min
- [Create custom subagents](https://code.claude.com/docs/en/sub-agents) — Claude Code Docs — the frontmatter contract (`tools`, `disallowedTools`, `model`, `skills`, `memory`, `isolation`, `permissionMode`, `mcpServers`), the Task→Agent rename, the 3-layer nesting limit. — 35 min
- [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) — Anthropic Engineering — production orchestrator-worker case study; effort-scaling rules; the 15× token economics that decide whether multi-agent is justified at all. — 35 min
- [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) — Anthropic Engineering — the five composable patterns and the workflow-vs-agent boundary. — 30 min
- [Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows) — Claude Code Docs — the four-way table organised by *who holds the plan*. — 30 min
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic Engineering — subagents return condensed 1,000–2,000 token summaries; the quantitative version of "the subagent returns three lines". — 25 min
- [Escalate hard decisions with the advisor tool](https://code.claude.com/docs/en/advisor) — Claude Code Docs — the inverted orchestration shape: weak executor, strong advisor consulted at decision points. — 12 min

**Tutor prompt:**
> Teach me task statement 1.2. Build me one table comparing subagents, agent view, agent teams, dynamic workflows, and the advisor tool on: who holds the plan, where intermediate results live, how many workers, whether workers can talk to each other, and what happens on interruption. Then give me one-paragraph scenarios (codebase-wide audit; 500-file migration; verbose log analysis; cross-checked research; a task needing sign-off between stages; a cheap executor that occasionally hits a hard call) and make me pick the mechanism. Grade me on the *reason*, not the label. Finish by explaining the 4×/15× token economics from the multi-agent research post and when multi-agent is the WRONG answer.

### 1.3 — Subagent invocation & context passing (Task tool)

> **Exam guide §6/§17 also names:** `allowedTools` must include `"Task"` for a coordinator to invoke
> subagents at all · the `AgentDefinition` config (descriptions, system prompts, tool restrictions) ·
> **structured state persistence and crash recovery using manifests** · spawning parallel subagents by
> emitting **multiple Task calls in a single response**, not across separate turns · using structured
> formats to keep source URLs / document names / page numbers separable for attribution.

- [Create custom subagents](https://code.claude.com/docs/en/sub-agents) — Claude Code Docs — **the** 1.3 fact: a subagent starts with a fresh, isolated context window; it does not see your conversation history, the skills already invoked, or files already read. The exception is a fork. — 35 min
- [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) — Anthropic Engineering — every subagent needs an objective, an output format, tool/source guidance, and clear task boundaries; vague delegation made subagents duplicate work. — 35 min
- [Handle approvals and user input (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/user-input) — Claude Code Docs — `AskUserQuestion` is **not available in subagents**, so a subagent must return a structured "needs clarification" result to the coordinator. — 18 min
- [Set up Claude Code in a monorepo or large codebase](https://code.claude.com/docs/en/large-codebases) — Claude Code Docs — which config surfaces do and do not reach a delegated worker. — 25 min
- [I passed CCAR-F with 893/1000](https://medium.com/@kishorkukreja/i-passed-anthropics-claude-certified-architect-foundations-exam-with-a-score-of-893-1000-2206c27efd6c) — Medium: Kishor Kukreja — subagents receive nothing automatically; information must be packed into the Task prompt. — 10 min
- [How I Passed the CCA-F Exam](https://medium.com/@johnelanji/how-i-passed-the-anthropic-claude-certified-architect-foundations-cca-f-exam-a-complete-1108dce46e9b) — Medium: John Mathew — "context isolation in multi-agent systems" is one of his five recurring exam patterns. — 15 min

**Tutor prompt:**
> Teach me task statement 1.3. State precisely what a non-fork subagent inherits and what it does not (conversation history, files already read, skills invoked, output style, parent context-window size, auto memory). Then give me a scenario: a coordinator has been told "never touch the vendor/ directory", and it delegates a refactor to a subagent. Walk me through what actually happens and what the architect must do. Then quiz me on the near-miss options: restating the rule in the delegation prompt vs putting it in CLAUDE.md vs a PreToolUse hook vs `disallowedTools` vs a permissions deny rule — rank them by reliability and explain when each is the intended answer. Finally, what changes if the subagent is a fork (`/subtask`) instead?

### 1.4 — Enforcement & handoff (gates vs prompts)

- [Automate actions with hooks (guide)](https://code.claude.com/docs/en/hooks-guide) — Claude Code Docs — the cleanest official statement of the thesis: hooks give "deterministic control: certain actions always happen rather than relying on the LLM to choose to run them." — 15 min
- [Configure permissions](https://code.claude.com/docs/en/permissions) — Claude Code Docs — the second deterministic surface: allow/ask/deny with tool-specific syntax, managed policy, working-directory scoping. — 25 min
- [Choose a permission mode](https://code.claude.com/docs/en/permission-modes) — Claude Code Docs — what each mode auto-approves; protected paths that `allow` rules can never override because the safety check runs first. — 25 min
- [Claude Code settings](https://code.claude.com/docs/en/settings) — Claude Code Docs — scope precedence, and the exception that matters: permission rules MERGE across scopes, and DENY beats ALLOW. — 20 min
- [Keep Claude working toward a goal (/goal)](https://code.claude.com/docs/en/goal) — Claude Code Docs — a textbook gate: a **fresh** evaluator model decides whether Claude may stop. — 14 min
- [Handle approvals and user input (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/user-input) — Claude Code Docs — "the callback never fires for auto-approved tools" — only a PreToolUse hook is guaranteed to see every tool call. — 18 min
- [Tools reference](https://code.claude.com/docs/en/tools-reference) — Claude Code Docs — one tool-name string serves permission rules, subagent tool lists and hook matchers. — 30 min
- [The CCA Exam: A Practical Guide with Real Code](https://medium.com/@sathishkraju/the-claude-certified-architect-exam-a-practical-guide-with-real-code-d01e123238ac) — Medium: Sathish Raju — "If it must always happen, use code. If it should usually happen, use a prompt." — 15 min

**Tutor prompt:**
> Teach me task statement 1.4 as an enforcement-layer vs guidance-layer decision. Rank every mechanism by reliability: prompt instruction, CLAUDE.md rule, skill, subagent `tools` allowlist, `disallowedTools`, permissions allow/ask/deny, permission mode, PreToolUse hook, Stop hook, `/goal`. For each, say what it can and cannot guarantee. Then give me the classic exam scenario — a `process_refund` tool must never fire above $500 — and make me argue why a system-prompt instruction is the wrong answer even though it works "most of the time". Quiz me on the precedence traps: why does `permissions.allow` not pre-approve protected paths, why does a `canUseTool` callback not fire for auto-approved tools, and why do permission rules merge across scopes when everything else overrides?

### 1.5 — Apply **Agent SDK** hooks for tool call interception and data normalization

> ⚠️ **Scope correction.** The exam guide titles this statement *"Apply **Agent SDK** hooks…"* and §17
> lists hooks under **Claude Agent SDK** — "hooks (PostToolUse, tool call interception)". The sources
> below are predominantly **Claude Code** hooks documentation. The concepts transfer and the Claude
> Code pages are the most detailed writing on hooks that exists, but the exam's framing is SDK-side.
> Read them for the mechanism; expect items phrased in Agent SDK terms.

- [Hooks reference](https://code.claude.com/docs/en/hooks) — Claude Code Docs — **best single page for this statement.** 30+ events, matcher syntax, JSON stdin contract, `permissionDecision` values (`deny`/`allow`/`ask`/`defer`), exit codes 0/2/other, five hook types (command, http, mcp_tool, prompt, agent). — 35 min
- [Automate actions with hooks (guide)](https://code.claude.com/docs/en/hooks-guide) — Claude Code Docs — seven copy-ready recipes plus the prompt-based and agent-based handlers for judgment calls. — 15 min
- [Claude Code settings](https://code.claude.com/docs/en/settings) — Claude Code Docs — where hooks live, `allowedHttpHookUrls`, `allowManagedHooksOnly`, `disableAllHooks`. — 20 min
- [Explore the .claude directory](https://code.claude.com/docs/en/claude-directory) — Claude Code Docs — where every config surface lives and when it loads. — 10 min
- [Run parallel sessions with worktrees](https://code.claude.com/docs/en/worktrees) — Claude Code Docs — `WorktreeCreate`/`WorktreeRemove` hooks replacing the git logic entirely: hooks as a *substitution* point, not only a veto. — 18 min
- [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action) — Anthropic Academy *(free signup)* — the Hooks module teaches enforcement of non-negotiable rules in context. — 180 min

**Tutor prompt:**
> Teach me task statement 1.5. First split the hook events into two lists: those that CAN block (PreToolUse, PermissionRequest, UserPromptSubmit, Stop, SubagentStop, PreCompact, ConfigChange) and those that CANNOT because the action already happened (PostToolUse, SessionEnd, Notification, FileChanged). Then drill the two control channels: exit codes (0 = parse stdout for JSON, 2 = blocking error with stderr as the reason, anything else = non-blocking) and the `hookSpecificOutput.permissionDecision` JSON with deny/allow/ask/defer. Give me scenarios — auto-format after edits, block edits to protected files, re-inject context after compaction, audit config changes, require judgment about whether a commit message is acceptable — and make me pick the event, the matcher, and the hook TYPE (command vs prompt vs agent vs http vs mcp_tool). Explain when a shell hook is the wrong tool because the decision needs judgment.

### 1.6 — Task decomposition (chaining vs dynamic)

- [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) — Anthropic Engineering — prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer; and the predefined-code-path vs model-directed boundary. — 30 min
- [Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows) — Claude Code Docs — moving the plan out of Claude's turn-by-turn judgment into a script; limits (16 concurrent, 1,000 per run, no mid-run user input). — 30 min
- [Run agents in parallel](https://code.claude.com/docs/en/agents) — Claude Code Docs — the discriminating questions for choosing a coordination style. — 12 min
- [Loop engineering: getting started with loops](https://claude.com/blog/getting-started-with-loops) — Claude blog — turn-based vs goal-based vs time-based vs proactive, each with its stop condition. — 10 min
- [Increase output consistency](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency) — Anthropic Platform Docs — the first-party rationale for chaining: "each subtask gets Claude's full attention, reducing inconsistency errors across scaled workflows." — 14 min
- [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) — Anthropic Platform Docs — the "Chain complex prompts" and "Subagent orchestration" sections. — 40 min
- [How the agent loop works (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/agent-loop) — Claude Code Docs — turns, `max_turns`, `max_budget_usd`, and what actually bounds a dynamic decomposition. — 25 min

**Tutor prompt:**
> Teach me task statement 1.6 as one question: **who decides what runs next — the model, or code I wrote?** Define prompt chaining, routing, parallelization (sectioning vs voting), orchestrator-workers, and evaluator-optimizer, each briefly, with its failure mode. Then give me short scenarios and make me choose between a fixed chain and dynamic decomposition, justifying on predictability, cost, latency and context. Include at least two where the naive answer (spin up subagents) is wrong because the steps are known in advance. Close by explaining the workflow-vs-agent boundary from "Building effective agents" in one sentence I could reuse on the exam.

### 1.7 — Session state: resume & fork

> **Exam guide §17 uses this vocabulary:** `--resume`, **`fork_session`**, **named sessions**, and
> **session context isolation**. Expect `fork_session` as the written form rather than `/fork`.

- [Manage sessions](https://code.claude.com/docs/en/sessions) — Claude Code Docs — `--continue` / `--resume` / `--from-pr` / `/resume`; exactly what resume restores and what it never does (plan and bypassPermissions modes, `--mcp-config`, `--settings`, `--add-dir`); the three-way resume-from-summary dialog; `/branch` keeps session permission grants, `--fork-session` does not. — 30 min
- [Persist sessions to external storage (SessionStore)](https://code.claude.com/docs/en/agent-sdk/session-storage) — Claude Code Docs — the mechanism-level answer: "forkSession is not a byte copy" — it rewrites every sessionId and REMAPS message UUIDs. Also `getSessionMessages` returns the post-compaction chain, not raw history. — 20 min
- [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works) — Claude Code Docs — resume reopens the SAME session ID and appends; fork copies history into a NEW session ID leaving the original untouched. — 25 min
- [Commands](https://code.claude.com/docs/en/commands) — Claude Code Docs — `/clear`, `/resume`, `/branch`, `/fork`, `/rewind` and what each actually rolls back. — 12 min
- [Run parallel sessions with worktrees](https://code.claude.com/docs/en/worktrees) — Claude Code Docs — resume returns a session to its worktree; a fork starts in the launch directory. — 18 min
- [Run Claude Code programmatically](https://code.claude.com/docs/en/headless) — Claude Code Docs — session-ID lookup is scoped to the current project directory and its git worktrees; `-p`/SDK sessions don't appear in the picker but resume by ID. — 30 min

**Tutor prompt:**
> Teach me task statement 1.7. Draw the precise line between resume and fork: same session ID vs new one, what happens to the original, what carries over, what must be passed again on the command line. Then quiz me on the traps: (1) `/branch` vs `--fork-session` and session permission grants; (2) why "resume from summary" can silently lose information; (3) what a resumed session does NOT restore; (4) why `getSessionMessages` returned 18 messages when the store has 503 entries; (5) why a forked transcript can't be a byte copy; (6) resuming a session that was running inside a worktree. For each, tell me the concrete production symptom an architect would see.

---

## Domain 2 — Tool Design & MCP Integration (18%)

### 2.1 — Tool descriptions & boundaries

- [Define tools (schemas, descriptions, tool_choice)](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools) — Anthropic Platform Docs — "extremely detailed descriptions… by far the most important factor in tool performance"; aim for 3–4 sentences minimum; consolidate related operations; namespace by service. — 25 min
- [Writing effective tools for AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents) — Anthropic Engineering — write for a new team member; unambiguous parameter names (`user_id` not `user`); return only high-signal information; evaluate tool specs empirically. — 25 min
- [Troubleshooting tool use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/troubleshooting-tool-use) — Anthropic Platform Docs — the rule an item can turn on: "Differentiate tools by WHEN to use them, not only WHAT they do." — 12 min
- [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) — Anthropic Engineering — the agent-computer-interface section: example usage, edge cases, input format requirements, clear boundaries, poka-yoke parameter design. — 30 min
- [Give Claude custom tools (in-process MCP server)](https://code.claude.com/docs/en/agent-sdk/custom-tools) — Claude Code Docs — tool anatomy, annotations (`readOnlyHint` is the only one with behaviour), "annotations are metadata, not enforcement." — 22 min
- [Understanding MCP servers](https://modelcontextprotocol.io/docs/learn/server-concepts) — modelcontextprotocol.io — tools are model-controlled, resources application-controlled, prompts user-controlled. — 25 min
- [The CCA Exam: A Practical Guide with Real Code](https://medium.com/@sathishkraju/the-claude-certified-architect-exam-a-practical-guide-with-real-code-d01e123238ac) — Medium: Sathish Raju — agents work best with 4–5 tools, not 18; tool descriptions are routing logic. — 15 min

**Tutor prompt:**
> Teach me task statement 2.1. Show me a bad tool description ("Gets customer information") and rewrite it to Anthropic's standard, narrating each thing you added: what it does, when to use it, when NOT to use it, what each parameter means, and what it does NOT return. Then drill consolidation: why `create_pr`/`review_pr`/`merge_pr` should probably be one tool with an `action` parameter, and when consolidation goes too far. Quiz me on pairs of tools with overlapping descriptions and make me predict which one Claude calls and why. Finish with the namespacing rule and why "fewer, more capable tools reduce selection ambiguity" is the exam's preferred phrasing.

### 2.2 — Structured MCP errors (isError, categories)

- [MCP specification — Tools](https://modelcontextprotocol.io/specification/2025-06-18/server/tools) — modelcontextprotocol.io — **normative source.** Two distinct mechanisms: JSON-RPC protocol errors (unknown tool, invalid arguments) vs tool EXECUTION errors returned inside a successful result with `isError: true`. Also `outputSchema` / `structuredContent`. — 25 min
- [Give Claude custom tools](https://code.claude.com/docs/en/agent-sdk/custom-tools) — Claude Code Docs — "A handler error doesn't stop the agent loop." Throwing surfaces the raw exception; returning `isError: true` surfaces the message YOU compose. — 22 min
- [Handle tool calls](https://platform.claude.com/docs/en/agents-and-tools/tool-use/handle-tool-calls) — Anthropic Platform Docs — "Write instructive error messages… include what went wrong and what Claude should try next." Claude retries 2–3 times with corrections before apologising. — 20 min
- [Troubleshooting tool use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/troubleshooting-tool-use) — Anthropic Platform Docs — symptom→cause→fix for malformed `tool_result` sequences and invented parameters. — 12 min
- [Writing effective tools for AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents) — Anthropic Engineering — errors should steer the agent toward a better strategy, and may include a correctly formatted input example. — 25 min
- [I passed CCAR-F with 893/1000](https://medium.com/@kishorkukreja/i-passed-anthropics-claude-certified-architect-foundations-exam-with-a-score-of-893-1000-2206c27efd6c) — Medium: Kishor Kukreja — the exam wants `errorCategory`, `isRetryable`, attempted queries and partial results — never "Operation failed". — 10 min

> **Exam guide §6 names the taxonomy directly** — it is official, not community vocabulary. Knowledge of:
> *"the distinction between transient errors (timeouts, service unavailability), validation errors
> (invalid input), business errors (policy violations), and permission errors"*. Skills in: *"returning
> structured error metadata including `errorCategory` (transient/validation/permission), `isRetryable`
> boolean, and human-readable descriptions"*. It also names *"why uniform error responses (generic
> "Operation failed") prevent the agent from making appropriate recovery decisions"* and reporting to
> the coordinator *"only errors that cannot be resolved locally, along with partial results and what was
> attempted"*.
>
> The **MCP spec** is narrower — it defines only the two mechanisms (JSON-RPC protocol error vs a result
> with `isError: true`) and no category vocabulary. Both are examinable; keep them straight.

**Tutor prompt:**
> Teach me task statement 2.2. First separate the two error mechanisms in the MCP spec — a JSON-RPC protocol error vs a result with `isError: true` — and explain why the distinction matters: one reaches the MODEL and enables recovery, the other is a transport failure the client handles. Then teach the four-category taxonomy (transient / validation / business / permission + `isRetryable`), which the exam guide names in its own objectives for 2.2 — while the MCP spec defines only the two mechanisms and no categories. Now quiz me: for each of these failures (HTTP 500, rate limit, malformed date argument, refund exceeds policy limit, expired OAuth token, unknown tool name, empty result set, partial results after a timeout) tell me the mechanism, the category, whether it's retryable, and what the error payload should contain. Punish me for any answer that returns a bare string.

### 2.3 — Tool distribution & tool_choice

- [Define tools](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools) — Anthropic Platform Docs — all four `tool_choice` options (`auto`, `any`, `tool`, `none`), the assistant-prefill side effect of `any`/`tool`, and incompatibility with manual extended thinking. — 25 min
- [Tool search tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool) — Anthropic Platform Docs — the numbers exam items anchor on: selection accuracy degrades past 30–50 tools; adopt at 10+ tools or >10k tokens of definitions; `defer_loading`, and at least one tool must stay non-deferred. — 30 min
- [Give Claude custom tools](https://code.claude.com/docs/en/agent-sdk/custom-tools) — Claude Code Docs — the availability-vs-permission table: `tools:` and bare-name `disallowedTools` remove a tool from context; `allowedTools` and scoped rules only change permission, leaving the tool visible so Claude may waste a turn. — 22 min
- [Writing effective tools for AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents) — Anthropic Engineering — namespacing as the distribution lever at scale. — 25 min
- [Manage tool context](https://platform.claude.com/docs/en/agents-and-tools/tool-use/manage-tool-context) — Anthropic Platform Docs — how definitions and results consume the window. *(medium confidence — path confirmed, page not opened)* — 12 min
- [Programmatic tool calling](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) — Anthropic Platform Docs — orchestrating tool calls from code so intermediate results never enter context. *(medium confidence)* — 12 min

**Tutor prompt:**
> Teach me task statement 2.3 in two halves. Half one, `tool_choice`: define `auto`, `any`, `tool`, `none`; explain that `any` and `tool` prefill the assistant message so Claude emits no natural-language text before the tool_use block; explain what `tool_choice: any` + `strict: true` guarantees together. Half two, distribution: at what tool count does selection accuracy degrade, when do I adopt the tool search tool, what does `defer_loading` do, and what 400 errors can I cause. Then quiz me on the availability-vs-permission distinction — for each of `tools: [...]`, bare `disallowedTools`, `allowedTools`, and `Bash(rm *)`, tell me whether Claude can still SEE the tool and what that costs. End with a scenario: an agent has 55 tools across five services and picks wrong ~30% of the time. Give me the candidate fixes, ranked.

### 2.4 — MCP server integration (.mcp.json, resources)

> **Exam guide §17 also names:** **environment variable expansion** in `.mcp.json` · **multi-server
> simultaneous access** · "resources for content catalogs, tools for actions" · description quality as
> the driver of tool *adoption*.

- [Connect Claude Code to tools via MCP](https://code.claude.com/docs/en/mcp) — Claude Code Docs — three scopes (local / project via committed `.mcp.json` / user), stdio vs HTTP/SSE, `claude mcp add`, MCP resources via `@` mentions, MCP prompts as slash commands, and the credential rule: don't commit servers needing personal credentials. — 30 min
- [Understanding MCP servers (tools vs resources vs prompts)](https://modelcontextprotocol.io/docs/learn/server-concepts) — modelcontextprotocol.io — the control model, direct resources vs parameterized resource templates, parameter completion. — 25 min
- [MCP specification — Tools](https://modelcontextprotocol.io/specification/2025-06-18/server/tools) — modelcontextprotocol.io — capability declaration, pagination, `notifications/tools/list_changed`. — 25 min
- [Introduction to Model Context Protocol](https://anthropic.skilljar.com/introduction-to-model-context-protocol) — Anthropic Academy *(free signup)* — build a server AND a client in Python; defining tools, resources and prompts by hand. — 120 min
- [Tools reference](https://code.claude.com/docs/en/tools-reference) — Claude Code Docs — custom tools come from MCP servers; skills route through the existing Skill tool rather than adding a tool entry. — 30 min

**Tutor prompt:**
> Teach me task statement 2.4. Start with the control model as a table: tools = model-controlled, resources = application-controlled, prompts = user-controlled — and give me scenarios where the naive answer is "make it a tool" but the correct answer is "expose it as a resource", explaining why. Then teach the three Claude Code config scopes and make me decide, for five servers (a company-wide read-only docs server, a personal GitHub PAT server, a shared CI server with no credentials, a local dev database, an org-mandated one), which scope and whether `.mcp.json` gets committed. Finish on direct resources vs resource templates with `{parameters}` and why templates are self-documenting.

### 2.5 — Built-in tools (Read/Write/Edit/Bash/Grep/Glob)

- [Tools reference](https://code.claude.com/docs/en/tools-reference) — Claude Code Docs — **the** page. Per-tool behaviour and permission column; the load-bearing sentence: "The tool names are the exact strings you use in permission rules, subagent tool lists, and hook matchers." — 30 min
- [Configure permissions](https://code.claude.com/docs/en/permissions) — Claude Code Docs — the tiered model: read-only tools need no approval in-scope; Bash prompts except a built-in read-only set; "don't ask again" persistence differs for Bash vs file modification. — 25 min
- [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works) — Claude Code Docs — the five built-in tool categories plus orchestration tools. — 25 min

**Tutor prompt:**
> Teach me task statement 2.5. Go tool by tool — Read, Write, Edit, Bash, Grep, Glob — and for each tell me: what it returns, what it handles that I might not expect (images, PDFs by page range, notebooks with outputs), whether it prompts for permission and under what condition, and what it costs in context. Then quiz me on the three-mechanism naming surface: write a permission rule, a subagent tool allowlist, and a hook matcher that all target the same tool, and explain why the strings must match exactly. Include the Bash `cd` persistence rule and why `Bash(git diff *)` with the space is not the same as `Bash(git diff*)`. Give me scenarios where an architect should reach for Grep/Glob instead of Bash and say why.

---

## Domain 3 — Claude Code Configuration & Workflows (20%)

### 3.1 — CLAUDE.md hierarchy & modular organization

- [How Claude remembers your project](https://code.claude.com/docs/en/memory) — Claude Code Docs — **the** page. Load order managed policy → user → project → local; files are CONCATENATED not overridden; `@path` imports (max depth 4, don't reduce context); `.claude/rules/`; `claudeMdExcludes`; and the exam-shaped line: "Claude treats them as context, not enforced configuration. To block an action regardless of what Claude decides, use a PreToolUse hook instead." — 30 min
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — the include/exclude table and the per-line test: "Would removing this cause Claude to make mistakes?" Bloated CLAUDE.md causes Claude to ignore actual instructions. — 35 min
- [Set up Claude Code in a monorepo or large codebase](https://code.claude.com/docs/en/large-codebases) — Claude Code Docs — per-directory CLAUDE.md vs path-scoped rules vs per-directory skills; and that where you START determines what loads. — 25 min
- [Explore the .claude directory](https://code.claude.com/docs/en/claude-directory) — Claude Code Docs — every config file with when it loads and whether it's committed. — 10 min
- [How the agent loop works (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/agent-loop) — Claude Code Docs — "Persistent rules belong in CLAUDE.md rather than in the initial prompt, because CLAUDE.md content is re-injected on every request." — 25 min
- [Extend Claude with skills](https://code.claude.com/docs/en/skills) — Claude Code Docs — the load-time distinction: a skill's body loads only when used; CLAUDE.md content is always resident. — 25 min

**Tutor prompt:**
> Teach me task statement 3.1. Give me the load order broad→specific and stress that files CONCATENATE rather than override — then quiz me on a case where a community guide claims "project CLAUDE.md overrides user CLAUDE.md" and tell me what is actually true. Cover `@path` imports (depth limit, resolution, why they do NOT reduce context) versus `.claude/rules/` with and without `paths:` frontmatter (which is the only thing that DOES reduce context). Then run the include/exclude drill: I'll give you candidate lines and you tell me keep or cut using "would removing this cause Claude to make mistakes?". Finish with what survives `/compact` — project-root CLAUDE.md is re-read from disk, nested subdirectory files are not.

### 3.2 — Slash commands & skills

> **Exam guide §17 names the frontmatter it tests:** `context: fork`, `allowed-tools`, **`argument-hint`**,
> plus **project vs user scope** for both `.claude/commands/` and `.claude/skills/`. Note the guide still
> lists `.claude/commands/` as its own surface — see the terminology-drift note below.

- [Extend Claude with skills](https://code.claude.com/docs/en/skills) — Claude Code Docs — **custom slash commands have been MERGED into skills.** `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` both create `/deploy`. `disable-model-invocation` for side-effecting workflows; running a skill in a subagent. — 25 min
- [Commands](https://code.claude.com/docs/en/commands) — Claude Code Docs — built-in commands vs skill-based commands: built-ins execute CLI logic and are never auto-invoked; skills execute via reasoning, declare `allowed-tools`, can be auto-invoked and can chain (up to 6). — 12 min
- [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — Anthropic Platform Docs — degrees of freedom (high/medium/low); descriptions must be THIRD PERSON because they're injected into the system prompt; ≤1,024 char description; SKILL.md body under 500 lines; references one level deep. — 35 min
- [Introduction to Agent Skills](https://anthropic.skilljar.com/introduction-to-agent-skills) — Anthropic Academy *(free signup)* — module 4 is explicitly the decision boundary between a skill, CLAUDE.md, hooks and subagents. — 60 min
- [Explore the .claude directory](https://code.claude.com/docs/en/claude-directory) — Claude Code Docs — "Commands and skills are now the same mechanism. For new workflows, use skills/ instead." — 10 min

> ⚠️ **Terminology drift:** community study guides and older blog posts still teach `.claude/commands/*.md` as a separate mechanism from skills. It isn't, as of current Claude Code. Old files keep working. Know both framings — an exam item may use either.

**Tutor prompt:**
> Teach me task statement 3.2. Start by resolving the terminology drift: explain that custom slash commands and skills are now the same mechanism, that `.claude/commands/*.md` still works, and what skills add (a directory for supporting files, frontmatter to control invocation, subagent execution, dynamic context injection). Then teach the decision boundary: when does a procedure belong in a skill vs CLAUDE.md vs a hook vs a subagent? Use the stated rule — "when a section of CLAUDE.md has grown into a procedure rather than a fact". Then quiz me on skill frontmatter: what `disable-model-invocation` is for, why the description must be third person, why there is exactly one description field, and why keeping references one level deep matters. Give me workflows and make me choose the mechanism.

### 3.3 — Path-specific rules

- [Claude Code settings](https://code.claude.com/docs/en/settings) — Claude Code Docs — **canonical.** Scope priority managed > CLI args > local > project > user; highest wins EXCEPT permission rules which MERGE; `ACTION(pattern)` syntax with gitignore-style globbing (documented on the *permissions* page); rules evaluated deny → ask → allow, first match wins, and **rule specificity does not change that order**. — 20 min
- [How Claude remembers your project](https://code.claude.com/docs/en/memory) — Claude Code Docs — `.claude/rules/*.md`: rules WITHOUT `paths:` load unconditionally at launch; rules WITH `paths:` globs load only when Claude reads a matching file. User rules load before project rules, so project rules win. — 30 min
- [Set up Claude Code in a monorepo or large codebase](https://code.claude.com/docs/en/large-codebases) — Claude Code Docs — the three-way decision table, plus `claudeMdExcludes` matching absolute paths (start relative-style patterns with `**/`), and that project `.claude/settings.json` is NOT inherited from parent directories the way CLAUDE.md is. — 25 min
- [Explore the .claude directory](https://code.claude.com/docs/en/claude-directory) — Claude Code Docs — the full file tree with load timing. — 10 min

**Tutor prompt:**
> Teach me task statement 3.3. Give me the three ways to scope guidance to a subset of a repo — per-directory CLAUDE.md, path-scoped `.claude/rules/` with `paths:` globs, per-directory skills — and for each: where it lives, exactly when it loads, and which situation it's designed for. Then teach permission-rule path syntax: `ACTION(pattern)` with gitignore globbing, `*` within a segment vs `**` across directories, `./` for repo-root-relative, and the precedence rule: evaluation runs deny → ask → allow and the first match wins — specificity does NOT promote a rule, so a narrow allow never beats a broad deny. Now quiz me on the traps: permission rules MERGE across scopes while everything else overrides; `.claude/settings.json` does not inherit from parent dirs; `claudeMdExcludes` matches absolute paths so a bare `*.md` won't match; managed policy CLAUDE.md can never be excluded.

### 3.4 — Plan mode vs direct execution

- [Choose a permission mode](https://code.claude.com/docs/en/permission-modes) — Claude Code Docs — **the** page. Full mode table (default/Manual, acceptEdits, plan, auto, dontAsk, bypassPermissions); plan mode is enforced by the tool layer, not Claude's judgment; four ways to enter it; protected paths; auto-mode fallback after 3 consecutive / 20 total classifier blocks, which resumes prompting — and in a headless `-p` run with no `--permission-prompt-tool`, skips the blocked action and lets the run continue rather than stopping it. — 25 min
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — Explore→Plan→Implement→Commit, with the explicit skip rule: "If you could describe the diff in one sentence, skip the plan." — 35 min
- [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works) — Claude Code Docs — Shift+Tab mode cycling and checkpoints (and what checkpoints cannot cover: remote side effects). — 25 min
- [Commands](https://code.claude.com/docs/en/commands) — Claude Code Docs — `/plan` prefixing a single prompt. — 12 min
- [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action) — Anthropic Academy *(free signup)* — the Permission Modes module. — 180 min

**Tutor prompt:**
> Teach me task statement 3.4. Build the mode table: for default/Manual, acceptEdits, plan, auto, dontAsk and bypassPermissions, state exactly what is auto-approved and what still prompts. Emphasise that plan mode is enforced by the TOOL LAYER, not by Claude choosing to behave. Then give me the "when to plan" heuristic and, crucially, the "when to skip" heuristic — and quiz me with six change requests where I must decide plan vs direct execution and justify on uncertainty, file count and familiarity. Finish with two traps: why `dontAsk` is the right CI mode and what it does to `AskUserQuestion`; and what repeated classifier blocks actually do — auto mode pauses into prompting, and a headless `-p` run with no `--permission-prompt-tool` skips the blocked action and keeps going instead of stopping.

### 3.5 — Iterative refinement

> ⚠️ No dedicated docs page exists for this task statement — it is taught across the three sources below.
> The *guide*, however, is specific about what it tests.

> **Exam guide §17 names four things for 3.5 that no source below covers:** **input/output examples**
> (§6 asks for 2–3 concrete ones where prose descriptions are interpreted inconsistently) ·
> **test-driven iteration** (write tests first, share failures, iterate) · the **interview pattern**
> (have Claude interview you about requirements and failure modes before implementing, in unfamiliar
> domains) · **sequential vs parallel issue resolution** (batch interacting issues together, fix
> independent ones sequentially). Teach these from the guide — there is no doc to read.

- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — the named failure patterns: "correcting over and over" (after two failed corrections, `/clear` and rewrite the prompt), the kitchen-sink session, the trust-then-verify gap, infinite exploration. Plus the four-rung verification ladder. — 35 min
- [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — Anthropic Platform Docs — evaluation-driven development: build evals BEFORE the docs; baseline without the skill; the Claude-A-authors / Claude-B-uses iteration loop; refine from observed failures. — 35 min
- [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action) — Anthropic Academy *(free signup)* — "Steering Long Sessions" and "Trust It: Verifying Unsupervised Runs". — 180 min

**Tutor prompt:**
> Teach me task statement 3.5. There is no single canonical page, so synthesise from the three sources I give you. Cover: course-correct early rather than late; the two-failed-corrections rule and why re-prompting beats a third correction; `/clear` vs `/compact` as refinement tools; the four-rung verification ladder (same-prompt check → `/goal` evaluator → Stop hook → verification subagent) and when to climb it. Then teach evaluation-driven iteration: baseline first, minimal instructions, iterate from observed failures, and the two-instance authoring loop. Quiz me with situations where a session has gone wrong and make me pick between correcting again, clearing and re-prompting, compacting with a focus, or escalating to a gate.

### 3.6 — Claude Code in CI/CD (-p, --json-schema)

- [Run Claude Code programmatically](https://code.claude.com/docs/en/headless) — Claude Code Docs — **the** page. `-p`/`--print`, `--bare` for reproducible CI, `--output-format text|json|stream-json`, `--json-schema` putting the validated value in `structured_output`, `--allowedTools`, `dontAsk`/`acceptEdits` modes, `--continue`/`--resume`, SIGTERM → exit 143, and `system/init` exposing `plugin_errors`/`mcp_server_errors` so a gate can fail on a non-empty array. — 30 min
- [Claude Code GitHub Actions](https://code.claude.com/docs/en/github-actions) — Claude Code Docs — `anthropics/claude-code-action@v1`, auto-detection of interactive vs automation mode, `claude_args` passthrough, `--max-turns` cost control. — 20 min
- [Claude Code GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd) — Claude Code Docs — the concrete job: `claude -p "…" --permission-mode acceptEdits --allowedTools "Bash Read Edit Write mcp__gitlab"`. — 15 min
- [Get structured output from agents (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/structured-outputs) — Claude Code Docs — schema validation with built-in re-prompting, and the trap where `subtype === 'success'` but `structured_output` is absent. — 25 min
- [Choose a permission mode](https://code.claude.com/docs/en/permission-modes) — Claude Code Docs — `dontAsk` as the locked-down CI mode; auto mode continuing in headless. — 25 min
- [Find bugs with ultrareview](https://code.claude.com/docs/en/ultrareview) — Claude Code Docs — `claude ultrareview` as a non-interactive subcommand: exit 0/1/130, `--json`, progress to stderr so stdout stays parseable. *(docs free; running it is a paid feature — read for the pattern)* — 10 min
- [I passed CCAR-F with 893/1000](https://medium.com/@kishorkukreja/i-passed-anthropics-claude-certified-architect-foundations-exam-with-a-score-of-893-1000-2206c27efd6c) — Medium: Kishor Kukreja — the `-p` flag runs Claude Code non-interactively; without it, pipelines hang. — 10 min

**Tutor prompt:**
> Teach me task statement 3.6. Start with the single most-tested fact: `-p` is mandatory in CI because without it the job hangs waiting for input. Then teach the flag surface: `--bare` and why reproducibility demands it, the three output formats and what each returns, `--json-schema` + `--output-format json` landing the validated value in `structured_output`, `--allowedTools` using permission-rule syntax (mind the space in `Bash(git diff *)`), and which permission mode to pick for an unattended run. Quiz me on failure design: how do I make CI fail when an MCP server didn't load; what does SIGTERM do and what exit code; what happens to auto mode in headless when the classifier blocks repeatedly; and what's the trap in treating `subtype === 'success'` as success. End with one GitHub Actions and one GitLab job I could defend in an interview.

---

## Domain 4 — Prompt Engineering & Structured Output (20%)

### 4.1 — Explicit criteria / false positives

- [Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — Anthropic Platform Docs — give Claude explicit permission to say "I don't know"; direct-quote grounding for long documents; verify-with-citations and remove unsupported claims; chain-of-thought verification; best-of-N. — 15 min
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — the adversarial-review caveat, which IS the false-positive lesson: "A reviewer prompted to find gaps will usually report some, even when the work is sound… Tell the reviewer to flag only gaps that affect correctness or the stated requirements." — 35 min
- [Increase output consistency](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency) — Anthropic Platform Docs — specify the exact output format; constrain with examples; ground in retrieval. — 14 min
- [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) — Anthropic Platform Docs — "Be clear and direct", XML tags to separate instruction types, overeagerness. — 40 min
- [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — Anthropic Platform Docs — degrees of freedom: how tightly to specify a task based on how fragile it is. — 35 min
- [Find bugs with ultrareview](https://code.claude.com/docs/en/ultrareview) — Claude Code Docs — "every reported finding is independently reproduced and verified, so the results focus on real bugs rather than style suggestions." — 10 min

**Tutor prompt:**
> Teach me task statement 4.1 as false-positive suppression. Take a real scenario: a code-review agent reports 40 issues per PR and the team has stopped reading them. Walk me through every lever: stating explicit criteria for what counts as a finding, giving permission to return zero findings, requiring evidence (a quote / a reproduction) for each claim, and constraining scope to correctness and stated requirements. Explain WHY an adversarially-prompted reviewer manufactures findings. Then quiz me on prompts and make me identify which one will over-report and rewrite it. Finish by naming the difference between "be more accurate" (useless) and an explicit criterion (useful).

### 4.2 — Few-shot prompting

- [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) — Anthropic Platform Docs — 3–5 examples; relevant, diverse, STRUCTURED; wrap each in `<example>` and the set in `<examples>`; put `<thinking>` inside examples to demonstrate the reasoning pattern; put longform data at the TOP of the prompt. — 40 min
- [Increase output consistency](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency) — Anthropic Platform Docs — "constrain with examples" stated as **more effective than abstract instructions**. Also the trap: prefilling is NOT supported on Claude 4.6+. — 14 min
- [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — Anthropic Platform Docs — the examples pattern as few-shot input/output pairs inside a skill. — 35 min
- [Anthropic's Interactive Prompt Engineering Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial) — GitHub: anthropics — Chapter 7 "Using Examples" with exercises and an answer key. *(model-specific claims are dated to Claude 3; techniques hold)* — 120 min
- [Building with the Claude API](https://anthropic.skilljar.com/claude-with-the-anthropic-api) — Anthropic Academy *(free signup)* — the "Prompt Engineering Techniques" section: XML tags and providing examples. — 480 min

**Tutor prompt:**
> Teach me task statement 4.2. State the concrete guidance: how many examples, what makes them good (relevant, diverse, structured), why to wrap them in `<example>`/`<examples>` tags, and where examples sit relative to longform data in a long-context prompt. Explain why examples beat abstract instructions. Then give me a classification task with an inconsistent output and make me fix it three ways — more instructions, few-shot examples, and a constrained schema — and rank them by reliability for that task. Quiz me on two traps: when few-shot is the WRONG answer (when you need guaranteed schema compliance — use structured outputs), and the fact that assistant prefilling is not supported on Claude 4.6+ even though older material teaches it.

### 4.3 — Structured output via tool_use + JSON schema

> **Exam guide §17 also names:** **nullable/optional fields to prevent hallucination** · the
> **`"other"` + detail-string pattern** for open-ended categories · **strict mode to eliminate syntax
> errors** · enum types. These are schema *design* judgments, not API surface — expect items about
> which schema shape stops a model inventing a value.

- [Structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs) — Anthropic Platform Docs — the two mechanisms: `output_config.format` constrains the response, `strict: true` guarantees tool arguments. The supported JSON Schema subset and what is NOT supported (recursive schemas, external `$ref`, `additionalProperties` other than false, and string/number bounds like `minLength` or `minimum`). Note `minItems` *is* supported, but only for values 0 and 1 — so `minItems: 2` is not. — 25 min
- [Get structured output from agents (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/structured-outputs) — Claude Code Docs — DRAFT-07 validation (Zod must be converted with `target: 'draft-7'`), result subtypes, and the exam nuance: "A result can also end with subtype success but no `structured_output` value… Treat that case as a failure as well." — 25 min
- [Strict tool use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/strict-tool-use) — Anthropic Platform Docs — the guaranteed-conformance lever and the schema subset it requires. *(Note: the fact that `pattern` is outside the supported subset is stated on the* troubleshooting-tool-use *page, not this one.)* — 10 min
- [Increase output consistency](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency) — Anthropic Platform Docs — the discriminating rule: "If you need Claude to always output valid JSON that conforms to a specific schema, use Structured Outputs instead of the prompt engineering techniques below." — 14 min
- [Run Claude Code programmatically](https://code.claude.com/docs/en/headless) — Claude Code Docs — `--json-schema` in CI; an invalid schema now exits with a validator diagnostic rather than being silently ignored. — 30 min
- [Define tools](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools) — Anthropic Platform Docs — `tool_choice: any` + `strict: true` guarantees both that a tool is called and that inputs conform. — 25 min

**Tutor prompt:**
> Teach me task statement 4.3. Separate the mechanisms cleanly: prompt-and-parse, `output_config.format` with a JSON schema, `strict: true` on a tool, `--json-schema` in headless, and the Agent SDK `outputFormat`. For each: what it guarantees, what it does not, and where the validated value shows up. Then drill the supported schema subset — make me classify schema features as supported or not (enum of objects, `minimum`, `minLength`, `minItems: 2`, recursive `$ref`, `additionalProperties: true`, `const`, `anyOf`, `format: uuid`, external `$ref` URL). Finish with the two traps: draft-07 vs Zod's default 2020-12, and why `subtype === 'success'` with a missing `structured_output` must be treated as a failure.

### 4.4 — Validation, retry & feedback loops

> **Exam guide §17 also names, and no listed source covers it:** **Pydantic** — schema validation,
> *semantic* validation errors (as distinct from syntactic), and validation-retry loops. If you don't
> know Pydantic, build one extraction pipeline with it; the exam expects the semantic-vs-syntactic
> distinction.

- [Get structured output from agents (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/structured-outputs) — Claude Code Docs — "The SDK validates the output against it, re-prompting on mismatch" — the retry loop is built in. `error_max_structured_output_retries` and how to tell the two causes apart via the `errors` list. — 25 min
- [Structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs) — Anthropic Platform Docs — with a constrained schema there are no schema-violation retries at all; prompt-then-parse-then-retry is correct only when the features you need aren't supported. — 25 min
- [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — Anthropic Platform Docs — the validator feedback loop ("only proceed when validation passes") and plan-validate-execute for batch/destructive/high-stakes operations. — 35 min
- [Keep Claude working toward a goal (/goal)](https://code.claude.com/docs/en/goal) — Claude Code Docs — a feedback loop where a fresh evaluator model decides whether the exit criterion is met; conditions must be provable from the transcript. — 14 min
- [claude-cookbooks: patterns/agents](https://github.com/anthropics/claude-cookbooks/tree/main/patterns/agents) — GitHub: anthropics — `evaluator_optimizer.ipynb` is the runnable code form of this task statement. — 60 min
- [Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — Anthropic Platform Docs — iterative refinement: feed outputs back as inputs asking Claude to verify or expand. — 15 min

**Tutor prompt:**
> Teach me task statement 4.4. Draw the decision tree first: when is a retry loop unnecessary (constrained schema guarantees conformance), when is it necessary (semantic validation, business rules, unsupported schema features), and when should the loop be a separate evaluator instead of a self-check. Then teach the mechanics: what the SDK retries automatically, what error subtype you get when retries are exhausted, and how to distinguish validation failure from model-fallback retraction. Quiz me on validation requirements — "must be valid JSON", "the total must equal the sum of line items", "the date must be in the future", "the category must be one of six", "the summary must be supported by the source" — and for each make me choose schema constraint, code validation, evaluator model, or human review. Finish with the plan-validate-execute pattern and why it beats direct execution for destructive batch work.

### 4.5 — Batch processing (Message Batches API)

> ⚠️ Thin by necessity — the docs page below is essentially the only quality source. Everything else found was SEO tutorial content.

> **Exam guide §17 adds one fact no listed source states plainly:** the Message Batches API has
> **no multi-turn tool calling support**. It also names `custom_id` for request/response correlation,
> polling for completion, latency-tolerance assessment, and failure handling *by* `custom_id`.

- [Batch processing (Message Batches API)](https://platform.claude.com/docs/en/build-with-claude/batch-processing) — Anthropic Platform Docs — **the** source. 50% discount; 100,000 requests OR 256 MB per batch; most complete under 1 hour; results at completion or 24h whichever first; batches EXPIRE at 24h (expired requests not billed); results downloadable for 29 days; unique `custom_id` per request; `max_tokens >= 1` required; stream results from `results_url` as JSONL; per-request result types succeeded/errored/expired. — 25 min
- [How I Passed the CCA-F Exam](https://medium.com/@johnelanji/how-i-passed-the-anthropic-claude-certified-architect-foundations-cca-f-exam-a-complete-1108dce46e9b) — Medium: John Mathew — the tradeoff the exam actually tests: cost savings cost latency (hours); user-facing, CI/CD and approval workflows need the synchronous API. — 15 min

**Tutor prompt:**
> Teach me task statement 4.5. Give me the hard numbers as a fact sheet: discount, the two size limits, typical and maximum completion time, expiry semantics and billing on expiry, result retention, the `custom_id` requirement and what it's for, and the `max_tokens` constraint. Then — this is the part the exam actually tests — give me workloads (nightly document classification, a live chat assistant, a CI PR review gate, a 200k-row backfill, an eval harness run, a user-facing search summariser, an approval workflow, a weekly report) and make me choose batch vs synchronous and defend it on latency, cost and failure handling. Punish any answer that picks batch for something a human is waiting on.

### 4.6 — Multi-instance / multi-pass review

- [Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows) — Claude Code Docs — **best source.** "have independent agents adversarially review each other's findings before they're reported"; `/deep-research` votes on claims and reports unverifiable ones as UNVERIFIED rather than refuted. — 30 min
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — the Writer/Reviewer two-session pattern (a fresh context reviews better because it isn't biased toward code it just wrote) and the adversarial-review over-reporting caveat. — 35 min
- [Find bugs with ultrareview](https://code.claude.com/docs/en/ultrareview) — Claude Code Docs — a fleet of reviewers whose findings are independently reproduced and verified; the three-way comparison against single-pass `/code-review` and `/review <pr>`. — 10 min
- [Run agents in parallel](https://code.claude.com/docs/en/agents) — Claude Code Docs — when the job outgrows a handful of subagents and findings need cross-checking. — 12 min
- [Run parallel sessions with worktrees](https://code.claude.com/docs/en/worktrees) — Claude Code Docs — the file-isolation half: `isolation: worktree` on a subagent, `worktree.baseRef` fresh vs head. — 18 min
- [Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — Anthropic Platform Docs — best-of-N verification: inconsistency across independent runs signals hallucination. — 15 min
- [Lessons on building effective human-agent teams](https://claude.com/blog/building-effective-human-agent-teams) — Claude blog — the named Doer-Verifier pattern. — 20 min

**Tutor prompt:**
> Teach me task statement 4.6. Explain why a second pass in the SAME session is weaker than a second pass in a FRESH context, and name the bias. Then lay out the escalation ladder: self-check in the same prompt → Writer/Reviewer across two sessions → a verification subagent → a fleet with independent reproduction and cross-checking → best-of-N voting. For each, say what it costs and what it buys. Then quiz me on the two failure modes: over-reporting (an adversarial reviewer manufactures findings — what's the fix?) and unverifiable claims (what should a verifier do with a claim it could not check — count it as refuted, or report it as UNVERIFIED?). Give me scenarios and make me pick a rung on the ladder.

---

## Domain 5 — Context Management & Reliability (15%)

### 5.1 — Preserve critical info across long context

> **Exam guide §17 also names:** **lost-in-the-middle effects** · **position-aware input ordering** ·
> **scratchpad files** for long sessions · **structured fact extraction from verbose tool outputs** ·
> progressive summarization, which §17 names bare and §6's own 5.1 objective frames as a *risk* — it condenses numeric values, percentages, dates and customer-stated expectations into vague summaries.

- [Explore the context window](https://code.claude.com/docs/en/context-window) — Claude Code Docs — the interactive walkthrough with token costs for every startup item, and a "what survives compaction" breakdown (skill descriptions are flagged `noSurviveCompact`). — 25 min
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic Engineering — the six named techniques: compaction, structured note-taking, sub-agent architectures, just-in-time retrieval, system-prompt altitude, token-efficient tool design. — 25 min
- [Context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing) — Anthropic Platform Docs — `clear_tool_uses_20250919` with `trigger`/`keep`/`clear_at_least`/`exclude_tools`; paired with the memory tool so Claude writes important info to memory BEFORE results are cleared. — 20 min
- [How Claude remembers your project](https://code.claude.com/docs/en/memory) — Claude Code Docs — project-root CLAUDE.md survives `/compact` (re-read from disk); nested subdirectory files are not re-injected. — 30 min
- [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works) — Claude Code Docs — context fills → older TOOL OUTPUTS cleared first, then the conversation is summarized; detailed early instructions may be lost. — 25 min
- [How the agent loop works (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/agent-loop) — Claude Code Docs — "Persistent rules belong in CLAUDE.md rather than in the initial prompt, because CLAUDE.md content is re-injected on every request." — 25 min
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — "performance degrades as context fills"; `/clear` between unrelated tasks. — 35 min

**Tutor prompt:**
> Teach me task statement 5.1. Start with the mechanics: what fills a context window, what gets evicted FIRST when it's full (tool outputs), what happens next (summarization), and what specifically does not survive. Then teach the six context-engineering techniques by name, each with the situation it solves. Now quiz me on placement: I have an instruction that must hold for the next four hours of work — where do I put it, and why is "the initial prompt" wrong? Give me pieces of information (a coding standard, a one-off correction, a 900-line API spec, the current bug's repro steps, an architectural decision, a list of files to ignore) and make me place each in CLAUDE.md, a path-scoped rule, a skill, a memory/notes file, a subagent's delegation prompt, or nowhere. Finish with `/compact <focus>` vs `/clear` vs delegating the read to a subagent.

### 5.2 — Escalation & ambiguity resolution

> **Exam guide §6 names the trigger criteria directly:** escalate on an explicit customer request for a
> human, on policy gaps or exceptions, and on inability to make progress. It names **sentiment analysis
> and model self-rated confidence as unreliable** triggers — a customer's mood is not case complexity,
> and self-reported confidence is uncalibrated. §17 adds *"honoring customer preferences"*. Contrast
> with 5.5, where *calibrated* field-level confidence is a legitimate review-routing signal.

- [Escalate hard decisions with the advisor tool](https://code.claude.com/docs/en/advisor) — Claude Code Docs — **best first-party page.** The advisor receives the FULL conversation and returns guidance; the capability check (advisor must be at least as capable as the main model); Claude decides WHEN to call it; Claude "adapts when its own evidence contradicts a specific claim" and surfaces the conflict rather than obeying. — 12 min
- [Handle approvals and user input (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/user-input) — Claude Code Docs — `AskUserQuestion`: 1–4 questions per call, 2–4 options each, 12-char header, `multiSelect`; the response taxonomy (approve / approve-with-changes / approve-and-remember / reject with a message / suggest-alternative / redirect); and **not available in subagents**. — 18 min
- [Lessons on building effective human-agent teams](https://claude.com/blog/building-effective-human-agent-teams) — Claude blog — teach agents to surface hard-tradeoff decisions directly, "ensuring that decisions with hard tradeoffs always had a human in the loop"; batch agent communications to protect human attention. — 20 min
- [The advisor strategy](https://claude.com/blog/the-advisor-strategy) — Claude blog — the rationale and benchmarks: Sonnet + Opus advisor gained 2.7pp on SWE-bench Multilingual while cutting cost per task 11.9%; the inversion of traditional orchestration. — 8 min
- [Advisor tool (Messages API)](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool) — Anthropic Platform Docs — the API-level definition. *(medium confidence — not opened directly)* — 10 min

**Tutor prompt:**
> Teach me task statement 5.2. Distinguish three kinds of escalation and the mechanism for each: (a) the agent is uncertain about an approach → advisor tool, a stronger model consulted mid-task; (b) the requirement is genuinely ambiguous and only a human knows → `AskUserQuestion`; (c) the action is irreversible → a permission gate or hook, not a question. Then quiz me on the constraints: what happens when a SUBAGENT needs clarification (it can't call AskUserQuestion — what's the correct pattern?); what does Claude do when advisor guidance contradicts its own evidence; and what capability relationship must hold between main model and advisor. Finish with the cost argument from the advisor-strategy benchmarks: why is "weak executor + strong advisor" sometimes better than "strong model throughout"?

### 5.3 — Error propagation across multi-agent

- [Handling stop reasons](https://platform.claude.com/docs/en/build-with-claude/handling-stop-reasons) — Anthropic Platform Docs — distinguish `stop_reason` (normal completion signalling) from HTTP errors (request failures); `refusal` is an HTTP 200, not an error. — 15 min
- [Handle tool calls](https://platform.claude.com/docs/en/agents-and-tools/tool-use/handle-tool-calls) — Anthropic Platform Docs — `is_error: true` lets Claude recover; a silent empty result masks a failure. — 20 min
- [Hooks reference](https://code.claude.com/docs/en/hooks) — Claude Code Docs — `PostToolUseFailure`, `StopFailure`, `SubagentStop`, and exit code 2 turning stderr into the rejection reason the model reads. — 35 min
- [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) — Anthropic Engineering — durable execution with checkpoints so long-running agents resume rather than restart; high-level tracing without reading conversation contents; rainbow deployments so in-flight agents aren't disrupted. — 35 min
- [Building a C compiler with a team of parallel Claudes](https://www.anthropic.com/engineering/building-c-compiler) — Anthropic Engineering — tests should emit ERROR markers on single parseable lines plus aggregate summary statistics, not verbose raw output; agents solve whatever the verification defines. — 25 min
- [Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows) — Claude Code Docs — resume replay is order-based: cached results stop at the first agent that didn't finish, so many small agents preserve more progress than one long one. — 30 min
- [Troubleshooting tool use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/troubleshooting-tool-use) — Anthropic Platform Docs — malformed `tool_result` sequences and the request-time errors they produce. — 12 min

**Tutor prompt:**
> Teach me task statement 5.3. Trace one error from origin to coordinator: a tool inside a subagent hits an HTTP 500. Show me every point where information can be lost — the tool returning an empty result instead of `is_error`, the subagent summarising away the failure, the coordinator treating a returned summary as success, a hook that can't block because it's PostToolUse. Then teach the fixes: instructive `is_error` payloads, structured "obstacle" reporting from subagents, checkpointing so a failure doesn't restart everything, and parseable single-line error markers instead of verbose output. Quiz me on the silent-failure family — for five outputs (empty array, "no results found", a summary with no mention of the error, a 200 with `refusal`, a partial result) make me say whether the coordinator can tell something went wrong and what to change.

### 5.4 — Context in large codebase exploration

> **Exam guide §17 names the `/memory` command, `/compact`, and the `Explore` subagent** as Claude Code
> surfaces that may appear. Know what each does to the context window. §6's 5.4 objective also names
> **crash recovery using structured agent state exports (manifests) that the coordinator loads on
> resume** — the same mechanism cross-referenced at 1.3.

- [Set up Claude Code in a monorepo or large codebase](https://code.claude.com/docs/en/large-codebases) — Claude Code Docs — **the** page. Where you start determines what loads; `claudeMdExcludes`; `permissions.deny` Read rules for vendored code (and their limit: they don't filter denied paths out of recursive search output); `worktree.sparsePaths`; the `additionalDirectories` vs `--add-dir` access table. — 25 min
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic Engineering — just-in-time retrieval: hold lightweight identifiers, load content at runtime. The direct answer for this statement. — 25 min
- [Create custom subagents](https://code.claude.com/docs/en/sub-agents) — Claude Code Docs — use subagents to isolate verbose output; the warning that many subagents each returning detailed results also consumes significant context. — 35 min
- [Explore the context window](https://code.claude.com/docs/en/context-window) — Claude Code Docs — delegate large reads so file contents land in the subagent's window, not yours. — 25 min
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — the "infinite exploration" failure pattern: scope it or use subagents. — 35 min
- [Introduction to Agent Skills](https://anthropic.skilljar.com/introduction-to-agent-skills) — Anthropic Academy *(free signup)* — progressive disclosure as a context-budget technique. — 60 min

**Tutor prompt:**
> Teach me task statement 5.4. Scenario: a 2M-line monorepo, and I need Claude to find every place a deprecated auth helper is used and propose a migration. Walk me through the context strategy: where I launch the session and why that choice alone changes what loads; how I keep vendored and generated code out (`permissions.deny` Read rules, `claudeMdExcludes`, sparse worktrees) and what each does NOT cover; when to delegate exploration to a subagent and what it should return; and why just-in-time retrieval beats pre-loading. Then quiz me on the anti-patterns: reading 40 files "to be safe", spawning 12 subagents that each return 8,000 tokens, and putting the whole architecture doc in CLAUDE.md. For each, tell me the symptom and the fix.

### 5.5 — Human review & confidence calibration

> ⚠️ **Weakest-served statement in the *docs* — but not in the blueprint.** No first-party Anthropic
> documentation page targets confidence calibration; searches returned only content farms. However
> **Exam guide §17 names precisely what is tested**, so treat this as the syllabus and assemble the
> teaching from the sources below:
> - **field-level confidence** (not a single whole-output score)
> - **calibration with labeled validation sets**
> - **stratified sampling for error-rate measurement**
> - **accuracy segmentation by document type and field**
> 
> The reliable-versus-unreliable **escalation trigger** list lives at **objective 5.2**, not here — see
> that statement.

- [Lessons on building effective human-agent teams](https://claude.com/blog/building-effective-human-agent-teams) — Claude blog — **closest thing to canon.** Autonomy granted in proportion to demonstrated reliability, then expanded deliberately; "the best long-running agents have many different ways to verify their work before a human looks at it"; the Doer-Verifier pattern; batching agent communications; recalibrating guardrails as models improve. — 20 min
- [Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — Anthropic Platform Docs — explicit permission to say "I don't have enough information to confidently assess this"; best-of-N inconsistency as a hallucination signal; the caveat that these techniques reduce but don't eliminate. — 15 min
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices) — Claude Code Docs — the four-rung verification ladder and the named "trust-then-verify gap" failure pattern. — 35 min
- [Building a C compiler with a team of parallel Claudes](https://www.anthropic.com/engineering/building-c-compiler) — Anthropic Engineering — "it is easy to see tests pass and assume the job is done, when this is rarely the case"; verifier quality is load-bearing because agents solve whatever verification defines. — 25 min
- [Find bugs with ultrareview](https://code.claude.com/docs/en/ultrareview) — Claude Code Docs — independent reproduction and verification as the mechanism that makes findings worth a human's attention. — 10 min
- [Handle approvals and user input (Agent SDK)](https://code.claude.com/docs/en/agent-sdk/user-input) — Claude Code Docs — the approval taxonomy, including approve-with-changes and approve-and-remember. — 18 min

**Tutor prompt:**
> Teach me task statement 5.5. This is thinly documented, so synthesise. Cover: (1) graduated autonomy — how a team moves from reviewing everything to dispatching agents unsupervised, and what evidence justifies each expansion; (2) verification layers that run BEFORE a human looks, so human attention is spent on judgment not triage; (3) the Doer-Verifier pattern; (4) calibration — giving the model explicit permission to admit uncertainty, and using cross-run inconsistency as a confidence signal; (5) why "tests pass" is not "the job is done" and why the verifier's quality determines what the agent optimises for. Then quiz me: given an agent with a 92% success rate on a task type, what should the human review policy be, and what would change it? Include the trap where over-tight guardrails constrain a newer, better model.

### 5.6 — Provenance & uncertainty in synthesis

> **Exam guide §17 also names:** **claim-source mappings** · **temporal data handling** ·
> **conflict annotation** when sources disagree · **coverage gap reporting**.

- [Search results (content blocks with source attribution)](https://platform.claude.com/docs/en/build-with-claude/search-results) — Anthropic Platform Docs — **the concrete mechanism.** `search_result` blocks — `source`, `title` and `content` are required; `citations: {enabled: true}` is **optional and off by default**, so you must switch it on. Blocks can be returned from your own tool or supplied in a user message. Once enabled, citations appear automatically; no special prompting needed. *(~81KB page — fetch with a narrow prompt)* — 12 min
- [Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — Anthropic Platform Docs — extract word-for-word quotes FIRST and analyse only from them; after drafting, find a supporting quote for every claim and REMOVE any claim it can't support. — 15 min
- [Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows) — Claude Code Docs — `/deep-research` votes on each claim, filters those that didn't survive, and reports claims the verifiers couldn't check as **UNVERIFIED rather than refuted**. — 30 min
- [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) — Anthropic Engineering — a separate citation agent running after synthesis. — 35 min
- [Increase output consistency](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency) — Anthropic Platform Docs — an output format that forces the model to name the knowledge-base entry it used: a cheap provenance pattern. — 14 min

**Tutor prompt:**
> Teach me task statement 5.6. Scenario: a research pipeline where subagents gather sources and a synthesis agent writes the summary, and stakeholders complain they can't tell which claim came from where. Give me the mechanisms in order of strength: an output schema that requires a source field per claim, quote-extraction-before-analysis, `search_result` content blocks with citations enabled, and a separate post-synthesis citation agent. For each, say what it guarantees. Then teach the three-state uncertainty model — supported / refuted / UNVERIFIED — and why collapsing "couldn't check" into "refuted" is a real bug. Quiz me with four exam-style options where three are prompt-level asks ("tell Claude to include sources") and one is a structural mechanism, and make me explain why the structural one wins.

---

# Official program

- **Claude Certified Architect – Foundations Exam Guide (PDF)** — Anthropic, v1.0 (July 2026) *(free signup)* — authoritative for *what is tested*: the 5 domains, all 30 task statements, 12 sample questions with answer rationales, and the appendix concept list. Bundled in this repo at `exam-guide/`. It also sits behind Skilljar registration on Anthropic's partner learning domain — register a free account there to check for a newer version. **The only officially-authored practice items in existence.** Do them last and treat any disagreement with a community answer key as settled in Anthropic's favour.
- **Anthropic Academy** — `anthropic.skilljar.com` *(free signup)* — the four free courses listed above under their relevant task statements (Claude Code in Action, Introduction to MCP, Introduction to Agent Skills, Building with the Claude API) are the official prep path.

# Wisdom (Communities)

> **There is no community for this exam.** No Reddit thread, no Hacker News thread, no findable
> X/Twitter thread, no Discord or forum. This was searched for specifically — don't spend time looking.
> The closest thing to practitioners talking is the three write-ups below.

- [I passed CCAR-F with 893/1000](https://medium.com/@kishorkukreja/i-passed-anthropics-claude-certified-architect-foundations-exam-with-a-score-of-893-1000-2206c27efd6c) — Medium: Kishor Kukreja — best short read for calibrating what a "correct" answer looks like; names the exam's anti-patterns explicitly. — 10 min
- [The CCA Exam: A Practical Guide with Real Code](https://medium.com/@sathishkraju/the-claude-certified-architect-exam-a-practical-guide-with-real-code-d01e123238ac) — Medium: Sathish Raju — most code-concrete; the enforcement-layer vs guidance-layer framing and the four error categories. — 15 min
- [How I Passed the Anthropic CCA-F Exam](https://medium.com/@johnelanji/how-i-passed-the-anthropic-claude-certified-architect-foundations-cca-f-exam-a-complete-1108dce46e9b) — Medium: John Mathew — reduces most of the exam to five recurring architectural patterns; reports the real exam is harder than the official practice test. — 15 min

# Terminology drift

Community guides and older posts are stale on all of these. An exam item may use either framing.

- The **Task tool was renamed Agent** in Claude Code v2.1.63. `Task(...)` still works as an alias, and SDKs still emit "Task" in the `system:init` tools list.
- **Custom slash commands and skills are now the same mechanism.** `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` both create `/deploy`.
- **`/subtask` is the forked subagent** that inherits the full conversation context. **`/fork` copies the whole session** into a new background session. Not the same thing.
- **"Agent view", "agent teams" and "dynamic workflows" are three distinct surfaces**, not synonyms for subagents.
- **Assistant prefilling is not supported on Claude 4.6 and later** — widely taught in older prompt-engineering material, and a likely distractor.
- **The count is 30, not 27** — confirmed against Exam Guide §6. Any guide, repo or note saying 27 is wrong.
- **`paullarionov/claude-certified-architect` (4.2k★) is wrong on three facts**: it says 4 scenarios out of **8** (§3 says 6), "multiple choice, 1 correct out of 4" (§3 says multiple-response items too), and **$99** (§3 says $125). It also claims "no guessing penalty", which the guide neither confirms nor denies — unverified rather than wrong. Its reasoning content is sound; do not trust its numbers.
- The **four error categories** (transient / validation / business / permission) come from the **exam guide's own 2.2 objective** — they are official. The **MCP spec** defines no categories, only the two mechanisms. Don't let a spec-first reading talk you out of them.

# Gaps

- **Concepts the exam guide names that no listed resource teaches.** Flagged inline at their task statements; collected here so nothing is lost: **Pydantic** (4.4) · **stratified sampling, labeled validation sets, field-level confidence, accuracy segmentation** (5.5) · **crash recovery using manifests** (1.3) · the **`"other"` + detail-string schema pattern** (4.3) · **temporal data handling and conflict annotation** (5.6). For these, the exam guide *is* the source — there is no doc to read.
- **5.5 (human review & confidence calibration)** — no first-party page targets it. Must be assembled from the five sources listed under 5.5.
- **3.5 (iterative refinement)** — no dedicated page anywhere; carried by best-practices, skill-authoring best practices, and the Claude Code in Action course.
- **4.5 (Message Batches API)** — exactly one authoritative source. It is enough, since the exam tests the batch-vs-synchronous tradeoff rather than the API surface.
- **No Messages API reference is listed.** Beware `docs.anthropic.com/en/api/complete`, which several older guides cite — it is the legacy Text Completions endpoint, not Messages. Verify a current URL before relying on one.
- **No normative MCP *resources* spec is listed** — only the Tools spec. `modelcontextprotocol.io/docs/learn/server-concepts` covers resources well enough for 2.4; find a current spec URL if you need protocol-level detail, and note that `spec.modelcontextprotocol.io` is a stale host.
- **Three pages carry a medium-confidence marker** — `manage-tool-context`, `programmatic-tool-calling` and `advisor-tool`. All three are live; their annotations are less thoroughly checked than the rest, so confirm a specific detail on the page before leaning on it. (`strict-tool-use` carries a different caveat: the relevant fact is stated on the troubleshooting-tool-use page, not that one.)
- **Practice material** — unofficial question banks and mock exams exist, but they are not listed here: this file grounds a tutor's *teaching*, and practice material is for self-testing. Their answer keys are also unreliable — where one disagrees with the exam guide or the official docs, the key is wrong.

# Out of scope — do not study

The exam guide lists these explicitly as **not appearing on the exam** (§17). Reproduced in full,
because the most expensive mistake in prep is studying something that cannot be tested.

- Fine-tuning Claude models or training custom models
- Claude API authentication, billing, or account management
- Detailed implementation of specific programming languages or frameworks (beyond what's needed for
  tool and schema configuration)
- Deploying or hosting MCP servers (infrastructure, networking, container orchestration)
- Claude's internal architecture, training process, or model weights
- Constitutional AI, RLHF, or safety training methodologies
- Embedding models or vector database implementation details
- Computer use (browser automation, desktop interaction)
- Vision / image analysis capabilities
- Streaming API implementation or server-sent events
- Rate limiting, quotas, or API pricing calculations
- OAuth, API key rotation, or authentication protocol details
- Specific cloud provider configurations (AWS, GCP, Azure)
- Performance benchmarking or model comparison metrics
- Prompt caching implementation details (beyond knowing it exists)
- Token counting algorithms or tokenization specifics

Also not tested, though not in §17's list: exam logistics — registration, scheduling, policies, the
NDA, appeals, and recertification.
