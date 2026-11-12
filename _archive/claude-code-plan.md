# Claude Code — Stepwise Learning Plan

My personal course for learning Claude Code from zero.
Written against my install: **v2.1.245** · model `opus` · effort `medium` ·
permission mode `auto` · Postman plugin enabled · VS Code extension.

---

## How to use this

- **6 sessions, ~30–45 min each.** Do them in order, one sitting per session.
- Every step has **Do** (what to type) and **Expect** (how I know it worked).
- Don't read ahead. The drills are the learning; the prose is scaffolding.
- Stuck? Paste this file's step number to Claude: *"step 3.2 did X instead."*

### Progress

- [ ] Session 1 — The agent loop and how to talk to it
- [ ] Session 2 — Permission modes and plan mode
- [ ] Session 3 — Steering: interrupt, rewind, context
- [ ] Session 4 — Commands, prefixes, shortcuts
- [ ] Session 5 — `CLAUDE.md`
- [ ] Session 6 — Extending, config, and the wider tool

---

## The one-paragraph version

claude.ai is a chatbot: text in, text out, I copy-paste. Claude Code is an
**agent**: it lives in my project folder and has real tools — it reads and
writes files, runs terminal commands, searches the codebase, fetches web
pages, uses git. I give it an **outcome**, and it runs a loop: *gather context
→ act → verify → report*, looping again if verification failed. Everything in
this course is learning to steer that loop.

---

# SESSION 1 — The agent loop and how to talk to it

### Step 1.1 — Launch in the right place

- [ ] **Do:** `cd ~/MiraanLearning/Learning` then `claude`
- [ ] **Do:** ask `what files and folders are in this directory?`

**Expect:** it runs search tools and then answers. I see *tool calls*, not just
text — that's the difference between an agent and a chatbot.

**Learn:** Claude Code's world is the folder I launch it from. Never launch
from my home directory — it makes the context vague, huge and slow.

### Step 1.2 — Outcome, not keystrokes

- [ ] **Do:** ask `fix my code`
- [ ] **Do:** then ask something specific, naming a file and a symptom.

**Expect:** the vague one flails and interrogates me; the specific one acts.

**Learn:** state the finish line, not the steps.

```
BAD   "open main.py"
BAD   "write me a function"
GOOD  "add a --verbose flag to main.py that prints each step as it runs"
```

Finding the file is its job. Defining *done* is mine.

### Step 1.3 — Give it a verification step

- [ ] **Do:** ask for any small change phrased as
      `...then run it and confirm it prints X`

**Expect:** it edits, runs the thing, reads the output, and loops if it failed.

**Learn:** "and verify it" is the cheapest quality upgrade available.

```
BAD   "fix the bug"
GOOD  "fix the crash on empty input, then run it with an empty string
       and confirm it prints 'no input' instead of a traceback"
```

### Step 1.4 — Point at things

- [ ] **Do:** type `@AI_Engineer_Pathway/` + `Tab` to autocomplete a file, then
      ask about its contents
- [ ] **Do:** screenshot any error on my machine, paste with `Ctrl+V`, ask
      `what's causing this?`

**Expect:** it reads the attached file; it reads the image.

**Learn:**

| How | What |
|---|---|
| `@` + filename | attaches a file or folder (`Tab` autocompletes) |
| `Ctrl+V` | pastes an **image** — error screenshots, mockups, diagrams |
| a pasted URL | it fetches and reads the page |
| highlighting in VS Code | sent along automatically |

---

# SESSION 2 — Permission modes and plan mode

Every file write and every shell command passes a permission gate.

### Step 2.1 — Find the dial

- [ ] **Do:** press `Shift+Tab` repeatedly, watching the mode label at the
      bottom of the screen.

**The five modes:**

| Mode | Behaviour | Use when |
|---|---|---|
| `default` | asks before *every* write and command | new repo, learning |
| `auto` ← **mine** | auto-approves safe stuff, asks for risky stuff | daily driver |
| `plan` | **read-only — cannot change anything** | anything non-trivial |
| `accept edits` | auto-accepts edits, still gates commands | known refactor |
| `bypass permissions` | approves everything ⚠️ | throwaway sandboxes only |

