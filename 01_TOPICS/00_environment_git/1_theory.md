# Environment, Git & GitHub — Theory & Explanation

**Day 1** · Level 1

---

## 1. Checking the environment

Before writing any code, I checked what tools are already on my computer — like checking the kitchen before cooking.

- **Python** — the language used for almost everything in this course. Checked with `python3 --version` → got `Python 3.13.5` (well above the 3.11+ minimum).
- Important gotcha: plain `python` does not exist on this machine — only `python3`. This is normal on modern Linux systems. Python 2 reached end-of-life in Jan 2020, so many systems removed the plain `python` command entirely to avoid confusion between Python 2 and 3. **Always type `python3` explicitly.**
- **VS Code** — the code editor, already installed.
- **Terminal** — where I type commands instead of clicking. Already in use.

## 2. `venv` — virtual environments

**The problem:** two different projects on the same computer might need two different versions of the same library. The computer can normally only hold one version installed at a time, so installing one breaks the other.

**The fix — `venv`:** creates a separate, sealed folder just for one project. Libraries installed inside it do not affect anything outside it, and do not affect other projects.

```
      python3 -m venv venv        → creates the sealed box (a folder named venv/)
      source venv/bin/activate    → step INSIDE the box (prompt shows "(venv)")
      deactivate                  → step back out
```

Activating only changes the current terminal session — it rewrites `PATH` (the list of folders the terminal searches for programs) so `python`/`pip` point inside `venv/`, not the system-wide ones.

## 3. `pip` — the package manager

`pip` downloads libraries ("packages") and installs them inside the currently active `venv`.

| Command | What it does |
|---|---|
| `pip install <name>` | Downloads and installs a package, plus anything it depends on |
| `pip list` | Shows every installed package, human-readable table |
| `pip freeze` | Shows every installed package in `name==version` format — the format `pip install` can read back in |
| `pip freeze > requirements.txt` | Saves that list to a file, so the exact environment can be rebuilt later |
| `pip install -r requirements.txt` | Installs everything listed in that file |

**Dependency:** a library that another library needs to work. Installing `requests` pulled in 4 extra packages it depends on: `urllib3`, `idna`, `charset_normalizer`, `certifi`.

Note: `pip list` shows `pip` itself as a package, but `pip freeze` does not — `pip` and a couple of base tools are excluded from `freeze` on purpose, since every fresh `venv` already includes them.

### Proof: the destroy-and-rebuild test

Believing `requirements.txt` works is not the same as proving it. The real test:

1. `deactivate` then `rm -rf venv` — destroy the environment completely.
2. `python3 -m venv venv` then `source venv/bin/activate` — build a brand new, empty one.
3. `pip install -r requirements.txt` — rebuild using only the saved file.
4. `pip list` again — confirm it matches exactly.

**Result:** all 5 packages came back at the exact same pinned versions. This proves `requirements.txt` is a complete, accurate record of the environment — this matters because a teammate or a production server rebuilds from this file alone, with no other hidden setup steps.

## 4. `.env` and `.gitignore` — and why the ORDER matters

**`.env`** — a file that holds secret values (API keys, passwords) as `KEY=value` pairs, kept separate from the actual code. Code reads secrets from this file instead of having them hardcoded, so the secret is never visible inside the source code itself.

**`.gitignore`** — tells Git "never save these files into history." I put `venv/`, `__pycache__/`, `*.pyc`, and `.env` inside it.

**The dangerous rule: `.gitignore` must exist BEFORE `.env` is ever committed, even once.**

`.gitignore` only affects *future* commits. It has no power over the past. If a secret file gets committed even one single time, it is permanently saved inside Git's history. Removing it afterward needs an advanced, destructive history-rewrite — and if it was ever pushed to GitHub, the secret should be treated as already leaked and replaced.

**Real example:** this exact mistake happened on the real ScreenerAI project — its `.env` got committed before `.gitignore` existed. This Learning repo's `.gitignore` was created first, deliberately, before any `.env` file, to avoid repeating that mistake.

## 5. Git — why version control exists

Without a tool like Git, tracking changes over time means manually copying folders ("project_v1," "project_final_FINAL") — messy, and it never shows exactly what changed.

**Git** saves snapshots of code over time, called **commits**. You can see exactly what changed and when, go back to any old snapshot, and multiple people can work on the same project without overwriting each other.

Git is a **distributed version control system** — every developer has the *entire* history on their own machine, not just the latest version.

### The three rooms (three-stage model)

1. **Working directory** — the normal folder, where files are edited.
2. **Staging area** (the "index") — a waiting room. `git add` moves a changed file here, choosing what goes into the next snapshot.
3. **Repository** — the permanent history. `git commit` saves whatever is staged, forever, as a new snapshot.

