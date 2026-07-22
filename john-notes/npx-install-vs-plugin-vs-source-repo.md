# npx install vs. plugin vs. having the source repo cloned

**Question this answers:** I already have this repo (mp-skills) cloned with well-formed skills in it — what does the Quickstart's `npx` command actually do, what does "install" mean here, and how does that compare to the Claude Code plugin option, given I'll be using Claude Code, Codex, and likely other LLMs?

---

## What `npx` does

`npx skills@latest add mattpocock/skills` doesn't touch this cloned repo at all. `npx` fetches an npm package (here, the `skills` CLI from [skills.sh](https://skills.sh)) and runs it once, without installing it globally. That CLI then reaches out to GitHub (`mattpocock/skills`), lets you pick which `SKILL.md` files you want, and **copies** them into whatever project directory you're standing in when you run it — plus wires them into the agent-specific config each harness expects (`.claude/skills/`, Codex's equivalent, etc.).

## What "install" means here

This repo (my fork) is the **source** — the skills are already well-formed here. But a skill only does anything when it's sitting inside a *target project's* skill directory where your coding agent will actually look for it. "Install" = deploy copies of the chosen skills into that target repo (or into a global `~/.claude/skills`, `~/.agents/skills`, etc.) so Claude Code/Codex can see and invoke them there.

So this is a matter of *distribution*, not *authoring*. The skills already exist and work; the question is just getting copies into the places your agents read from. That's literally the entire job of `npx skills@latest add ...`.

(Side note: the project's own `scripts/link-skills.sh` does something similar but via **symlinks** into `~/.claude/skills` / `~/.agents/skills` — that's the workflow Matt himself uses to keep his own global skill dirs live-synced to this repo via `git pull`, rather than copying.)

## Plugin vs skills.sh — given I'm using Claude Code *and* Codex

| | skills.sh (`npx skills add`) | Claude Code plugin |
|---|---|---|
| Mechanism | Copies editable files into your project/global dirs | Installs a managed, read-only bundle via `/plugin install` |
| Editable? | Yes — hack them, fork them, make them yours | No — you don't touch the files; you get what Matt ships |
| Updates | Manual — you re-run the installer or `git pull` if symlinked | Automatic — bundle updates when Matt bumps plugin version |
| Cross-agent (Codex, others) | **Yes** — README explicitly says it installs into Codex and other "Agent-Skills-standard" harnesses today | **No** — Claude Code-specific, no Codex plugin yet (see `.agents/adr/0002-...`) |

Given I plan to work across Claude Code, Codex, and likely other LLMs, **the plugin route is a non-starter for anything beyond Claude Code** — it simply doesn't exist for other harnesses yet. `skills.sh` is the one path that's portable across agents, which matches my setup better.

One more option worth knowing: since I now have my own fork, I can point the installer at it instead of Matt's — `npx skills@latest add John-GitHub/mp-skills` — if I ever want to install my own customized versions rather than upstream's.