When it asks permission, one option is *"yes, and don't ask again for this
command"* — that writes a persistent rule so it stops nagging about
`git status` forever.

### Step 2.2 — Prove plan mode is read-only

- [ ] **Do:** `Shift+Tab` to **plan mode**, then ask
      `create a file called test.txt containing hello`
- [ ] **Do:** verify with `!ls`

**Expect:** no file created. It tells me what it *would* do instead.

### Step 2.3 — Same task, unlocked

- [ ] **Do:** `Shift+Tab` back to `auto`, ask again, then delete the file.

**Expect:** it creates it this time.

**Learn:** the mode is the safety dial. **Check it first when edits aren't
happening.**

### Step 2.4 — Plan mode as the real workflow

- [ ] **Do:** in plan mode, ask
      `read my Claude Code settings and tell me what's configured`

**Expect:** it reads `~/.claude/settings.json`, reports model / effort / mode /
plugins, and changes nothing.

**The workflow:**

```
Shift+Tab → plan mode
   ↓
"I want to add login to this app. Explore the code and propose how."
   ↓
Claude reads everything, asks clarifying questions, writes a plan
   ↓
I read the plan.  "No — one database table, not three."
   ↓
Approve  →  it executes the agreed plan
```

**Learn the habit:** anything bigger than a one-liner starts in plan mode.
Fixing a wrong *plan* costs one sentence; fixing wrong *code* costs an hour.
And because it physically cannot modify anything, I can explore an unfamiliar
codebase with zero risk.

---

# SESSION 3 — Steering: interrupt, rewind, context

### Step 3.1 — Interrupt

- [ ] **Do:** ask for a 2000-word essay on anything. After ~3 seconds press
      **`Esc`**.
- [ ] **Do:** then say `actually just give me 3 bullets`

**Learn:** interrupting is free. Letting it build on a wrong premise is not.
Hit `Esc` the *moment* I see it going sideways — don't sit politely. Plain
disagreement works too: *"no, that's wrong, I meant X."* Course-correcting is
the normal mode of operation, not a failure.

### Step 3.2 — Rewind

- [ ] **Do:** press **`Esc` `Esc`**, pick an earlier message, send something
      different from that point.
- [ ] **Do:** run `/rewind` and read the checkpoint list.

**Learn:** `Esc Esc` rewinds the conversation. `/rewind` restores the
conversation **and my files** to a checkpoint. It's the undo button for a
session that went wrong.

### Step 3.3 — See your context

- [ ] **Do:** `/context` — study the breakdown.
- [ ] **Do:** `/clear`, then `/context` again. Compare the numbers.
- [ ] **Do:** ask something that depended on the old conversation. It won't
      know — **that's the point.** I traded memory for focus, deliberately.

**Learn — the #1 habit:**

> ### One task per session. Finish → `/clear` → next task.

Context is everything in the current conversation, and the window is finite.
Two ways to fail: **too little** context (it guesses wrong) and **too much
junk** (it gets distracted, slow, expensive, and measurably worse). A
200-message session that wandered across five unrelated tasks performs
*worse than a brand-new one*. Context is also what I pay for on every turn.

**Also know:**

| Command | Does |
|---|---|
| `/clear` | wipes context — use constantly |
| `/compact` | summarises to free space, keeps the thread alive |
| `/context` | shows what's eating the window — run it when it feels dumb |
| `claude -c` | continue the most recent conversation in this folder |
| `/resume` · `claude -r` | reopen a past session |
| `/export` | export the conversation |

---

# SESSION 4 — Commands, prefixes, shortcuts

### Step 4.1 — Tour the commands

