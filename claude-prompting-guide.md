# Prompting Guide for Claude

A practical guide for getting good results from the Claude chat app and Claude Code. Start with the defaults, reach for the templates when the task warrants it, and iterate.

---

## Defaults Before Templates

A few habits that matter more than any template:

- **Start with a reasonable prompt and iterate.** A solid first prompt plus one or two follow-ups almost always beats a 30-minute prompt-engineering session. Reserve the long template for high-stakes one-shot tasks or prompts you'll reuse many times.
- **Upload files instead of pasting.** Claude reads PDFs, images, spreadsheets, and code files directly. For anything over a page or two, attach the file rather than dumping it into the prompt.
- **Use Projects for recurring work.** On claude.ai, a Project holds reference documents and persistent instructions, so you don't repaste context every session. If you find yourself copy-pasting the same background into prompt after prompt, that material belongs in Project instructions or files.
- **Use Artifacts for deliverables.** Long-form documents, code, and diagrams appear as editable Artifacts you can iterate on, rather than buried in the chat.
- **Turn on Extended Thinking for hard problems.** For complex reasoning, planning, or multi-step analysis, the Extended Thinking toggle helps more than any "think step by step" instruction in the prompt.

---

## Template 1 — Short Version (your default)

Use this for everyday tasks. Most of what you ask Claude to do should look like this.

```
# GOAL
[Action verb + exactly what you want.]

# CONTEXT
[Key facts, data, or background needed to complete the task.]

# REQUIREMENTS & OUTPUT
- [Constraint 1, e.g., Max 3 paragraphs]
- [Constraint 2, e.g., Use bullet points]
- [Constraint 3, e.g., Professional tone]
```

A `# ROLE` line is optional. It helps when you want a specific tone or persona ("respond as a friendly customer-support agent"), but it does not unlock hidden expertise. Claude already brings its full knowledge to the task — you don't need to tell it to be a senior engineer for it to act like one.

---

## Template 2 — Long Version (high-stakes or reusable prompts)

Use this when the task is complex, the output is high-stakes, or you'll reuse the prompt many times. **Do not use it for simple tasks** — it can make responses more verbose and meandering than they need to be.

```
# ROLE
You are a [role/expert/persona].
(In claude.ai, durable persona rules can also live in Project instructions
or custom instructions, rather than being repeated each prompt.)

# GOAL
[Clearly describe what you want.]

# CONTEXT
<context>
[Paste information, data, notes, examples, constraints.]
</context>

# REQUIREMENTS
- [Rule 1 — e.g., Write at a high-school reading level]
- [Rule 2 — e.g., Base all answers strictly on the provided context]
- [Rule 3]
- If information is missing or uncertain, say so explicitly.
- Do not invent facts.
- If the request is ambiguous in a way that materially changes
  the answer, ask one clarifying question before proceeding.

# OUTPUT FORMAT
## Summary
[Short answer]

## Detailed Analysis
[Main explanation]

## Recommendations / Next Steps
[Actionable suggestions]

## Risks or Uncertainties
[Limitations, assumptions, unknowns]
```

**Note on the "thinking process" block.** Earlier versions of this template included a `# THINKING PROCESS` section telling Claude to identify factors, weigh interpretations, and explain trade-offs. Skip it. Claude already reasons through problems, and for genuinely hard reasoning the Extended Thinking toggle does more than any prompt instruction. Only add explicit reasoning scaffolding when you actually want the reasoning visible in the output.

---

## Four Principles

**1. Specificity beats complexity.**
Weak: *"Give me a good answer."*
Strong: *"Give a concise technical explanation with one real-world example."*

**2. Structure beats fancy wording.**
A prompt with clear sections and headings outperforms an eloquent run-on paragraph. Markdown headers (`# GOAL`, `# CONTEXT`) and XML tags (`<context>...</context>`) both work — pick one and be consistent.

