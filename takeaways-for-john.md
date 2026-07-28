# Takeaways for John

Personal notes on how Claude Code skills, commands, and tools actually work — captured while exploring Matt Pocock's `mp-skills` repo and installing his skills. Written for me, on this machine, so the paths and versions are the real ones.

## At a glance

- I installed Matt's skills as a **Claude Code plugin** (`mattpocock-skills@mattpocock`) — a managed, read-only bundle, not editable copies.
- The install is **global** (user scope): once done, the skills work in **every** project on this machine. No per-repo reinstall.
- The files live in my home directory under `~/.claude/plugins/`, **not** inside any project. Nothing is hidden — I can open any skill's `SKILL.md` and read it.
- It's pinned to version `1.2.0` (a snapshot, not a live sync of Matt's repo). It moves only when Matt publishes a **new version** *and* the marketplace refreshes — automatically if auto-update is on for that marketplace, otherwise on a manual `/plugin` update.

---

## 1. The mental model: commands vs skills vs tools

Start here — this vocabulary makes everything below make sense. These three get muddled because a skill is *exposed as* a slash command, but they're distinct layers. **Skills are instructions; tools are callable functions; a command is just something I type.** They combine, but none contains the others.

| Layer | Who invokes it | Backed by | Example |
|---|---|---|---|
| **Slash command** | I type it | CLI logic *or* a command file | `/model`, `/ask-matt` |
| **Skill** | Me *or* the model | a `SKILL.md` folder | `/ask-matt`, `tdd` |
| **Tool** | The model (Claude) | harness/server code | `Read`, `Bash`, `mcp__…__navigate` |

`/ask-matt` appears in two rows on purpose — it's a *skill surfaced as* a slash command. That overlap is exactly what makes these easy to confuse.

### What a "tool" really is

A **tool** is a named function the model can call, usually with structured parameters (a JSON input schema), that runs and returns a result. The model picks the tool and fills the parameters; the harness executes it and hands back the result.

There are **two senses of "tool"** — and the DeepLearning.AI course likely showed the second while calling it the first:

1. *Strict tool-use sense* — a function **registered with the model** so Claude can call it directly. Defined at the **harness or server level**, independent of any skill. Most tools (`Read`, `Bash`, the MCP browser tools) have nothing to do with skills.
2. *A helper script bundled inside a skill folder* — e.g. a skill folder holding `SKILL.md` plus `analyze.py`. That script is **not** a registered tool. The `SKILL.md` just says "run `analyze.py`," and Claude runs it **using the Bash tool**. The script rides on an existing tool; it never becomes a new callable tool. This is the only "tool-ish" thing that lives *inside* a skill.

So: **tools normally live outside skills.** A skill can *tell* Claude to use a tool, and can *bundle a script* run via a tool — but the tools themselves are a separate layer.

### What I can author myself

| I want to… | Author this | Tied to a skill? |
|---|---|---|
| Trigger a canned prompt by name | Custom slash command (a `.md` in `~/.claude/commands/` or `<project>/.claude/commands/`) | No |
| Package reusable instructions/discipline | Skill (a `SKILL.md` folder, optionally + scripts) | *is* the skill |
| Give Claude a new callable function | MCP server (which *exposes* one or more tools) | No — independent |

I **can't** create the hardwired built-ins (`/model`, `/plugin`, `/context`, `/help` — only Anthropic ships those). I **can** drop a Markdown file into `~/.claude/commands/<name>.md` and it becomes `/<name>`; the file body is a prompt template fed to Claude when I invoke it. In current Claude Code these custom commands have essentially **merged into skills** — a bare `.md` in `commands/` is now the lightweight/legacy form of a skill, not a separate authored layer. (This is a moving target between versions, so treat the neat three-layer split above as a teaching model, not a spec.) For a genuine new tool I'd stand up an MCP server (see §5), or define tools in code via the Agent SDK if I were building my own agent.

---

## 2. How Claude Code finds skills

Claude Code only recognizes skills placed in specific directories it **scans**:

- `~/.claude/skills/<name>/` — user-level, available in every project
- `<project>/.claude/skills/<name>/` — project-level, that repo only
- (`~/.agents/skills/` is the equivalent for Codex and other Agent-Skills harnesses)