This staging step is what makes Git different from a simple backup tool — it lets you commit only part of your changes, deliberately, instead of saving everything blindly.

### `git init`

```bash
git init
```

Tells Git "start watching this folder." Creates a hidden `.git` folder holding the entire history, branches, and configuration for this repo.

Ran it → got the default branch name warning (`master` vs `main`), and confirmed with `git status` → "On branch master, No commits yet" with every real file listed as untracked (`.gitignore`, `.claude/`, `00_PLAN/`, etc. — correctly, `venv/` and `.env` did NOT appear, proving `.gitignore` is already working before the very first commit).

### `git add` — staging

```bash
git add -A
```

`-A` stages every change across the whole project — new, modified, and deleted files all at once. After this, `git status` shows the same files, but now under "Changes to be committed" instead of "Untracked files" — this is the file moving from the working directory into the staging area.

### `git commit` — saving the snapshot

```bash
git commit -m "Day 1 restart: fresh environment, git, and docker setup"
```

`-m` writes the commit message directly in the command. This created the **root commit** (`c5c94c9`) — the very first commit, with no parent — containing 280 files.

**Good commit message feedback:** the message was specific (said *what* happened and *what areas*), which is the main goal — letting someone understand the change without opening the diff. One professional convention worth knowing: many teams keep the first line under ~50 characters and write it in **imperative mood** (e.g. "Restart Day 1 setup" instead of "Day 1 restart"). Both styles are acceptable; imperative + short is simply the most common convention seen across real projects.

### `git log` — the history

```bash
git log
```

Shows every commit, newest first: the commit hash (unique ID), author, date, and message. With one commit so far, it showed just that one, including `(HEAD -> master)` — `HEAD` is a pointer meaning "where you currently are"; right now it points at branch `master`, which points at this commit. (Full depth on `HEAD` and detached `HEAD` comes later today.)

### `git diff` — reviewing changes before committing

```bash
echo "# test line for git diff" >> README.md
git diff
```

Shows a line-by-line comparison against the last commit — added lines marked with `+`, removed lines with `-`. This lets you review exactly what you're about to save, before staging or committing it.

After cleaning the test line back out (plain bash, not a Git command — Git's own undo tools come later), `git diff` produced no output at all, confirming the working directory exactly matched the last commit again.

## Branching, merging, and conflicts

**Branch:** a movable pointer to a commit. Creating one is nearly instant — it does not copy any files. `git branch <name>` creates it without moving onto it; `git switch <name>` moves `HEAD` onto it; `git switch -c <name>` does both at once.

**Important distinction:** uncommitted changes never belong to any branch. They live only in the working directory / staging area. What decides which branch a change ends up on is which branch `HEAD` was pointing at **at the moment `git commit` runs** — not when the edit was made.

**Why `master`, not `main`:** Git's own local default branch name has always been `master`; GitHub changed only its own platform-level default to `main` around 2020. A local `git init` still uses Git's default unless `init.defaultBranch` is configured. Renamed with `git branch -m master main` (works even while on a different branch, by naming both old and new explicitly).

### Fast-forward merge
Happens when the target branch (`main`) has had **no new commits** since the other branch diverged. Git just moves `main`'s pointer forward — no new merge commit, history stays linear.
```bash
git switch main
git merge practice-feature   # → "Fast-forward"
```

### Clean three-way merge
Happens when **both** branches have new commits since diverging, but they touch **different** things. Git finds the common ancestor, combines both sides automatically, and creates a **merge commit** — a commit with two parents.
```bash
git switch -c feature-colors   # branched from main, added "blue" to colors.md
git switch main                 # meanwhile, main added greeting.md (different file)
git merge feature-colors        # → "Merge made by the 'ort' strategy" — no conflict
```
`git log --oneline --graph` shows the shape: two lines splitting from a shared ancestor, joining back into one merge commit with two parents.

### Real merge conflict — caused and resolved
Happens when both branches change the **same line** differently. Git cannot guess which is correct, so it stops and inserts conflict markers directly into the file:
```
red
<<<<<<< HEAD
blue-light
=======
blue-dark
>>>>>>> feature-conflict
```
- `<<<<<<< HEAD` → start of the current branch's version
- `=======` → divider between the two versions
- `>>>>>>> <branch>` → end, naming the incoming branch

**Resolving it:** manually edit the file to the final intended content, delete all marker lines, then `git add` + `git commit` as normal — this becomes a merge commit, reached by hand instead of automatically. Verified with a second real conflict (`main`'s `blue-light` vs `feature-conflict`'s `blue-dark`), resolved by keeping both.

### `>` vs `>>` (shell redirection, not Git — came up naturally while practicing)
- `>` **overwrites** the file completely — deletes old content first.
- `>>` **appends** to the end — keeps old content, adds new text after it.
