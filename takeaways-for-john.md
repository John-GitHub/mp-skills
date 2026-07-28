# Takeaways for John

Personal notes on how Claude Code skills, commands, and tools actually work — captured while exploring Matt Pocock's `mp-skills` repo and installing his skills. Written for me, on this machine, so the paths and versions are the real ones.

## At a glance

- I installed Matt's skills as a **Claude Code plugin** (`mattpocock-skills@mattpocock`) — a managed, read-only bundle, not editable copies.
- The install is **global** (user scope): once done, the skills work in **every** project on this machine. No per-repo reinstall.
- The files live in my home directory under `~/.claude/plugins/`, **not** inside any project. Nothing is hidden — I can open any skill's `SKILL.md` and read it.
- It's a **snapshot** pinned to version `1.2.0`, not a live sync. It changes only when I explicitly update the plugin.

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
| Give Claude a new callable function | MCP server (a tool) | No — independent |

I **can't** create the hardwired built-ins (`/model`, `/plugin`, `/context`, `/help` — only Anthropic ships those). I **can** drop a Markdown file into `~/.claude/commands/<name>.md` and it becomes `/<name>`; the file body is a prompt template fed to Claude when I invoke it — the lighter-weight cousin of a skill. For a genuine new tool I'd stand up an MCP server (see §5), or define tools in code via the Agent SDK if I were building my own agent.

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
| What it does | Registers a managed, read-only bundle globally | *Copies* editable skill files into a target project |
| Updates | Auto: follows the plugin `version` | Manual: re-copy to update |
| Best when | "Just keep me current, I won't edit them" | "I want my own editable fork inside this project" |

Since I only care about **capability, not owning local copies**, the plugin is the right choice and npx adds nothing I want. *Live a quiet life and ignore the npx step unless you want to explore your own version of a skill — and you probably don't.*

### Exact sequence — Path A (plugin)

```text
/plugin marketplace add mattpocock/skills
/plugin install mattpocock-skills@mattpocock
/reload-plugins
/setup-matt-pocock-skills
```

- Lines 1–3 are **one-time, global** (not per repo). `/reload-plugins` is required before the new slash commands (like `/ask-matt`) show up as recognized in the current session — installing alone isn't enough.
- Line 4, `/setup-matt-pocock-skills`, is **per new repo**: it asks which issue tracker to use, what triage labels I apply, and where to save docs.

### Exact sequence — Path B (npx), only if I want editable copies

```text
npx skills@latest add mattpocock/skills   # pick skills + agents; be sure to select /setup-matt-pocock-skills
/setup-matt-pocock-skills
```

Per repo every time, since it copies files into that repo's own tree.

---

## 4. What the install did, and where everything lives

Verified by reading Claude Code's own config files directly — not guessed from docs.

### It's global (user scope)

- `~/.claude/settings.json` → `"enabledPlugins": {"mattpocock-skills@mattpocock": true}`
- `~/.claude/plugins/installed_plugins.json` → `"scope": "user"`, cached at `~/.claude/plugins/cache/mattpocock/mattpocock-skills/1.2.0`

Both live in my home directory, not in any project — and `mp-skills` has no `.claude/` folder at all. So the plugin is available in **every** project on this machine, with no per-repo reinstall. A fresh project just works; Matt's skill *files* won't appear inside that new project (they stay in `~/.claude/plugins/`), and that's fine — capability follows me, not files. I'd only re-touch `/plugin` or `/reload-plugins` if the skills stopped showing up.

### What the two install commands actually mean

- `/plugin marketplace add mattpocock/skills` registers the GitHub repo as a **marketplace pointer** in `~/.claude/settings.json`. Claude Code reads that repo's `.claude-plugin/marketplace.json`, which lists one plugin: `mattpocock-skills`, sourced from `./`.
- `/plugin install mattpocock-skills@mattpocock` installs it, pinned to a specific git commit (`2ab9580…`, version `1.2.0`). Claude Code reads the plugin's `.claude-plugin/plugin.json`, whose `skills` array lists 22 skill directories — exactly the promoted `engineering/` + `productivity/` skills. Each `SKILL.md` becomes available: model-invoked ones reachable automatically, user-invoked ones only when I type them.
- It's a **snapshot, not a live sync**: frozen at that commit until I explicitly update. Unlike the symlink pattern (§2), a `git pull` on Matt's repo won't change what's installed.

### The files are on my disk — nothing is hidden

Two similar-looking names that are easy to confuse:

- `.claude-plugin/` — a folder *inside Matt's repo* holding only the **manifests** (`plugin.json`, `marketplace.json`). This is the table of contents — it lists which skill folders belong to the plugin. It is **not** where the skill content lives.
- `~/.claude/plugins/` — a folder in *my home directory* (`C:\Users\johnp\.claude\plugins\`). This is where the install actually wrote files.

Under `~/.claude/plugins/` the install produced two real copies:

1. `marketplaces/mattpocock/` — a **full git clone** of Matt's entire repo (`.git` and all).
2. `cache/mattpocock/mattpocock-skills/1.2.0/` — the **pinned copy** Claude Code actually loads from at runtime.

To read any skill's definition, open its `SKILL.md`. For example, `/ask-matt` is literally this file:

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
| **Tool** | `/mcp` lists connected MCP servers + their tools; `/context` shows the full active tool set | Built-in tools (`Read`, `Bash`, `Edit`): none exposed — they live in the harness. MCP tools: the **source of the MCP server** | Built-in + tool-use concepts: Anthropic API / Agent SDK docs. MCP generally: the spec at `modelcontextprotocol.io` |

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
# see the entire active tool set (built-ins + MCP) with token costs
/context

# author a tool = stand up an MCP server, then register it.
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