Just having a `SKILL.md` somewhere in a git repo is **not** enough — it has to live in (or be linked into) a scanned location. Plugins are the exception: they register through a separate mechanism (§4) and don't need to sit in the scan path at all.

### The symlink pattern (for when I'm *editing* a skills repo)

If I ever maintain skills in a separate source repo and want to edit-and-use them live from that working copy, the trick is to **symlink** each skill folder into a scanned directory instead of copying it:

```bash
ln -sfn /path/to/repo/skills/<bucket>/<name> ~/.claude/skills/<name>
```

Because it's a symlink, a `git pull` or a local edit shows up immediately — no reinstall. That's what `mp-skills`' own `scripts/link-skills.sh` does (linking into both `~/.claude/skills` and `~/.agents/skills`). It's a **dev-loop convenience for skill authors**, explicitly "dev-only… not a supported installer" — not how I consume someone else's skills.

---

## 3. Installing Matt's skills: two paths, pick one

Two supported ways to consume these skills, two philosophies. I don't run both.

| | **Plugin** (what I used) | **npx / skills.sh installer** |
|---|---|---|
| Command | `/plugin marketplace add` + `/plugin install` | `npx skills@latest add mattpocock/skills` |
| What it does | Registers a managed bundle globally | Deploys editable skill files into a target — **defaults** to copying into the current project, but can also go global or symlink |
| Updates | When Matt bumps the version and the marketplace refreshes (auto if enabled, else manual `/plugin` update) | Manual: re-run the installer (or `git pull` if symlinked) |
| **Cross-agent?** | **Claude Code only** — no Codex/other-harness plugin yet | **Yes** — installs into Codex and other Agent-Skills harnesses too |
| Best when | "Just keep me current in Claude Code, I won't edit them" | "I want editable copies, and/or the same skills across Codex + others" |

Since I only cared about **capability in Claude Code, not owning local copies**, the plugin was the right choice. The one thing that would flip this: I also use **Codex** — and the plugin doesn't exist for Codex, so if I want these skills there too, `skills.sh` is the only path that reaches across agents. *Live a quiet life and ignore the npx step unless you want editable copies or cross-agent reach — for Claude-Code-only capability, you don't need it.*

### Exact sequence — Path A (plugin)

```text
/plugin marketplace add mattpocock/skills
/plugin install mattpocock-skills@mattpocock
/reload-plugins
/setup-matt-pocock-skills
```

- Lines 1–3 are **one-time, global** (not per repo). `/reload-plugins` is required before the new slash commands show up as recognized in the current session — installing alone isn't enough.
- Line 4, `/setup-matt-pocock-skills`, is **per new repo**: it asks which issue tracker to use, what triage labels I apply, and where to save docs.

**A note on how plugin skills are named when I invoke them.** Every plugin skill has a **fully-qualified name**, `/<plugin>:<skill>` — e.g. `/mattpocock-skills:ask-matt`. That namespaced form is what `/context` displays (I saw `mattpocock-skills:code-review`, etc.) and it always works. The **bare** form, `/ask-matt`, works as a shortcut when the name is unambiguous — and Matt's own README uses the bare `/setup-matt-pocock-skills`, so it's a real, author-blessed shorthand, not a mistake. If a bare name ever isn't recognized, reach for the fully-qualified one. *(Quickest way to see which forms exist: type `/ask` at the prompt and read the autocomplete.)*

### Exact sequence — Path B (npx), only if I want editable copies

```text
npx skills@latest add mattpocock/skills   # pick skills + agents; be sure to select /setup-matt-pocock-skills
/setup-matt-pocock-skills
```

By default this copies into the repo you're standing in, so it's per-repo — but the installer can also install globally or via symlink, so "per repo" is the default, not a hard rule.

---

## 4. What the install did, and where everything lives

Verified by reading Claude Code's own config files directly — not guessed from docs. **Caveat on what "verified" buys me:** reading these files proves what's on **this machine, at version `1.2.0`, right now** — it does *not* prove the behaviour is guaranteed, unchanged across versions, or identical on someone else's setup. Local file inspection answers "what got stored here," not "what the product promises in general."

