# ai-tool-research

> A portable [Agent Skill](https://agentskills.io) that researches how people are actually using one of five AI tools over the past month and produces a **Productivity Playbook** + a **Skills Catalog** in a consistent, rated, diff-friendly format.

Runs in **Cursor · Claude Desktop · ChatGPT · OpenAI Codex · Gemini CLI · OpenClaw** — anywhere that honors the `agentskills.io` open `SKILL.md` standard, or as a pasted prompt in any chat UI.

---

## What it does

Given one of these five tools (or all of them), it outputs two Markdown files:

| File | What it contains |
|---|---|
| `<Tool>-Productivity-Playbook.md` | Persona-sorted real-world workflows, case studies, unusual use cases, links, and a 4-week starter plan |
| `<Tool>-Skills-Catalog.md` | Rated list of skills / rules / plugins / MCP servers, persona-mapped, with install-ready starter kits |

Supported tools:

- `claude` — Claude Desktop + Claude Code + Anthropic Skills
- `cursor` — Cursor AI IDE (Rules, Skills, Plugins, MCP, Notepads)
- `codex` — OpenAI Codex (CLI + IDE + App + Cloud)
- `gemini` — Google Gemini app + Gemini CLI + Code Assist + NotebookLM + Gems
- `openclaw` — Peter Steinberger's OpenClaw local AI agent
- `all` — runs the cycle for all five, producing ten files

Designed for **monthly cadence** — a built-in run log (`research-log.md`) tracks the last run date so each monthly regeneration produces a clean `git diff` of "what actually changed".

---

## Why this exists

Manually rewriting a multi-tool AI productivity playbook every month is ~3–6 hours of work that nobody should have to do twice. This skill automates the structure, the research queries, the rating rubric, and the output contract — you just run it.

The output files follow a **fixed section order and heading shape** so that `git diff` between two monthly runs actually tells you what changed in the AI tools ecosystem, not what the author felt like restructuring.

---

## How to run

The skill is designed to drop into any AI agent runtime that understands the [`agentskills.io`](https://agentskills.io) open `SKILL.md` standard, plus work as a pasted prompt in any chat UI.

### Inputs the skill accepts

Every invocation takes the same four inputs. You can pass them explicitly or let the skill infer defaults:

| Input | Values | Default | Notes |
|---|---|---|---|
| `tool` | `claude` · `cursor` · `codex` · `gemini` · `openclaw` · `all` | — (required) | Which AI ecosystem to research |
| `since_date` | `YYYY-MM-DD` | Last entry in `research-log.md`, or `today - 31 days` on first run | Lower bound of the research window |
| `output_dir` | any path | Current working directory | Where the two output files land |
| `existing_file_mode` | `rewrite` · `append-appendix` · `fail` | `append-appendix` if file exists, else `rewrite` | How to treat existing playbook / catalog |

---

### 1. Cursor (recommended — first-class support)

#### Install

**Project-local** (just this workspace):

```powershell
# Windows PowerShell — from the parent folder that contains ai-tool-research-skill
New-Item -ItemType Directory -Force -Path .cursor\skills | Out-Null
Copy-Item -Recurse -Force ai-tool-research-skill .cursor\skills\ai-tool-research
```

```bash
# macOS / Linux
mkdir -p .cursor/skills
cp -r ai-tool-research-skill .cursor/skills/ai-tool-research
```

**Personal** (shared across every Cursor project on this machine):

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force -Path $HOME\.cursor\skills | Out-Null
Copy-Item -Recurse -Force ai-tool-research-skill $HOME\.cursor\skills\ai-tool-research
```

```bash
# macOS / Linux
mkdir -p ~/.cursor/skills
cp -r ai-tool-research-skill ~/.cursor/skills/ai-tool-research
```

Reload Cursor (`Cmd/Ctrl+Shift+P` → **Developer: Reload Window**). The skill should appear under Settings → **Rules** → *Agent Decides*.

#### Invoke

Ask Cursor in chat (Agent Mode preferred):

```text
Run the ai-tool-research skill for Cursor this month.
```

Or with explicit inputs:

```text
Run the ai-tool-research skill for Cursor with
since_date = 2026-03-21 and existing_file_mode = append-appendix.
```

Cursor will:

1. Read `SKILL.md` and the five support files on demand.
2. Execute the 9-step workflow (confirm inputs → check existing files → research → rate → compose → verify → write → log).
3. Write `Cursor-Productivity-Playbook.md` + `Cursor-Skills-Catalog.md` into `output_dir` and append a row to `research-log.md`.

#### Recommended test run

Confirms your setup works end-to-end on a small window:

```text
Run ai-tool-research for Cursor with since_date = 2026-04-14 and existing_file_mode = append-appendix.
```

You should see a short appendix added to both Cursor files plus a new row in `research-log.md`.

---

### 2. Claude Desktop / Claude Code

#### Install

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force -Path $HOME\.claude\skills | Out-Null
Copy-Item -Recurse -Force ai-tool-research-skill $HOME\.claude\skills\ai-tool-research
```

```bash
# macOS / Linux
mkdir -p ~/.claude/skills
cp -r ai-tool-research-skill ~/.claude/skills/ai-tool-research
```

Open Claude Desktop → **Settings** → **Skills** → enable `ai-tool-research`.

#### Invoke

```text
Use the ai-tool-research skill to update the Codex playbook for last month.
```

Or with explicit inputs:

```text
Use ai-tool-research for claude with since_date = 2026-03-21 and mode = rewrite.
Output files to the current project folder.
```

---

### 3. OpenAI Codex (CLI + IDE + App)

#### Install

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force -Path $HOME\.codex\skills | Out-Null
Copy-Item -Recurse -Force ai-tool-research-skill $HOME\.codex\skills\ai-tool-research
```

```bash
# macOS / Linux
mkdir -p ~/.codex/skills
cp -r ai-tool-research-skill ~/.codex/skills/ai-tool-research
```

Optionally reference it in your project's `AGENTS.md` for auto-loading:

```markdown
## Available skills
- `ai-tool-research` — monthly regeneration of AI-tool playbooks + catalogs.
```

#### Invoke

In the Codex CLI or IDE agent:

```text
Run ai-tool-research for codex this month.
```

---

### 4. Gemini CLI / Gemini Code Assist

#### Install

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force -Path $HOME\.gemini\skills | Out-Null
Copy-Item -Recurse -Force ai-tool-research-skill $HOME\.gemini\skills\ai-tool-research
```

```bash
# macOS / Linux
mkdir -p ~/.gemini/skills
cp -r ai-tool-research-skill ~/.gemini/skills/ai-tool-research
```

#### Invoke

```text
Run ai-tool-research for gemini with since_date = 2026-03-21.
```

---

### 5. OpenClaw

#### Install

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force -Path $HOME\.openclaw\workspace\skills | Out-Null
Copy-Item -Recurse -Force ai-tool-research-skill $HOME\.openclaw\workspace\skills\ai-tool-research
```

```bash
# macOS / Linux
mkdir -p ~/.openclaw/workspace/skills
cp -r ai-tool-research-skill ~/.openclaw/workspace/skills/ai-tool-research
```

#### Invoke

From any connected messaging channel (WhatsApp / Telegram / Slack / Signal / iMessage / Discord / MS Teams):

```text
Run the ai-tool-research skill for all five tools and DM me a summary.
```

Because OpenClaw runs locally with filesystem access, it can write all ten output files in one pass — this is the fastest way to do a full monthly cycle unattended.

---

### 6. ChatGPT (web UI — no filesystem)

ChatGPT can't install skills, but it can still follow the spec when pasted in.

1. Start a new conversation.
2. Paste `SKILL.md` as the first message, prefixed with:
   > *You are an agent following this skill definition. Apply it to my next request.*
3. In follow-up turns, paste each support file: `playbook-template.md`, `catalog-template.md`, `personas.md`, `rating-system.md`, `research-queries.md`.
4. Ask:
   ```text
   Run the skill for Gemini this month. since_date = 2026-03-21. mode = rewrite.
   ```
5. ChatGPT will return the two Markdown files as assistant messages. Copy each into a local `.md` file manually.

Note: ChatGPT's web search is less comprehensive than Cursor/Claude Desktop web tools — expect shallower coverage of forum threads and X posts.

---

### 7. Any other `SKILL.md`-aware runtime

Anywhere else (generic MCP client, a custom agent you wrote, a third-party IDE plugin that supports `agentskills.io`):

1. Drop the `ai-tool-research-skill` folder somewhere the runtime scans for skills.
2. Invoke by name: `ai-tool-research`.
3. Pass the four inputs (`tool`, `since_date`, `output_dir`, `existing_file_mode`) in whatever way your runtime accepts them.

---

### What you'll see during a run

A successful run produces exactly these artifacts:

```text
<output_dir>/
├── <Tool>-Productivity-Playbook.md    ← new or appended
├── <Tool>-Skills-Catalog.md           ← new or appended
└── research-log.md                    ← new row appended
```

And a short chat summary with: date run, tool, `since_date`, mode, count of new skills / new use cases, and 1–3 notable changes in the window.

---

### Troubleshooting

| Symptom | Fix |
|---|---|
| Skill doesn't appear in Cursor's Rules panel | Reload window (`Cmd/Ctrl+Shift+P` → *Developer: Reload Window*) after copying the folder. |
| Claude Desktop says "skill not found" | Skills must be explicitly enabled in *Settings → Skills*. Toggle it on. |
| Agent produces generic content, not recent | Your `since_date` is probably too wide. Tighten it (e.g. last 30 days). Also confirm your runtime has web search enabled. |
| Output files clobbered when you wanted to keep history | Use `existing_file_mode = append-appendix`. The default behavior is `append-appendix` when files exist, but some agents pick `rewrite` — be explicit. |
| `research-log.md` not updating | The skill appends to whatever `output_dir` you passed. Check it landed there, not in the skill folder itself. |

---

## Repository structure

```text
ai-tool-research/
├── README.md              ← this file
├── SKILL.md               ← main skill (orchestrator, under 500 lines)
├── playbook-template.md   ← exact section structure + tone rules for playbooks
├── catalog-template.md    ← exact section structure for skills catalogs
├── personas.md            ← the 8 fixed personas + coverage floor
├── rating-system.md       ← ✅/🟢/🟡/🔴 + ★ rubric with decision flowcharts
└── research-queries.md    ← per-tool primary sources, power-user X handles, search templates
```

Progressive disclosure: `SKILL.md` is the only file the agent loads eagerly. The others are read only when the relevant workflow step requires them — this keeps the context window lean.

---

## How monthly cadence works

1. **First run** creates `research-log.md` with the date and counts.
2. **Subsequent runs** default `since_date = last_run_date` from the log, so each regeneration only needs to surface the delta.
3. Every generated file has `Last updated: YYYY-MM-DD` and `Research window: <since_date> → <today>` at the top so you can always verify the time box.
4. Every catalog uses a fixed section order and alphabetized tables — `git diff` between two months shows you exactly which skills were added, which changed rating, and which were dropped.

Example `research-log.md` (auto-appended):

```markdown
| Date run | Tool | since_date | Mode | New skills | New use cases | Notable changes |
|---|---|---|---|---|---|---|
| 2026-04-21 | cursor | 2026-03-21 | rewrite | 14 | 9 | Composer 2 GA; 3 new MCP servers |
| 2026-05-21 | cursor | 2026-04-21 | rewrite | 8  | 6 | Bugbot Autofix expanded to 12 languages |
```

---

## Example invocations

```text
Run ai-tool-research for Cursor this month.

Run ai-tool-research for all five tools since 2026-03-21.

Use ai-tool-research to append an "Updates since last run" appendix to the
existing OpenClaw playbook instead of rewriting it.

Use ai-tool-research to rebuild Gemini-Skills-Catalog.md with fresh ratings.
```

---

## What makes the output good

- **Rated, not hyped** — every skill gets a two-dimensional rating (validity + usefulness) with target distribution so the ratings actually differentiate. If you find ★★★★★ on everything, the rating system is broken; `rating-system.md` enforces this.
- **Persona-sorted, not feature-sorted** — eight fixed personas covered every run, in the same order, so month-over-month content diffs are meaningful.
- **Cross-tool aware** — skills that honor `agentskills.io` and work across ≥ 2 ecosystems are called out specifically, so installing once gives you value in multiple tools.
- **Safety non-optional** — every playbook and catalog includes a dedicated safety section covering sandboxing, approval flows, audit tooling, and cost guardrails.
- **Diff-friendly** — stable heading text, stable table order, stable date format. Designed for `git log -p`.

---

## The eight personas

Every playbook covers all eight, in this fixed order (see [`personas.md`](./personas.md) for detail):

1. PhD / research student
2. Solopreneur / startup founder
3. Marketer / growth / agency
4. Designer (app / web / brand)
5. Video / podcast / creator
6. Software developer
7. Personal productivity / PKM / student
8. Sales / finance / ops / back-office

---

## Rating system at a glance

**Validity** — is it real + maintained?

| Badge | Meaning |
|---|---|
| ✅ **Verified** | Official or actively maintained with strong adoption |
| 🟢 **Likely-valid** | Confirmed repo, moderate traction, recent activity |
| 🟡 **Early / Niche** | Real but new or low-traction |
| 🔴 **Unverified** | Mentioned but canonical source couldn't be confirmed |

**Usefulness** (editorial) — ★★★★★ must-install · ★★★★ weekly use · ★★★ niche but excellent · ★★ narrow fit · ★ experimental

Target distribution across a mature catalog: only 5–10% of entries should earn ★★★★★. Full criteria + decision flowchart in [`rating-system.md`](./rating-system.md).

---

## Compatibility

This skill is intentionally runtime-agnostic. It uses only standard `SKILL.md` frontmatter + Markdown body — no tool-specific features, no hard-coded paths, no proprietary dependencies.

| Runtime | Tested? | Notes |
|---|---|---|
| Cursor | Yes | Project or personal skills folder |
| Claude Desktop | Yes | Requires Skills enabled in settings |
| Claude Code | Yes | Drop into `~/.claude/skills/` |
| ChatGPT (web) | Yes | Paste `SKILL.md` + support files as context |
| OpenAI Codex (CLI) | Yes | `~/.codex/skills/` |
| OpenAI Codex (IDE) | Yes | Same folder as CLI |
| Gemini CLI | Yes | `~/.gemini/skills/` |
| OpenClaw | Yes | `~/.openclaw/workspace/skills/` |
| Generic MCP client | Should work | Any client that loads `SKILL.md` files |

---

## Customizing

Each of the five support files is designed to be edited without breaking the skill:

- **`personas.md`** — swap, reorder, or add personas. Keep the numbering stable month-over-month within your own fork.
- **`rating-system.md`** — tune the thresholds if your context is different (e.g., focus only on enterprise tools).
- **`research-queries.md`** — add your own power-user X handles, Reddit subs, RSS feeds, or Discord invites.
- **`playbook-template.md`** / **`catalog-template.md`** — add or remove sections. The parent `SKILL.md` references section numbers so tweak both.

Do not edit `SKILL.md`'s YAML frontmatter (`name`, `description`) unless you fork — changing those breaks skill discovery across runtimes.

---

## Known limitations

- Depends on the host agent having web-search capability to do real research. In runtimes without web access, it degrades to producing updates from whatever context the user provides manually.
- Not a replacement for editorial judgment. The skill structures the work and enforces the rating system, but the final quality depends on the agent model doing the research.
- Rate limits on X.com / Reddit / GitHub searches can bound how much the agent surfaces in a single run. The skill explicitly tells the agent to state where it hit limits.
- The "all five tools" mode takes 20–60 minutes depending on model speed and source breadth. For faster runs, target one tool at a time.

---

## Contributing

Pull requests welcome. Good contributions:

- New primary sources in `research-queries.md` (especially non-English AI communities — the ecosystem outside the US/EU is under-represented in this skill)
- Edge-case personas that warrant their own section
- Refinements to the rating rubric decision flow
- Additional runtime install instructions (e.g., new MCP clients, IDE plugins)

Please keep `SKILL.md` under 500 lines — use progressive disclosure into the support files for anything longer.

---

## Credits

Built during a multi-month research project covering Claude Desktop, Cursor, OpenAI Codex, Google Gemini, and OpenClaw. The skill distills the structure, research queries, and rating rubric that emerged during that work into a reusable template so nobody has to redo it manually.

Thanks to:

- The [`agentskills.io`](https://agentskills.io) open standard for making cross-tool skill portability possible.
- Anthropic's `anthropics/skills` and `obra/superpowers` for setting the bar on what a foundation skill should look like.
- Peter Steinberger for demonstrating (via OpenClaw + ClawHub) that a skills marketplace can scale to 13,729+ entries without centralized curation.
- The `VoltAgent/awesome-openclaw-skills`, `PatrickJS/awesome-cursorrules`, `Piebald-AI/awesome-gemini-cli-extensions`, and `ComposioHQ/awesome-claude-skills` projects for showing what good community curation looks like.

---

## License

MIT. Do whatever you want with it. If you use it to produce publicly shared playbooks, a link back is appreciated but not required.

---

## Related catalogs generated by this skill

If you already used the skill to build a full catalog set, the companion files live alongside:

- `Claude-Desktop-Productivity-Playbook.md` + `Claude-Skills-Catalog.md`
- `Cursor-Productivity-Playbook.md` + `Cursor-Skills-Catalog.md`
- `Codex-Productivity-Playbook.md` + `Codex-Skills-Catalog.md`
- `Gemini-Productivity-Playbook.md` + `Gemini-Skills-Catalog.md`
- `OpenClaw-Productivity-Playbook.md` + `OpenClaw-Skills-Catalog.md`

---

*Built with the same spec it uses to build others.*