- [ ] **Do:** `/help`, then type `/` and arrow slowly through the whole list.
- [ ] **Do:** `/status` · `/cost` · `/model` (look, don't switch yet)

**Groups worth knowing:**

- **Context** — `/clear` `/compact` `/context` `/rewind` `/resume` `/export`
- **Setup** — `/init` `/memory` `/config` `/model` `/permissions` `/status`
  `/doctor` `/cost`
- **Work** — `/review` `/code-review` `/security-review` `/agents` `/mcp`
  `/plugin`
- **Terminal** — `/terminal-setup` (fixes `Shift+Enter`) `/vim`

### Step 4.2 — The two prefix keys

- [ ] **Do:** `!ls -la` and `!python3 --version`
- [ ] **Do:** `# I am a beginner, explain your reasoning as you go`

**Learn:** these are line prefixes, not commands.

| Prefix | Does |
|---|---|
| `!` | runs a shell command; output lands in the conversation |
| `#` | saves a fact to memory (Session 5 covers where it goes) |

### Step 4.3 — Shortcut drill

- [ ] **Do:** use every one of these at least once in a single session.

| Key | Action |
|---|---|
| `Esc` | interrupt |
| `Esc` `Esc` | rewind / edit a previous message |
| `Shift+Tab` | cycle permission mode |
| `Ctrl+C` ×2 | quit |
| `Ctrl+L` | clear the **screen** (not the context) |
| `Ctrl+R` | search command history |
| `Ctrl+V` | paste an image |
| `Shift+Enter` | newline without sending |
| `↑` | previous prompts |
| `Tab` | autocomplete files / commands |
| `@` `/` `!` `#` | file picker · command picker · shell · memory |

- [ ] If `Shift+Enter` doesn't give a newline: run `/terminal-setup`.

**Learn:** muscle memory beats a printed table.

---

# SESSION 5 — `CLAUDE.md`

**My biggest gap — I currently have none.**

`CLAUDE.md` is a plain markdown file **auto-loaded into context at the start of
every session** in that folder. It's how I stop repeating myself forever.

| File | Scope |
|---|---|
| `~/.claude/CLAUDE.md` | me, every project on this machine |
| `./CLAUDE.md` (folder **root**) | this project — commit it, team shares it |
| `./CLAUDE.local.md` | this project, private to me — gitignore it |

⚠️ It must be at the folder **root** to auto-load. A `CLAUDE.md` buried in a
subfolder only loads when files in that subfolder are touched. To keep content
elsewhere, the root file can just import it: `@subfolder/rules.md`.

### Step 5.1 — Generate one and read it

- [ ] **Do:** `/init` in a project folder.
- [ ] **Do:** open the `CLAUDE.md` it produced and read every line.

**Expect:** build/run/test commands, an architecture summary, conventions.
That's my model of what a good one looks like.

### Step 5.2 — Find my saved memory

- [ ] **Do:** `/memory` — see which files are loaded, and edit them.

**Expect:** the `#` line from Step 4.2 is in there.

### Step 5.3 — Prove it persists

- [ ] **Do:** `/clear`, then ask a question.

**Expect:** it already knows my preference with no reminder from me.

**Learn:** `CLAUDE.md` configures **behaviour**, not just facts. That's the
real power.

```markdown
## How I want you to work here
- I'm learning. Explain your reasoning as you go.
- Ask me a leading question before handing me the answer.
- Never commit the .env file.
```

**What belongs in it:** how to run/build/test, the architecture in five lines,
my conventions, gotchas, and **anything I've had to correct twice**.

| Tool | Does |
|---|---|
| `/init` | reads the project, writes a first-draft `CLAUDE.md` |
| `/memory` | opens memory files to edit, shows which are loaded |
| `#` | appends a line inline |

---

# SESSION 6 — Extending, config, and the wider tool

Four levels of customisation, simplest first.

### Step 6.1 — A custom slash command (easiest extension)

- [ ] **Do:** create `.claude/commands/eli5.md` containing:
      `Explain $ARGUMENTS as if I'm 10 years old. Use an analogy.`
- [ ] **Do:** run `/eli5 what an API key is`
- [ ] Delete it afterwards if I like.

**Learn:** a custom slash command is just a markdown file. `$ARGUMENTS` is
whatever I type after the command name.

### Step 6.2 — Skills, subagents, plugins

- [ ] **Do:** `/agents` — look at the built-ins (`Explore`, `Plan`,
      `general-purpose`, `claude-code-guide`).
- [ ] **Do:** ask `use the claude-code-guide agent to explain hooks to me`
- [ ] **Do:** `/plugin` — browse the marketplace. My Postman plugin is one.

**Expect:** a subagent gets dispatched and reports back a conclusion.

**Learn:**

- **Skills** are packaged workflows at `.claude/skills/<name>/SKILL.md`, loaded
  *on demand* so they cost nothing until used. I already have `/code-review`,
  `/security-review`, `/simplify`, `/update-config`, `/dataviz`.
- **Subagents** each get their **own fresh context window**, so heavy research
  happens over there and only the conclusion comes back — my main conversation
  stays clean. Define my own in `.claude/agents/*.md`.
- **Plugins** bundle skills + agents + hooks + MCP servers for distribution.

### Step 6.3 — Settings and the `.env` guard

- [ ] **Do:** `cat ~/.claude/settings.json` — read my own config now that I
      understand the fields.
- [ ] **Do:** ask `use the update-config skill to deny reads of any .env file`,
      approve it, then read the file again.
- [ ] **Do:** `/permissions` — browse my allow/deny rules.

**Learn:** three files, merged — `~/.claude/settings.json` (me),
`.claude/settings.json` (project, committed), `.claude/settings.local.json`
(project, private).

```json
{
  "model": "opus",
  "permissions": {
    "defaultMode": "auto",
    "allow": ["Bash(git status)", "Bash(pytest *)"],
    "deny": ["Read(**/.env)", "Bash(rm -rf *)"]
  }
}
```

That `deny` on `.env` means Claude **cannot read my API keys at all**, ever.
Don't hand-edit these files — use `/update-config`, which knows the schema.

### Step 6.4 — Hooks (guaranteed behaviour)

- [ ] **Read:** hooks are shell commands the harness runs at lifecycle points —
      `PreToolUse`, `PostToolUse`, `SessionStart`, `Stop`, `UserPromptSubmit`.
      Unlike instructions, these **always** fire. They're enforcement, not
      persuasion. A `PreToolUse` hook can **block** a tool call outright.

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{ "type": "command", "command": "black $CLAUDE_FILE_PATHS" }]
    }]
  }
}
```

→ auto-formats every Python file Claude touches.

- [ ] **Optional do:** add an echo hook, watch it fire on an edit, remove it.

### Step 6.5 — MCP, and fix my live warning

- [ ] **Do:** `/mcp` — my **Postman server is unauthenticated**; that's the
      warning at session start. Authorise it here, or disable the plugin.

**Learn:** MCP (Model Context Protocol) is the open standard for connecting
external systems — GitHub, Postgres, Sentry, Jira, Figma, Slack, my own
services. Each server exposes tools Claude can call.

```bash
claude mcp add --transport http github https://api.githubcopilot.com/mcp/
claude mcp list
```

then `/mcp` in-session to authenticate. Config lives in `.mcp.json` (project,
shareable) or user scope.

⚠️ **Security:** MCP servers take real actions on real systems and return text
Claude reads. Install only what I trust.

### Step 6.6 — Git, headless, parallelism, models

- [ ] **Do (git):** in any git repo, ask
      `what changed in the last 3 commits and why?`

Other strong prompts: `commit this` (reads the diff, writes a real message) ·
`open a PR` (uses the `gh` CLI) · `why is this line written this way?`
(log/blame archaeology) · `rebase onto main and fix the conflicts` ·
`review PR 142`. **It never commits or pushes unless I ask.**

- [ ] **Do (headless):** from a **plain terminal**, not inside a session:

```bash
claude -p "list the top-level folders here and what each is for"
echo "def f(x): return x*2" | claude -p "explain this in one sentence"
cat error.log | claude -p "what's the root cause?"
git diff | claude -p "write a commit message"
```

**Learn:** `-p` runs one prompt non-interactively and exits — Claude Code as a
unix tool I can pipe and script. It's also the foundation of the **Claude Agent
SDK** (Python/TypeScript) if I ever want to *build* agents rather than just use
one.

- [ ] **Read (parallelism):** `claude --bg "..."` background agent ·
      `claude -w name` own git worktree · `claude --cloud "..."` runs on
      Anthropic's machines · or just open several terminal tabs.

- [ ] **Read (models):**

| Model | Character |
|---|---|
| **Opus 5** ← mine | most capable — hard reasoning, big refactors |
| **Sonnet 5** | fast, strong, cheaper — great default for routine work |
| **Haiku 4.5** | fastest — simple mechanical edits |

`/model` to switch · `--effort low…max` (mine: `medium`) controls thinking
depth · `/cost` for spend · `/fast` = faster Opus output, *not* a weaker model.
**The best cost control is `/clear`** — I pay for context on every single turn.

---

# REFERENCE — keep this after finishing

## Troubleshooting

| Symptom | Fix |
|---|---|
| Confused, repetitive, feels dumb | **`/clear`** — 9/10 times it's context rot |
| Won't edit files | `Shift+Tab` — probably in plan mode |
| Slow | `/context` — something huge is loaded |
| Broken config, bizarre behaviour | `claude --safe-mode` (disables all customisation), then `/doctor` |
| `Shift+Enter` doesn't newline | `/terminal-setup` |
| MCP tools missing | `/mcp` — unauthenticated |
| Wrong answer *about Claude Code itself* | ask for the `claude-code-guide` agent — it reads live docs |

## Habits

**Do**

1. `cd` into the project before launching.
2. Write a `CLAUDE.md`. Add a line every time I correct the same thing twice.
3. `Shift+Tab` → plan mode for anything non-trivial.
4. `/clear` between tasks. Religiously.
5. Hit `Esc` early and often.
6. Always give it a way to verify — a test, a command, an expected output.
7. `@` my files. Paste screenshots of errors.
8. **Ask it to explain.** *"Why did you do it that way?"* is an excellent
   prompt.

**Don't**

1. Don't run one giant session all week.
2. Don't accept code I don't understand — I'm learning; demand the walkthrough.
3. Don't use bypass-permissions on anything I care about.
4. Don't write vague prompts and then blame the output.
5. Don't skip reading the diff. **I** am the reviewer.

## CLI quick reference

```bash
claude                     # interactive session in current folder
claude "fix the bug"       # start with a prompt
claude -c                  # continue most recent conversation here
claude -r                  # resume: picker of past sessions
claude -p "..."            # headless: one prompt, print, exit
claude -w name             # new git worktree for this session
claude --bg "..."          # background agent (manage: claude agents)
claude --cloud "..."       # run on Anthropic's infrastructure
claude --safe-mode         # disable ALL customisation (troubleshooting)
claude --model sonnet      # pick a model
claude --effort high       # thinking depth: low|medium|high|xhigh|max
claude --tools "Read,Grep" # restrict available tools
claude update              # upgrade
claude doctor              # health check
claude mcp list            # MCP servers
claude plugin              # plugins
```

## Links

**Official docs** — authoritative and genuinely well written. All pages live at
`https://docs.claude.com/en/docs/claude-code/<page>`:

`overview` · `quickstart` · `common-workflows` · `cli-reference` ·
`interactive-mode` · `slash-commands` · `memory` · `settings` · `sub-agents` ·
`skills` · `plugins` · `hooks` · `mcp` · `ide-integrations` ·
`github-actions` · `troubleshooting` · `security` · `costs`

- ⭐ **Best practices — read this twice** —
  https://www.anthropic.com/engineering/claude-code-best-practices
- Agent SDK — https://docs.claude.com/en/api/agent-sdk/overview

**Video & courses**

- Anthropic YouTube channel — https://www.youtube.com/@anthropic-ai
- Anthropic Academy (free structured courses) — https://www.anthropic.com/learn
- DeepLearning.AI short course, *Claude Code: A Highly Agentic Coding
  Assistant* — https://www.deeplearning.ai/short-courses/
- YouTube search — **sort by Upload date**; this tool changes monthly, so skip
  anything older than ~3 months:
  - https://www.youtube.com/results?search_query=claude+code+tutorial+beginner
  - https://www.youtube.com/results?search_query=claude+code+CLAUDE.md+hooks+subagents

**Community**

- Repo & issues — https://github.com/anthropics/claude-code
- Changelog (what's new) —
  https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- Awesome Claude Code (curated commands, agents, hooks) —
  https://github.com/hesreallyhim/awesome-claude-code
- Discord — https://anthropic.com/discord

---

*Claude Code ships fast — flags and command names do change. When something in
this file doesn't match reality, `claude --help` and the docs above are the
authority. Written against v2.1.245.*