### It's global (user scope)

- `~/.claude/settings.json` → `"enabledPlugins": {"mattpocock-skills@mattpocock": true}`
- `~/.claude/plugins/installed_plugins.json` → `"scope": "user"`, cached at `~/.claude/plugins/cache/mattpocock/mattpocock-skills/1.2.0`

Both live in my home directory, not in any project — and `mp-skills` has no `.claude/` folder at all. So the plugin is available in **every** project on this machine, with no per-repo reinstall. A fresh project just works; Matt's skill *files* won't appear inside that new project (they stay in `~/.claude/plugins/`), and that's fine — capability follows me, not files. I'd only re-touch `/plugin` or `/reload-plugins` if the skills stopped showing up.

### What the two install commands actually mean

- `/plugin marketplace add mattpocock/skills` registers the GitHub repo as a **marketplace pointer** in `~/.claude/settings.json`. Claude Code reads that repo's `.claude-plugin/marketplace.json`, which lists one plugin: `mattpocock-skills`, sourced from `./`.
- `/plugin install mattpocock-skills@mattpocock` installs it, pinned to a specific git commit (`2ab9580…`, version `1.2.0`). Claude Code reads the plugin's `.claude-plugin/plugin.json`, whose `skills` array lists 22 skill directories — exactly the promoted `engineering/` + `productivity/` skills. Each `SKILL.md` becomes available: model-invoked ones reachable automatically, user-invoked ones only when I type them.
- It's a **snapshot, not a live sync**: frozen at that commit. A newer version arrives only when Matt bumps the plugin `version` *and* the marketplace refreshes — automatically if auto-update is enabled for that marketplace, otherwise on a manual `/plugin` update. Either way, unlike the symlink pattern (§2), a plain `git pull` on Matt's repo won't change what's installed.

### The files are on my disk — nothing is hidden

Two similar-looking names that are easy to confuse:

- `.claude-plugin/` — a folder *inside Matt's repo* holding only the **manifests** (`plugin.json`, `marketplace.json`). This is the table of contents — it lists which skill folders belong to the plugin. It is **not** where the skill content lives.
- `~/.claude/plugins/` — a folder in *my home directory* (`C:\Users\johnp\.claude\plugins\`). This is where the install actually wrote files.

Under `~/.claude/plugins/` the install produced two real copies:

1. `marketplaces/mattpocock/` — a **full git clone** of Matt's entire repo (`.git` and all).
2. `cache/mattpocock/mattpocock-skills/1.2.0/` — the **pinned copy** Claude Code actually loads from at runtime.

To read any *skill's* definition, open its `SKILL.md`. One caveat on "nothing is hidden": a plugin can ship more than skills — hooks, MCP servers, agents, even executables on `PATH` — so fully *auditing* a plugin means reading its manifest, not just one `SKILL.md`. Matt's `plugin.json` happens to declare **only** a `skills` array (no hooks/servers/agents), so here reading the `SKILL.md` really is enough. For example, `/ask-matt` is literally this file:

```
C:\Users\johnp\.claude\plugins\cache\mattpocock\mattpocock-skills\1.2.0\skills\engineering\ask-matt\SKILL.md
```

> **Aside — a wrong guess I later corrected:** I'd assumed `mattpocock-skills` also appears under Anthropic's built-in `claude-plugins-official` marketplace. Checking that marketplace's plugin list on disk, it isn't there. I don't have a verified explanation for the `@claude-plugins-official` label I saw earlier — noting it so I don't trust the assumption later.

---

## 5. Getting help / finding the defining code for each layer

Same three layers as §1, same order. For each: how to **list** what exists, where the **definition on disk** lives (if any), and where the **docs** are.

| Layer | List what's available | Definition on disk | Where the docs are |
|---|---|---|---|
| **Slash command** | Type `/` for the autocomplete list; `/help` for built-ins | Built-ins: none (in the binary). Custom/plugin: the `.md` file in `~/.claude/commands/`, `<project>/.claude/commands/`, or the plugin's `commands/` folder | Built-ins: Claude Code docs (`code.claude.com/docs`) or `/help`. Custom: it *is* the `.md` file — just read it |
| **Skill** | `/skills` | The `SKILL.md`. Plugin: `~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/skills/.../SKILL.md`. User: `~/.claude/skills/<name>/SKILL.md`. Project: `<project>/.claude/skills/<name>/SKILL.md` | The `SKILL.md` itself is the source of truth. Matt's also have a human page at `aihero.dev/skills-<skill-name>`. Authoring: `code.claude.com/docs` |
| **Tool** | `/mcp` is the tool/server lister; `/context` shows what's filling the context window (tools appear there, with token costs, because their definitions consume context) | Built-in tools (`Read`, `Bash`, `Edit`): none exposed — they live in the harness. MCP tools: the **source of the MCP server** | Built-in + tool-use concepts: Anthropic API / Agent SDK docs. MCP generally: the spec at `modelcontextprotocol.io` |

### Exact commands — copy/paste, don't fumble

**Slash commands**

```text
# list everything invocable
/help
# (or just type "/" and read the autocomplete popup)

# read a custom command's definition (Git Bash)
cat ~/.claude/commands/explain.md
# PowerShell equivalent
Get-Content $HOME\.claude\commands\explain.md

# author one: create the file, and it's instantly available as /explain
#   file: ~/.claude/commands/explain.md
#   body (a prompt; $ARGUMENTS is filled from what you type after the command):
#     Explain how $ARGUMENTS works in this codebase, citing file:line references.
# then invoke:
/explain the auth flow
```

**Skills**

```text
# list installed skills
/skills
# invoke a plugin skill by its full name /<plugin>:<skill> (e.g. /mattpocock-skills:ask-matt),
# or the bare /<skill> shortcut when the name is unambiguous

# read a skill's full definition on disk (Git Bash)
cat ~/.claude/plugins/cache/mattpocock/mattpocock-skills/1.2.0/skills/engineering/ask-matt/SKILL.md
# PowerShell equivalent
Get-Content "$HOME\.claude\plugins\cache\mattpocock\mattpocock-skills\1.2.0\skills\engineering\ask-matt\SKILL.md"
# or the zero-syntax way — just ask:
#   "Claude, show me the ask-matt SKILL.md"

# author one: create the folder + file, then re-scan
#   file: ~/.claude/skills/my-skill/SKILL.md   (with YAML frontmatter: name, description)
# invoke it by name:
/my-skill
```

**Tools**

```text
# list connected MCP servers and the tools each exposes
/mcp
# see what's filling the context window — tools show up here with token costs
# (it's a context-usage view, not the authoritative tool inventory; /mcp is that)
/context

# author a tool = expose it via an MCP server (one server can advertise several tools), then register it.
# form: claude mcp add [options] <name> <commandOrUrl> [args...]
# stdio server (a local command); everything after -- is the command Claude runs:
claude mcp add my-server -- node C:/path/to/my-server.js
# IMPORTANT: default scope is "local" (this project only). For availability
# everywhere on this machine — the tool-layer version of "global" — add -s user:
claude mcp add -s user my-server -- node C:/path/to/my-server.js
# with env vars: -e KEY=value (before the --)
claude mcp add -s user my-server -e API_KEY=xxx -- npx my-mcp-server
# a remote HTTP server instead of a local command:
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp
# verify it landed and see its tools:
/mcp
# once registered, its tools appear as mcp__my-server__<toolname> and Claude calls them like Read/Bash
```

Scope flag `-s` takes `local` (default, this project), `user` (all my projects on this machine), or `project` (checked into the repo's `.mcp.json`, shared with anyone who clones it) — the same three-way scoping idea that distinguishes user vs project skills. *(Verified against `claude mcp add --help`.)*

### Fastest path in practice

- *"What's available right now?"* → `/` (commands), `/skills` (skills), `/mcp` + `/context` (tools).
- *"What does this skill actually do?"* → open its `SKILL.md` on disk, or just ask Claude to read it. Nothing is hidden.
- *"What does this built-in command/tool do?"* → `/help` or the Claude Code docs — no local file, so docs are the only source.

**Rule of thumb — "is there code I can read?"** If it has a file on my disk (custom command, any skill, my own MCP server), the file *is* the documentation — read it first. If it's baked into the binary (built-in commands, built-in tools), there's no file — go to the docs.

---

## 6. Evaluating skills, prompts, and agents

An **evaluation** ("eval") is a repeatable test of *observable behaviour*: give the model a task, record what it produces, and judge the result against criteria fixed in advance. This section is how I'd actually test whether a skill earns its keep. *(The concrete tooling below is grounded in Anthropic's `skill-creator` plugin, whose full source is on my disk — see §5 for how I read it; specific claims are marked verified.)*

### What counts as evidence

I don't get the model's private chain of thought, and I don't need it. But don't confuse *private reasoning* with the *agent transcript* — the transcript is fully observable and is one of the most useful diagnostic artifacts there is. Observable and usable:

- the input prompt;
- whether a given skill actually loaded;
- the tool calls and their arguments, in order;
- files read, written, or modified;
- the final response or artifact;
- elapsed time and token consumption;
- pass/fail on deterministic checks;
- the full run transcript.

**Read the transcripts, not just the final outputs.** A skill can produce an acceptable deliverable while tripling the token cost, or three separate runs can each independently write the same helper script — signals you only see in the trajectory. A skill that improves the artifact but bloats the route is a mixed result, not a win. The infrastructure that runs these tests repeatedly is an **evaluation harness**: minimally a test-case file, a repeatable execution script, and a score sheet.

### The three evaluation targets

| Object | Main question | Simplest comparison |
|---|---|---|
| **Prompt** | Does this wording produce a better result? | Prompt A vs. Prompt B |
| **Skill** | Does the skill improve the model's work? | With skill vs. without skill |
| **Agent** | Does it complete the task by an acceptable route? | Final result **plus** the observable tool-call trajectory |

A skill carries a **second, separate question**: does it *fire when it should and stay dormant when it should not?* Output quality and activation accuracy are distinct engineering problems with distinct test sets — don't try to measure both in one batch.

### The minimum useful skill evaluation

Start with **three realistic tasks**. For each task, run two arms: (1) the model with the skill loaded, (2) the model without it (baseline). Apply *identical* assertions to both arms. Don't open with 50 cases, heavy statistics, or a hosted platform — the first goal is to find out whether the *test design* is sound, not to produce a number.

**The n = 1 problem, stated plainly.** Three cases × two arms = six runs, and each cell holds a *single* observation. There's no error term: I can't separate a skill effect from run-to-run variance, and for many skills the variance is larger than the effect. Treat iteration 1 as **design validation, not measurement.** Two cheap mitigations, in order of value:

- **Replicate before concluding.** Once the test cases stop changing, run each cell three times and report mean ± standard deviation, not a single pass rate. *(Verified: `skill-creator`'s `aggregate_benchmark.py` reports mean/stddev/min/max per configuration plus the with/without delta, and its benchmark metadata hardcodes `runs_per_configuration: 3`.)*
- **Blind the grader.** If an LLM grader knows which output came from the skill arm, the comparison is contaminated. Strip the labels, or use the paired blind-comparison path. *(Verified: `agents/comparator.md` — an independent agent is shown two outputs and not told which is which.)*

**Guard the baseline.** If the skill is installed at **user scope** (§4), it can quietly load in the "baseline" arm and destroy the comparison. Confirm the baseline run had no access to the skill — disable it for that run, or inspect the transcript for a skill-load event. A baseline I didn't verify is not a baseline.

### Assertions: deterministic vs. heuristic

An **assertion** is a concrete, checkable property the output must have. For a domain-modeling skill, say: asks exactly one substantive unresolved question; doesn't repeat an answered question; doesn't emit a spec document before the interview is complete; makes a definitive recommendation where the evidence supports one.

Tag each assertion with its **modality** when I write it — the two are graded by different machinery:

- **Deterministic** — verified by *script*: was a required file produced, does the JSON validate, is the question count exactly one. Prefer these; write the script once and reuse it across iterations.
- **Heuristic** — judged by a human or an LLM grader: conceptual soundness, relevance, whether a recommendation is defensible. The grader judges the *observable artifact*, not the reasoning behind it.

Don't force assertions onto genuinely subjective properties — for those, human review is the instrument. *(Verified: `skill-creator`'s `grading.json` records each expectation as `text` / `passed` / `evidence`, and its guide pushes scriptable checks over eyeballing.)*

### No-fumble — install the official skill evaluator

Anthropic ships a `skill-creator` plugin that creates, revises, and *evaluates* skills: paired with-skill/baseline runs, assertions, per-run timing and token capture, benchmark aggregation with variance, an HTML review viewer for human grading, optional blind A/B comparison, and a separate trigger-description optimizer.

**Prerequisites.** Claude Code **with subagents** (the paired-run workflow spawns them) — the Claude.ai environment can't run baselines. Expect a real token bill: each test case is *two* full agent runs.

Open Claude Code in the target project. Run these **at the Claude prompt** (not PowerShell):

```text
/plugin install skill-creator@claude-plugins-official
/reload-plugins
```

- The official marketplace (`claude-plugins-official`) is registered on my machine already (it's in `known_marketplaces.json`) and appears to be added at startup, so a manual refresh is a *troubleshooting* step, not a routine one. If the install says the plugin isn't found, the local catalog is stale — run `/plugin marketplace update claude-plugins-official` and retry; if the *marketplace* is missing, run `/plugin marketplace add anthropics/claude-plugins-official`.
- `/plugin install` asks for a **scope**: user (all my projects), project (shared via `.claude/settings.json`), or local (this repo, me only). Choose deliberately — user scope is exactly what makes baseline contamination easy.
- `/reload-plugins` activates it without a restart. *Hedge:* it may report `0 skills` even when the skill loaded fine (its counter seems to reflect the plugin's `commands/` dir) — I haven't confirmed the mechanism, so I don't treat that count as a failure.

Confirm what installed, **at the Claude prompt**:

```text
/plugin list
```

To start it: invoke the namespaced skill directly (plugin skills are namespaced `plugin:skill`, so `/skill-creator:skill-creator`), or just state the task in plain language — its description is written to trigger on eval and skill-authoring requests. *(Verified: `plugin.json` name is `skill-creator`; the skill folder is `skills/skill-creator`.)*

### No-fumble — run a small first evaluation

**1. Locate the target skill.** Discover the path; don't hardcode a version (§5 covers this). **In PowerShell:**

```powershell
Get-ChildItem "$HOME\.claude\plugins\cache" -Directory -Recurse -Depth 3 |
  Where-Object { $_.Name -eq 'skills' }
```

Then read the `SKILL.md` under the version you actually found to confirm the target. Two cautions: this is a **Claude-managed cache** — fine to read and test against, but any edit there is overwritten on the next update, so to customize I copy the skill folder somewhere writable and point the evaluator at the copy. And if I ever share *this* document, I shouldn't paste an absolute path from it — it carries my username. (I keep the real `C:\Users\johnp` paths here because these notes are for me; see the intro.)

**2. Start the evaluator with a stop instruction.** Invoke the skill and give it bounded parameters — the final line is the circuit breaker:

> Evaluate the skill at `<absolute path to the skill folder>`.
> Start with exactly three realistic test prompts. For each, run two arms: one with the skill, one baseline with no skill loaded.
> Before running anything, show me: the three prompts; the assertions for each, tagged deterministic or heuristic; the files each run must produce; the workspace location you'll use.
> Grade only observable outputs, tool calls, files, transcripts, timing, tokens, and errors.
> **Stop after showing me the proposed test cases and assertions. Do not spawn any runs yet.**

Without that last line, the agent will launch six subagent runs against cases I haven't approved.

**3. Execute and review.** Once the cases resemble work I actually do:

> Run the three approved test cases, both arms. Grade every assertion, preserve the raw outputs and transcripts, capture timing and token counts, and generate the review viewer so I can inspect the outputs myself. Do not revise the skill yet.

Two things on that. **Preserve raw outputs and transcripts** — a summary score without the underlying data can't be audited or re-graded. And **capture timing the moment each run finishes**: the token and duration figures arrive in the subagent completion notification and are written to `timing.json`; they aren't persisted anywhere else. *(Verified: the `SKILL.md` calls that notification "the only opportunity to capture this data.")* **Look at the outputs myself before reading any score** — the skill-creator is emphatic about getting artifacts in front of the human *before* the agent's summary frames how I read them.

**4. Expected result layout** *(verified against `SKILL.md` step 1 + `aggregate_benchmark.py`)*:

```text
<skill-name>-workspace/                  # sibling to the skill folder
└── iteration-1/
    ├── eval-0/                          # one dir per test case (given a descriptive name)
    │   ├── eval_metadata.json           # eval_id, eval_name, prompt, assertions[]
    │   ├── with_skill/
    │   │   ├── outputs/                 # the files this run produced
    │   │   └── run-1/                   # run-1, run-2, run-3 when replicating
    │   │       ├── grading.json         # summary{pass_rate,…}, expectations[]{text,passed,evidence}
    │   │       └── timing.json          # total_tokens, duration_ms, total_duration_seconds
    │   └── without_skill/               # the baseline arm (or old_skill/ when improving a skill)
    │       └── run-1/ …
    ├── benchmark.json                   # aggregated mean ± stddev per arm + delta
    └── benchmark.md                     # human-readable version
```

### Diagnosing the results

**Don't lead with the aggregate score.** Ask instead:

- Did the test cases reflect *real* work, or did I write prompts the skill was bound to handle?
- Did the assertions actually *discriminate* — did anything pass in one arm and fail in the other?
- Did the baseline sometimes win? (If it usually wins, the skill is a net negative; if it *never* wins on any case, suspect a contaminated baseline.)
- Did the skill fail *consistently* or *intermittently*? Consistent failure implicates the skill; intermittent failure implicates variance, an ambiguous prompt, or an unreliable grader.
- Did the skill improve the deliverable while adding operational drag — more turns, more tokens, longer wall-clock?

A failed assertion doesn't by itself indict the skill — it can equally indict a vague test prompt, a badly specified assertion, a flaky grader, or ordinary variance. The value of the eval is that the failure is now *inspectable* rather than anecdotal. When I revise, I **generalize**: I'm iterating on three examples to build something that runs on thousands, so fixes that make only those three pass are overfitting.

### Trigger evaluation

Run this **only after output quality is satisfactory** — precise activation on a skill that performs badly is worthless. Build ~20 realistic queries, 8–10 that *should* trigger and 8–10 that *should not*, and have a human sign off on the labels first. (Call them positive/negative **cases** — "true positive/negative" are *outcomes* of a test, not properties of an input.) Quality rules that matter more than count:

- **Realistic, not abstract.** "Format this data" tests nothing. Real queries carry file paths, column names, company names, backstory, typos, casual phrasing.
- **Negatives must be near-misses.** A negative sharing no vocabulary with the skill is free credit; the useful ones are adjacent-domain requests where a naive keyword match would fire but shouldn't — e.g. *"generate Python dataclasses from this approved domain model"* against a domain-modeling *interview* skill.
- **Positives must be substantive.** Claude only consults a skill when the task is one it can't trivially handle alone, so a one-step request may not trigger a skill however good the description — trivial positives read as false negatives and send me optimizing a description that was never the problem.

*(Verified: `skill-creator`'s `run_loop.py` runs each query 3× for a stable trigger rate, splits 60/40 train/held-out, and picks `best_description` by **test** score to avoid overfitting; it requires `claude -p`, so it's Claude Code-only.)*

### When to scale the harness

The three-case manual recipe is for **discovery**. Move to a programmatic Python harness or a hosted platform (Phoenix, LangSmith) when I need: dozens/hundreds of cases; systematic variance across repeated runs; regression testing in CI; production trace monitoring; or multi-model / prompt-version A/B testing.

**Rule of thumb — start small, defend the cases.** Begin with three realistic tasks and assertions I can defend under questioning. The machinery scales later; the quality of the test cases cannot be outsourced, and no amount of harness sophistication rescues a bad one.