**3. Examples are powerful (few-shot).**
Two or three examples of the input-output pattern you want will steer Claude more reliably than three paragraphs describing it.

**4. Constraints improve quality.**
"Maximum 5 bullets," "explain for beginners," or "avoid jargon" force concrete choices and usually produce better output than open-ended requests.

---

## Hints & Best Practices

**Use markdown headings or XML tags.** Claude parses both reliably. For mixing instructions with large data inputs, XML tags like `<document>...</document>` or `<context>...</context>` are especially clear about what's data versus what's instruction.

**Put the most important instruction early.** Models pay closer attention to instructions near the top of a prompt. For very long prompts, you can repeat the critical constraint once at the end as a reminder — but only the most important one, not all of them.

**Separate data from instructions.** Don't interleave them. Put data inside tags or under a heading, and keep the instructions outside. This prevents Claude from treating part of your data as a new instruction.

**Use positive framing.** Tell Claude what TO do, not what to avoid. *"Use simple language a high schooler would understand"* works better than *"don't use jargon."* *"Be concise — limit to 3 sentences"* works better than *"don't be wordy."* Negative framing is fine as a backup for hard prohibitions ("never include real names in the output"), but make positive framing your default.

**Ask for trade-offs explicitly.**
Weak: *"Which is best?"*
Strong: *"Compare advantages, disadvantages, risks, and trade-offs."*

**Ask Claude to state its assumptions.** A single line — *"List any assumptions you're making"* — exposes hidden reasoning gaps and makes it easy to correct course.

**Scale your prompt to the task.** Match prompt length to task complexity. A quick question deserves a quick prompt. Heavy templates for trivial tasks waste your time and can produce worse output.

**Iterate rather than over-engineer the first prompt.** Send something reasonable, see what comes back, then refine. The exception is one-shot high-stakes work (a client deliverable) or prompts you'll reuse many times — those are worth investing in upfront.

---

## Claude-Specific Notes

**Claude rarely needs an "expert" role prompt for accuracy.** Role prompts help shape tone, voice, and persona. They do not unlock hidden capability. Skip the role line unless persona matters.

**Extended Thinking exists as a real feature.** On claude.ai, toggle "Extended thinking" for hard reasoning, math, complex planning, or multi-step analysis. On the API, it's a parameter on the request. This does more than any "think step by step" instruction.

**Projects hold persistent context.** Project instructions and project files act like a shared system prompt across every chat in that Project. Put durable context there — coding conventions, your writing style, recurring background — instead of repasting it.

**Artifacts are for deliverables.** Documents, long code, diagrams, and web apps render as editable Artifacts. You can iterate on them turn by turn without losing the previous version.

**File uploads work directly.** PDFs, images, spreadsheets (xlsx, csv), Word docs, and code files can be attached. For anything more than a few paragraphs, upload rather than paste.

---

## Claude Code: A Different Beast

Claude Code is a terminal-based coding agent, not a chat app. It can read your codebase, edit files, run commands, and execute multi-step workflows. The chat templates above don't fit it well — what matters here is scoping the task, giving Claude the right context, and using the extensibility features so you don't repeat yourself.

### Prompting Basics

**Be specific about scope.** Name the files to touch and the files to leave alone. *"Refactor the authentication module"* is too broad; *"In `src/auth/`, extract the token validation logic from `login.ts` into a new `validators.ts` file. Don't change anything outside `src/auth/`"* is workable.

**Ask Claude to read before writing.** *"Read the existing tests in `tests/`, then write a new test for X that follows the same patterns"* produces consistent code. *"Write a test for X"* often does not.

**Plan first, then implement.** For non-trivial changes, ask Claude to produce a plan and wait for approval before editing files. **Planning mode** (toggle with Shift+Tab) makes this explicit: Claude can read the codebase but cannot edit until you approve the plan. This catches misunderstandings before they touch your code.

**Concrete tasks work; vague aspirations don't.** *"Run the test suite and fix the failures"* is a real instruction. *"Make my code better"* isn't.

**Use `/clear` between unrelated tasks.** Starting a new task with a fresh context window produces sharper results than letting the conversation grow across topics.

---

### `CLAUDE.md` — Persistent Project Context

Project-wide rules — build commands, code style, "always run the linter before claiming you're done" — belong in a `CLAUDE.md` file at the repo root. Claude Code reads it automatically at session start, every session.

A few guidelines:

- **Run `/init` to bootstrap one.** In a new project, `/init` creates a starter `CLAUDE.md` from your codebase. Edit it down to what's actually useful.
- **Keep it lean.** Roughly 80–120 lines is the practical sweet spot. Every line competes with the next for Claude's attention.
- **Put facts, not personality.** "Use pnpm, not npm" or "all API routes go in `src/routes/`" are useful. "Be a senior engineer who thinks step by step" is not — Claude already does that.
- **Layer it.** `CLAUDE.md` files can live globally (`~/.claude/CLAUDE.md`), per-project (repo root), or in subdirectories (only loaded when Claude reads files there). Personal preferences go in `CLAUDE.local.md` and should be gitignored.

---

### The Extensibility Stack

Claude Code has several extension mechanisms, each solving a different problem. Don't try to learn them all at once — most teams get 80% of the value from just `CLAUDE.md` plus skills. Here they are in order of how often you'll likely reach for them:

**Skills — reusable workflows.** A skill is a `SKILL.md` file in `.claude/skills/<name>/` that teaches Claude a repeatable procedure (e.g., "how to review a pull request in our team's style"). Claude can invoke a skill automatically when its description matches the task, or you can call it explicitly with `/skill-name`. *Use a skill when you find yourself pasting the same multi-step instructions into every new conversation.* This is usually the first extension feature to learn after `CLAUDE.md`.

**Subagents — isolated workers.** A subagent is a specialized Claude instance with its own context window, defined in `.claude/agents/<name>.md`. It does heavy or noisy work (searching the codebase, reading logs, running tests) and returns only a summary to your main session — its verbose output never pollutes your main context. *Use a subagent when a task would dump a lot of intermediate output into your conversation, or when you want to run several things in parallel.* You can also assign cheaper models (like Haiku) to subagents doing simple grunt work.

**Hooks — deterministic guardrails.** Hooks are shell commands that fire automatically at lifecycle events: before a tool runs, after a file is written, when a session starts, and so on. Configured in `settings.json`. *Use a hook when you need a guarantee, not a suggestion.* Examples: auto-run the linter after every file write, block `rm -rf` before it executes, refuse to read `.env` files, send a Slack notification when a long task finishes. Unlike `CLAUDE.md` rules (which Claude may forget mid-task), hooks always fire.

**MCP servers — external tools.** The Model Context Protocol lets Claude Code connect to external systems: GitHub, databases, Jira, browser automation (Playwright), your company's internal APIs. Once connected, Claude can query and act on them directly. *Use an MCP server when Claude needs to reach outside the local codebase.* Many official integrations exist for popular services.

**Slash commands — typed shortcuts.** Custom commands you invoke by typing `/name` in the terminal. Useful for repeatable workflows you want a clean entry point for (e.g., `/deploy`, `/review-pr`). Functionally these now overlap heavily with skills — recent Claude Code versions treat them as essentially the same thing. *Start with skills; slash commands are a packaging choice.*

**Plugins — bundles of the above.** A plugin packages skills, subagents, hooks, MCP servers, and slash commands into a single installable unit. Install with `/plugin`. *Use plugins to share a coherent setup across your team*, or to install community-built workflows (Playwright for browser testing, code reviewers, deployment pipelines). There are public plugin marketplaces and you can host an internal one for your team.

---

### A Quick Decision Guide

When you're not sure which feature to reach for:

| If you want to... | Use |
|---|---|
| Give Claude facts it should always know about this project | `CLAUDE.md` |
| Teach Claude a repeatable workflow (e.g., "how we do code reviews") | A skill |
| Run a noisy or parallel task without cluttering your main session | A subagent |
| Enforce a rule that must run every time (e.g., auto-lint, block dangerous commands) | A hook |
| Connect Claude to GitHub, a database, a browser, or another external system | An MCP server |
| Share a complete setup with your team | A plugin |

---

### Practical Starting Path

For colleagues new to Claude Code, I'd suggest in order:

1. Use Claude Code as-is for a week, just typing prompts. Get a feel for what it can do.
2. Add a `CLAUDE.md` with your project's conventions.
3. Turn on planning mode for non-trivial changes.
4. When you notice yourself repeating instructions, convert them into a skill.
5. When a task produces too much noise, move it into a subagent.
6. When you need a hard guarantee, add a hook.
7. Reach for MCP and plugins once the basics feel natural.

---

### Patterns for Long-Running Projects

The basics (one folder, one `CLAUDE.md`) work fine for short tasks. For projects that run weeks or months and accumulate lots of files, a few additional patterns help. These are drawn from [Mushtaq Bilal's Claude Code tutorial series](https://x.com/MushtaqBilalPhD/status/2053829787219595725), aimed at non-developers running long-form projects — but the patterns transfer well to any team doing sustained work.

**Nested `CLAUDE.md` files.** Instead of one giant `CLAUDE.md` at the project root, use a "global" one at the root with the big picture, and "local" ones in subfolders with task-specific rules. When Claude Code works inside a subfolder, it reads both — the global gives orientation, the local gives precision. Example: a root `CLAUDE.md` describes the project; `data/CLAUDE.md` says "treat all CSV files as raw; save cleaned versions with `_clean` appended, never overwrite originals"; `reports/CLAUDE.md` says "always follow argument → evidence → counterargument structure." Don't duplicate instructions across levels — Claude will follow the more specific one, but you'll get confused. Don't put contradictions across levels for the same reason.

**Ask Claude Code to create its own extensions.** You rarely need to hand-write a skill, slash command, subagent, or hook. Just describe what you want in plain English and ask Claude Code to create it. *"Create a slash command called `/firstdraft` that converts my raw notes into cohesive paragraphs."* *"Create a subagent called Citation Checker that takes a draft and verifies every cited source against the library folder, flagging anything missing, without editing the draft."* Claude Code writes the `.md` file in the right location. Restart the session and it's available. Edit the file later if needed.

**Use planning mode for anything with three or more steps, multiple folders, or long output.** Mushtaq's threshold is a good rule of thumb: small fixes go direct, complex tasks go through plan mode. "Synthesize my notes on 35 papers" is a plan-mode task. "Rename these PDFs by their titles" is not.

**Subagents for parallel and independent critique.** When you want multiple perspectives on the same artifact, run them as separate subagents in parallel — each has its own context window, so they don't influence each other and don't bloat your main session. Example: have a "Methodology Auditor" and a "Skeptical Reviewer" critique the same draft in parallel; you get two independent reports back as files, and your main context stays clean.

**The "four times manually" rule for automation.** Don't write a slash command, hook, or scheduled task for something you haven't done at least four times by hand. Premature automation produces tools that don't quite match the real workflow, then sit around getting outdated. Wait until you understand the pattern.

**Pre-edit safety hooks.** A simple, high-value hook: before Claude Code edits a file, copy the current version to a backup folder. Ask Claude Code to set it up: *"Create a pre-edit safety hook that copies a file and saves its current version before editing it."* This protects you from bad edits without needing checkpoint discipline.

**A few more guardrails worth borrowing:**

- Don't write slash commands longer than ~15 lines. If your instructions need more, split into two commands.
- Don't give subagents overlapping responsibilities — you'll get redundant or contradictory output.
- Never let a subagent edit your source files directly. Subagents produce reports as separate files; you decide what to act on.
- Don't connect MCP integrations to apps containing confidential information you don't want shared with the AI.

---

## Techniques from Anthropic's Claude Code Team

These come from Thariq Shihipar, the Claude Code engineering lead at Anthropic, mostly via his May 2026 workshop ["How we Claude Code"](https://howborisusesclaudecode.com/recap) and his article ["The Unreasonable Effectiveness of HTML"](https://x.com/trq212/status/2052811606032269638). They're the practices Anthropic uses internally.

### "Interview me" — the most repeated technique

When the requirements are fuzzy in your own head, don't try to write the perfect prompt. Ask Claude to interview you first.

```
I want to build [brief description]. Interview me in detail using
the AskUserQuestion tool. Ask about technical implementation, UI/UX,
edge cases, concerns, and tradeoffs. Don't ask obvious questions —
dig into the hard parts I might not have considered. Keep
interviewing until we've covered everything, then write a complete
spec to SPEC.md.
```

Then start a fresh session and prompt `Implement @SPEC.md`. Claude typically asks 20–60 questions depending on complexity. A third of them are usually things you genuinely hadn't considered. The technique now appears in [Anthropic's official Claude Code best-practices docs](https://code.claude.com/docs/en/best-practices).

**When to use it:** new features, anything ambiguous, anything where you don't already have a precise spec in your head.

**When to skip it:** small changes, fixes you can describe in one sentence, work where you already know exactly what you want. As Thariq puts it: if you have a precise spec in your head, getting quizzed is annoying.

You can wrap this in a slash command (`.claude/commands/interview.md`) so `/interview SPEC.md` does it in one keystroke.

### Ask for HTML, not Markdown, when Claude generates the artifact

If you're going to read the output but not edit it by hand, ask for HTML. Markdown won the default because humans edit it everywhere, but for Claude-generated specs, PR reviews, comparison tables, plans, and mockups, HTML produces clearer, denser, more useful artifacts: diagrams, expandable sections, color-coded severity, side-by-side comparisons, inline annotations.

Example prompt:

> *Help me review this PR by creating an HTML artifact that describes it. Focus on the streaming/backpressure logic. Render the actual diff with inline margin annotations, color-code findings by severity, and add whatever else conveys the concept well.*

This is now Anthropic's internal default for plans, code reviews, and reports. Stick with Markdown when you genuinely need to edit the file in your editor.

### Build verification into your workflow

Agents now run for hours at a time. Watching them isn't viable; verifying their output is. A few patterns from the workshop:

- **Always provide a way to verify.** Tests, screenshots, a script that runs the change. If you can't verify it, don't ship it.
- **Structure code for verification.** Keep state and UI in separate files so each can be tested in isolation. Code that's easy for humans to understand is also easier for agents to work on safely.
- **Use subagents for noisy exploration.** Investigations that read hundreds of files should happen in a subagent so your main context stays clean.

### Other useful patterns

- **Effort levels matter.** On low effort settings, Claude takes shortcuts (Thariq compared it to Pokemon speedrun glitches). Use higher effort or Extended Thinking for tasks where correctness matters more than speed.
- **Checkpoints + rewind.** Double-tap Escape or use `/rewind` to roll back to a previous state. This means you can let Claude try risky approaches without fear — if it doesn't work, rewind. Checkpoints persist across sessions.
- **Anti-goals help.** Tell Claude what NOT to do alongside what to do. *"Build a simple CLI bookmark manager. Don't add authentication. Don't over-engineer the database. I'm the only user."*
- **Agent view for multiple sessions.** Claude Code's built-in `agent view` lets you run multiple sessions side by side, like tmux but built for Claude Code.

---

## A Note on "Secret Codes" and Magic Commands

You'll see infographics on social media listing "secret" Claude commands like `/godmode`, `/ghost`, `/devil`, `/brief`, `/roast`, and so on, often promising to "100x" Claude's responses. **None of those are real built-in commands.** Claude has no hidden capabilities unlocked by magic words. When these "work," it's because Claude reads the word and infers what you want — `/brief` works the same as typing "be brief" or "max 3 lines."

The underlying ideas are mostly fine prompting techniques wearing a costume:

- *"Make Claude argue the opposite side"* → ask for a steelman or red-team analysis. Useful.
- *"Comparison table of all options"* → ask for one explicitly. Useful.
- *"Ultra-compressed output, 3 lines max"* → tell Claude the length you want. Useful.
- *"Just do the task, no explanation"* → "respond with the code only, no commentary." Useful.
- *"Brutally honest feedback, no sugarcoating"* → ask for it directly. Useful, occasionally.
- *"Reverse engineer why this works"* → ask Claude to explain the mechanism. Useful.

If you want these as actual slash commands so colleagues can type `/brief` and get a short answer, **create them yourself** as Claude Code slash commands (`.claude/commands/brief.md`) or as skills. That way they're real, documented, and shareable — not folklore. The "interview me" command above is a great template for the pattern.

The lesson: be skeptical of any post promising secret access to AI capability. The good prompting techniques are well-documented and don't need a costume.

---

## When Things Go Wrong

If the output isn't what you wanted, before rewriting the entire prompt, try:

1. **Tell Claude what was wrong with the last response.** *"That's too long — give me three bullets instead"* is usually enough.
2. **Ask Claude to state its assumptions.** This surfaces the misread.
3. **Add one specific constraint.** Often a single missing constraint is the problem.
4. **Switch to the long template** if iteration isn't converging and the task is genuinely complex.
5. **Start a new conversation.** If the chat has accumulated bad context, fresh is faster than course-correcting.

---

## Peak Hours and Rate Limits (Amsterdam Context)

A few points relevant to a team based in Amsterdam:

![Claude Code peak and off-peak hours by region](claude%20code%20usage%20eu.png)

**The "peak hours" picture has changed.** Tables like the one above (showing Amsterdam peak as 3 PM–9 PM CEST, and recommending mornings and late nights) were accurate when posted, but on **May 6, 2026** Anthropic announced two changes affecting Claude Code:

- The 5-hour session limits on Claude Code were **doubled** for Pro, Max, and Team plans.
- Peak-hour limit reductions on Claude Code were **removed** for Pro and Max plans.

So if your team uses Claude Code on a Pro, Max, or Team plan, peak hours no longer apply there. If you're on the **Free plan**, or using **claude.ai** (the web app, not Claude Code), peak-hour throttling still applies and the original table is still roughly correct: in Amsterdam (CEST, UTC+2), peak demand falls in the **afternoon and early evening**, and **mornings and late evenings** are off-peak. Always verify against Anthropic's official help center for current details, since these policies have changed several times in 2025–2026.

**Practical takeaways for an Amsterdam team:**

- **For heavy Claude Code work on Pro/Max/Team:** schedule freely. Limits are uniform across the day now, and 5-hour caps are roughly double what they were earlier in 2026.
- **For heavy claude.ai web app work:** if you bump into limits in the afternoon, shift that work to the morning or after dinner. Weekends are unrestricted.
- **For background or batch jobs:** the API has different rate limits than Claude Code's seat plans. If you're running long automated workflows, those go against API tier limits, which were also raised significantly in May 2026.
- **For teams spanning time zones:** colleagues in India and East Asia have most of their workday in off-peak windows for the web app; US-based colleagues in Pacific Time have their mornings in peak. Knowing this helps when scheduling shared work.

**Weekly limits still exist.** Even with the May 2026 boost, every plan has a weekly cap on total usage in addition to 5-hour session limits. The weekly cap was not changed. Heavy users can still hit it.

---

## Recommended Skills, Plugins, and Repos

A curated list of community resources that may help your team. **Caveat:** the Claude Code ecosystem moves fast, repos change names and ownership, and some entries below are from team recommendations rather than first-hand experience. Treat this as a starting reading list, not a vetted endorsement. Always read a repo's README and recent commits before installing anything that runs code on your machine.

**Development workflow frameworks:**

- [obra/superpowers](https://github.com/obra/superpowers) — A complete agentic development methodology built on composable skills. Forces a brainstorm → plan → implement flow with TDD, debugging, and review patterns baked in. Probably the most influential community skill collection. Install via `/plugin marketplace add obra/superpowers-marketplace`.
- [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) — A team-recommended plugin for compound engineering workflows. Worth reading the repo's README to see if their pattern matches how your team works.
- [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) — An agent harness with documented levers for cost and performance tuning (also appears in the token-optimization section below).

**Skills development and meta-tooling:**

- [anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) — Anthropic's own meta-skill for creating, modifying, and evaluating new skills. The official starting point if you want to write skills for your team.
- [anthropics/skills — frontend-design](https://github.com/anthropics/skills/blob/main/skills/frontend-design/SKILL.md) — Official skill for producing higher-quality frontend code and UI work; avoids the generic "AI-looking" design defaults.

**Memory and persistent context:**

- [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) — Persistent context across Claude Code sessions. Captures what happened, compresses it with AI, and injects relevant pieces back into future sessions via lifecycle hooks. Note: the project has a community crypto token associated with it (not made by the author); the core software is open source and runs locally.
- [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) — Skills relating to Obsidian (the note-taking app). Useful if your team's documentation lives in an Obsidian vault.

**Security and code review:**

- [anthropics/claude-code-security-review](https://github.com/anthropics/claude-code-security-review) — Anthropic's official security review tooling for Claude Code workflows.
- [getsentry/skills — gha-security-review](https://github.com/getsentry/skills/tree/main/skills/gha-security-review) — Sentry's GitHub Actions security review skill. Practical if you run CI/CD on GitHub Actions.
- [anthropics/claude-plugins-official — code-simplifier](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md) — An official agent definition for simplifying code; useful as both a tool and a reference for writing your own agents.

**External tools and integrations:**

- [openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc) — OpenAI's official plugin that lets you call Codex from inside Claude Code. Surprising but real. Useful for cross-model code review (have Claude write, have Codex review, or vice versa). Requires a ChatGPT subscription or OpenAI API key.
- [modelcontextprotocol/servers — sequentialthinking](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking) — Official MCP server that gives Claude a structured "think step by step" tool. Useful for hard reasoning problems where Extended Thinking alone isn't enough.
- [upstash/context7](https://github.com/upstash/context7) — MCP server that gives Claude live, version-accurate documentation for libraries. Solves the "Claude is suggesting code for the wrong library version" problem.
- [microsoft/playwright-cli](https://github.com/microsoft/playwright-cli) — Browser automation via Playwright. Pairs well with the official Playwright MCP plugin for end-to-end testing from inside Claude Code.
- [firecrawl/cli](https://github.com/firecrawl/cli) — Web scraping CLI from Firecrawl; useful when you need Claude to gather information from the web reliably.

**Starter kits and reference collections:**

- [garrytan/gstack](https://github.com/garrytan/gstack) — A starter stack for new projects. Investigate whether the conventions match your team's before adopting.
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — A curated list of design-oriented markdown resources.

**Research and exploration:**

- [karpathy/autoresearch](https://github.com/karpathy/autoresearch) — Andrej Karpathy's autonomous ML research loop. Not a productivity skill in itself, but a reference design for "agent iterates against a measurable goal" workflows. Community forks have ported the pattern to general software engineering tasks.
- [teng-lin/notebooklm-py](https://github.com/teng-lin/notebooklm-py) — A team-recommended Python interface for NotebookLM-style workflows.
- [HKUDS/RAG-Anything](https://github.com/HKUDS/RAG-Anything) — A RAG (retrieval-augmented generation) framework. Relevant if your team builds custom retrieval pipelines around Claude.

---

## Reducing Token Usage in Claude Code

Token usage hits in two places: 5-hour session limits (you'll get cut off mid-task) and weekly caps (worse — you're out for days). Even with the May 2026 limit increases, large refactors and codebase exploration burn through budgets fast. The tools below come from the community; they target different stages of the problem.

### Measure first — you can't optimize what you can't see

- [ryoppippi/ccusage](https://github.com/ryoppippi/ccusage) — CLI tool that reads local JSONL files in `~/.claude/projects/` and produces detailed reports: tokens per model, cache creation vs cache read, costs by day/month/session. The community-standard baseline tool. Run `npx ccusage` and you're done.
- [Maciek-roboblog/Claude-Code-Usage-Monitor](https://github.com/Maciek-roboblog/Claude-Code-Usage-Monitor) — Real-time terminal monitor with burn rate analytics, cost projections, and session forecasting that predicts when sessions will expire. Pair it with ccusage: one for history, one for live defense.
- [phuryn/claude-usage](https://github.com/phuryn/claude-usage) — Local web dashboard with Chart.js graphs if you prefer visuals to terminal tables.

### Reduce context and instruction bloat (input tokens)

- [nadimtuhin/claude-token-optimizer](https://github.com/nadimtuhin/claude-token-optimizer) — Restructures docs so Claude only loads what it needs; reports going from 11,000 → 1,300 tokens on a real project. Core idea: split your CLAUDE.md and only pay tokens for context when explicitly asked.
- [lucasrosati/claude-code-memory-setup](https://github.com/lucasrosati/claude-code-memory-setup) — The repo behind the "71.5x reduction" article. Fixes two silent failure modes: amnesia between sessions, and codebase re-reading inside a single session, by giving Claude a structural map of your code.
- [drona23/claude-token-efficient](https://github.com/drona23/claude-token-efficient) — Single CLAUDE.md file that keeps responses terse. Honest caveat from the author: the file loads into context on every message, so on low-output exchanges it's a net token increase. Only worth it on output-heavy workflows.

### Compress what Claude reads and writes

- [JuliusBrussee/caveman](https://github.com/juliusbrussee/caveman) — Skill that makes Claude write in terse fragments. Average 65% output reduction across 10 prompts; only affects output tokens, reasoning untouched. Yes, the name is silly. Also includes a sub-skill that rewrites CLAUDE.md into compressed form.
- [rtk-ai/rtk](https://github.com/rtk-ai/rtk) (Rust Token Killer) — CLI proxy that filters and compresses command output before it reaches the LLM context: `cargo test` 155→3 lines, `git status` 119→28 chars, roughly 10M tokens saved in 2 weeks per the author. Targets the often-overlooked source: noisy tool output.
- [ooples/token-optimizer-mcp](https://github.com/ooples/token-optimizer-mcp) — MCP server that caches and compresses tool descriptions and outputs.

### Architectural and systems-level levers

- [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) — Agent harness with documented levers like `MAX_THINKING_TOKENS=10000` to cap reasoning (saves ~70% on trivial tasks) and `CLAUDE_CODE_SUBAGENT_MODEL=haiku` so subagents use the cheaper model (~80% cheaper for exploration and file reading). Underrated levers — these are environment variables you can set today without installing anything else.

### Suggested order of operations

1. **Install ccusage today.** Run it. Look at the breakdown. Most token-optimization advice is wrong for your specific usage pattern; only data tells you what to fix.
2. **If your CLAUDE.md is bloated** → trim it manually first (target 80–120 lines), or reach for `claude-token-optimizer`.
3. **If tool output dominates your context** (lots of test runs, log dumps, `git status`) → rtk or similar filtering.
4. **If subagents are eating budget** → switch them to Haiku via the `CLAUDE_CODE_SUBAGENT_MODEL=haiku` env var.
5. **Skip the compression hacks until you've done steps 1–4.** They're marginal gains on top of structural fixes, and some (like always-loaded "be terse" instructions) can be net-negative depending on your workload.
