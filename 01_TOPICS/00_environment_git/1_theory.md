# Environment, Git & GitHub — Theory & Explanation

**Day 1** · Level 1

> Every concept below has two parts: **Simple version** (easy words, for fast understanding) and **Technical version** (the real professional words I need for interviews and work chat — but still written so it is easy to follow). Every command that was actually run is shown with its real output.

---

## 1. Checking the environment

**Simple version:** Before writing any code, I checked what tools are already on my computer. This is like checking what is already in the kitchen before cooking, so I do not buy something I already have.

**Technical version:** Before starting any project, a developer checks the **environment** — all the tools, language versions, and programs already installed on the machine — to confirm they meet the project's minimum requirements.

- **Python** — the programming language used for almost everything in this course.
  ```bash
  python3 --version
  ```
  Output: `Python 3.13.5` — above the 3.11+ minimum needed.

- **Gotcha:** plain `python` does not work on this machine.
  ```bash
  python --version
  ```
  Output: `command not found`.
  **Simple version of why:** Old computers had two versions of Python, called Python 2 and Python 3. Python 2 is now dead — nobody should use it anymore. So this computer removed the short word `python` completely, to stop anyone using the old dead version by accident.
  **Technical version:** Python 2 reached end-of-life in January 2020. Modern Linux systems remove the plain `python` alias entirely to prevent accidental use of the deprecated Python 2 interpreter. Always type `python3` explicitly.

- **VS Code** — the code editor, where I write and read code files. Already installed.
- **Terminal** — a text window where I type commands instead of clicking buttons with a mouse. Already in use.

## 2. `venv` — virtual environments

**Simple version:** Sometimes two different projects on the same computer need two different versions of the same tool. A computer can usually only keep one version of a tool installed at a time. So if I install the version project A needs, project B breaks — and the other way around. `venv` fixes this. `venv` makes a separate, closed box, just for one project. Anything I install inside that box stays inside that box. It does not touch anything outside, and it does not touch any other project.

**Technical version:** A **virtual environment** is an isolated, self-contained folder holding its own Python interpreter copy and its own installed packages, separate from the system-wide Python installation and from any other project's virtual environment. This solves **dependency conflicts** — a situation where two projects require incompatible versions of the same library.

```
      python3 -m venv venv        → creates the sealed box (a folder named venv/)
      source venv/bin/activate    → step INSIDE the box (prompt shows "(venv)")
      deactivate                  → step back out
```

**Simple version of "activating":** Turning the box on does not install anything by itself. It just tells the terminal: "from now on, when I type `python` or `pip`, use the tools inside this box, not the ones installed everywhere else on the computer."

**Technical version:** Activation modifies the `PATH` environment variable (the ordered list of folders the shell searches, in order, to find a program matching a typed command) for the current terminal session only, so `python` and `pip` resolve to the binaries inside `venv/bin/` instead of the system-wide ones. It does not change anything outside that one terminal session.

## 3. `pip` — the package manager

**Simple version:** `pip` is a tool that downloads other people's code, called "packages" or "libraries," and puts them inside my project's box (`venv`), so I can use that code instead of writing everything myself from zero.

**Technical version:** `pip` is Python's standard **package manager** — it downloads packages from PyPI (the Python Package Index) and installs them, along with any **dependencies** (other packages that package needs in order to work), into the currently active virtual environment.

| Command | Simple version | Technical version |
|---|---|---|
| `pip install <name>` | Downloads one tool and anything it needs | Installs a package and resolves/installs its transitive dependencies |
| `pip list` | Shows everything installed, as an easy-to-read table | Lists installed packages in human-readable table format |
| `pip freeze` | Shows the same list, but in a format that can be saved and reused later | Outputs installed packages as exact `name==version` pins, the format `pip install -r` can re-read |
| `pip freeze > requirements.txt` | Saves that list into a file, so I can rebuild the exact same box again later | Writes the frozen dependency list to a lockfile-style manifest, enabling reproducible environment rebuilds |
| `pip install -r requirements.txt` | Reads that saved file and installs everything listed in it | Installs every package pinned in the manifest, at the exact listed version |

**Real run and output, today:**
```bash
pip install requests
```
Output (abbreviated): `Successfully installed requests-2.32.5 urllib3-2.5.0 idna-3.11 charset_normalizer-3.4.4 certifi-2025.11.12`

**Simple version of "dependency":** `requests` needed 4 other small tools to actually work, so `pip` quietly installed those 4 too, without me asking for them one by one.
**Technical version:** installing `requests` pulled in its declared dependencies — `urllib3`, `idna`, `charset_normalizer`, `certifi` — because `requests` itself requires them internally to function.

```bash
pip list
```
Output:
```
Package            Version
------------------ -------
certifi            2025.11.12
charset-normalizer 3.4.4
idna                3.11
pip                 25.x
requests            2.32.5
urllib3             2.5.0
```

```bash
pip freeze > requirements.txt
cat requirements.txt
```
Output:
```
certifi==2025.11.12
charset-normalizer==3.4.4
idna==3.11
requests==2.32.5
urllib3==2.5.0
```

**Gotcha noticed:** `pip list` shows `pip` itself as an installed package, but `pip freeze` does not. `pip` and a couple of other base build tools are deliberately excluded from `freeze`'s output, because every fresh `venv` already comes with them automatically — freezing them would be pointless noise.

### Proof: the destroy-and-rebuild test

**Simple version:** Just believing the saved file is correct is not good enough. The only real proof is to destroy the whole box, build a brand new empty one, rebuild only from the saved file, and check if it comes back exactly the same.

**Technical version:** This test validates that `requirements.txt` is a complete and accurate specification of the environment — the same guarantee a teammate's machine or a production server relies on when rebuilding from this file alone, with zero other hidden manual setup steps.

```bash
deactivate
rm -rf venv
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pip list
```
Output: all 5 packages came back, at the exact same pinned versions as before deletion.

**Result:** confirmed match. This proves `requirements.txt` alone is enough to rebuild the identical environment from nothing.

## 4. `.env` and `.gitignore` — and why the ORDER matters

**Simple version:** `.env` is a file where I keep secret values, like passwords or API keys, written as `NAME=value`. The code reads the secret from this file instead of having the secret typed directly inside the code. `.gitignore` is a different file that tells Git: "never save these files into the project's saved history." I put `venv/`, `__pycache__/`, `*.pyc`, and `.env` inside `.gitignore`.

**Technical version:** `.env` is a plain-text file holding environment variables as `KEY=value` pairs, read at runtime (typically via `python-dotenv`) so secrets never appear hardcoded in source code. `.gitignore` is a Git configuration file listing path patterns that Git should never track or stage, even with `git add -A`.

**The dangerous rule, simple version:** `.gitignore` can only protect files from being saved from now on. It has no power over the past. If a secret file gets saved into history even one single time, that secret stays in the history forever, even if I delete the file afterward. The only way to truly remove it needs an advanced and dangerous technique that rewrites the whole history. And if that secret was ever uploaded to GitHub, I should assume someone else may have already seen it, so the real fix is to make the old secret invalid and create a new one.

**Technical version:** `.gitignore` only affects *future* commits, not already-committed history. Removing an already-committed secret requires a destructive history rewrite (`git filter-branch` or the `BFG Repo-Cleaner` tool), and even after that, if it was ever pushed, the secret must be treated as compromised and rotated (invalidated and replaced) regardless — rewriting history does not undo prior exposure.

**Real example tied to ScreenerAI:** this exact mistake happened on the real ScreenerAI project — its `.env` got committed before `.gitignore` existed to protect it. On this Learning repo, `.gitignore` was deliberately created **first**, before any `.env` file ever existed, specifically to avoid repeating that mistake.

**Proof run today:**
```bash
printf "venv/\n__pycache__/\n*.pyc\n.env\n" > .gitignore
printf "FAKE_API_KEY=abc123\n" > .env
git init
git status
```
Output: `venv/` and `.env` did **not** appear anywhere in the untracked files list — confirming `.gitignore` was already protecting them before the very first commit even happened.

### Proof `python-dotenv` really reads `.env`

**Simple version:** Just having a `.env` file is not proof the code actually reads it. I wrote a tiny test script to check for real.

**Technical version:** `python-dotenv` is a third-party library that loads `KEY=value` pairs from a `.env` file into the process's environment variables at runtime, so `os.getenv()` can read them like any other environment variable.

```bash
pip install python-dotenv
```
`test_env.py`:
```python
from dotenv import load_dotenv
import os

load_dotenv()
print(os.getenv("FAKE_API_KEY"))
```
```bash
python3 test_env.py
```
Output: `abc123` — matching the exact value inside `.env`. This proves `load_dotenv()` really did load the file, and `os.getenv()` really did fetch the value out of it, not just assumed/guessed.

## 5. Git — why version control exists

**Simple version:** Without a tool like Git, the only way to keep old versions of a project is to manually copy whole folders, with names like "project_v1," "project_final," "project_final_FINAL." This is messy, it wastes space, and it never shows exactly what changed between one copy and the next.

**Technical version:** **Git** is a **distributed version control system (DVCS)** — a tool that records snapshots of a codebase over time, called **commits**, letting you see exactly what changed and when, restore any previous snapshot, and let multiple people work on the same codebase without silently overwriting each other's work. "Distributed" means every developer's machine holds the **entire** project history locally, not just a pointer to the newest version on a central server.

### The three rooms (three-stage model)

**Simple version:** Git has three separate "rooms" a change passes through before it is permanently saved.

1. **Working directory** — Simple: the normal folder where I actually edit files. Technical: the literal files on disk, exactly as they currently sit.
2. **Staging area** (the "index") — Simple: a waiting room. I choose which changed files go into the next save by placing them here first. Technical: an intermediate index file tracking exactly which changes are queued for the next commit, letting a commit include only part of the current changes, deliberately.
3. **Repository** — Simple: the permanent history book. Technical: the `.git` object database holding every commit ever made, permanently, once `git commit` runs.

**Why staging matters:** this extra step is what makes Git different from a plain backup tool — it lets me commit only part of my changes on purpose, instead of blindly saving everything that happens to be sitting in the folder.

### `git init`

**Simple version:** This tells Git "start watching this folder from now on." It creates a hidden folder that holds the whole history.

**Technical version:** `git init` initializes a new Git repository by creating a hidden `.git` directory in the current folder, containing the object database, references (branches/tags), and configuration for this repo.

```bash
git init
```
Output: a default-branch-name warning (`master` vs `main` — explained below), then confirmed with:
```bash
git status
```
Output: `On branch master, No commits yet`, with every real file listed as untracked (`.gitignore`, `.claude/`, `00_PLAN/`, etc.) — correctly, `venv/` and `.env` did **not** appear, proving `.gitignore` was already working before the very first commit.

### `git add` — staging

**Simple version:** This moves a changed file from the normal folder into the waiting room, saying "yes, I want this included in the next save."

**Technical version:** `git add` stages a file — copies its current content into the index, marking it for inclusion in the next commit.

```bash
git add -A
```
`-A` stages **every** change across the whole project at once — new files, modified files, and deleted files all together.
Output of `git status` afterward: the same files now appear under "Changes to be committed" instead of "Untracked files" — this is the visible proof the file moved from the working directory into the staging area.

### `git commit` — saving the snapshot

**Simple version:** This permanently saves whatever is currently in the waiting room, as one new entry in the history book. Each entry has a short message explaining what it is.

**Technical version:** `git commit` creates a new commit object from the current contents of the index, tagged with author, timestamp, and the written message, plus a pointer to its parent commit (the commit it was built on top of).

```bash
git commit -m "Day 1 restart: fresh environment, git, and docker setup"
```
`-m` writes the commit message directly inside the command, instead of opening a text editor.
This created the **root commit** (`c5c94c9`) — the very first commit in this repo's history, with no parent commit at all — containing 280 files.

**Good commit message feedback:** the message was specific — it said *what* happened and *what areas* were touched — which lets anyone understand the change without opening the full diff. One professional convention worth knowing: many teams keep the first line under ~50 characters, written in **imperative mood** (e.g. "Restart Day 1 setup," like giving an order, instead of "Day 1 restart," describing a fact). Both styles are acceptable; imperative + short is simply the most common convention seen in real projects.

### `git log` — the history

**Simple version:** Shows the full list of every save ever made, newest first.

**Technical version:** `git log` prints the commit history: hash (unique ID), author, date, and message for every commit, newest first.

```bash
git log
```
Output, with one commit so far, showed just that one commit, including `(HEAD -> master)`.
**Simple version of `HEAD`:** a marker answering "where am I right now?" **Technical version:** `HEAD` is a reference pointer indicating the currently checked-out commit; right now it points at branch `master`, which itself points at this commit. (Full depth on detached `HEAD` is covered later in this same file.)

### `git diff` — reviewing changes before committing

**Simple version:** Shows exactly which lines changed, compared to the last save, before I decide to save again. Added lines are marked one way, removed lines another way.

**Technical version:** `git diff` (with no arguments) compares the working directory against the last commit, line by line, printing added lines prefixed with `+` and removed lines prefixed with `-`.

```bash
echo "# test line for git diff" >> README.md
git diff
```
Output: showed the one new line, prefixed with `+`.

After cleaning the test line back out (using plain bash, not a Git command — Git's own proper undo tools are covered later in this file):
```bash
git diff
```
Output: nothing at all — confirming the working directory exactly matched the last commit again.

## 6. Branching, merging, and conflicts

**Simple version of a branch:** A branch is like a bookmark that points at one specific save. Making a new bookmark is instant and free — it does not copy any files at all.

**Technical version:** A **branch** is a lightweight, movable pointer to a commit. Creating one (`git branch <name>`) does not move onto it or copy anything — it just creates the pointer. `git switch <name>` moves `HEAD` onto an existing branch. `git switch -c <name>` does both — creates the branch and moves onto it — in one step.

**Important distinction, simple version:** An uncommitted change does not belong to any branch at all. It only exists in the normal folder and the waiting room. What decides which branch a change ends up on is which branch I was standing on **at the exact moment I run `git commit`** — not when I made the edit.

**Technical version:** uncommitted changes live in the working directory/index regardless of the currently checked-out branch. The branch association is determined entirely by which branch `HEAD` points to at commit time, not edit time.

**Why `master`, not `main` — simple version:** Git itself has always used the name `master` as its own default. GitHub, the website, later decided to use `main` as its own separate default, around the year 2020. These are two different companies' defaults, not the same thing.
**Technical version:** a local `git init` still uses Git's own default branch name (`master`) unless the `init.defaultBranch` config value is set otherwise. GitHub only changed its *platform-level* default for newly created repos on its website.

```bash
git branch -m master main
```
Renames the current branch, working even while on a different branch, by explicitly naming both the old and new name.

### Fast-forward merge

**Simple version:** This happens when the branch I am merging INTO has had no new saves of its own since the other branch split off. Git can just slide its bookmark forward to the new spot — nothing needs to be combined.

**Technical version:** A fast-forward merge occurs when the target branch (`main`) has had **no new commits** since the other branch diverged. Git simply moves `main`'s pointer forward to match; no new merge commit is created, and history stays linear.

```bash
git switch main
git merge practice-feature
```
Output: `Fast-forward`.

### Clean three-way merge

**Simple version:** This happens when BOTH branches got new saves since they split, but those new saves touched different things. Git is smart enough to combine both sides by itself, and it creates one new special save that has two parents instead of one.

**Technical version:** Occurs when both branches have diverging commits that touch different content. Git locates the common ancestor commit, auto-combines both sides, and creates a **merge commit** — a commit with two parent commits instead of the usual one.

```bash
git switch -c feature-colors   # branched from main, added "blue" to colors.md
git switch main                 # meanwhile, main added greeting.md (different file)
git merge feature-colors        # → "Merge made by the 'ort' strategy" — no conflict
```
`git log --oneline --graph` shows the shape: two lines splitting from a shared ancestor, joining back into one merge commit with two parents.

### Real merge conflict — caused and resolved

**Simple version:** This happens when both branches change the exact same line, in two different ways. Git cannot guess which version is correct, so it stops and puts both versions directly inside the file, with special marker lines, and waits for me to decide by hand.

**Technical version:** A merge conflict occurs when both branches modify the same line(s) of the same file differently. Git inserts conflict markers directly into the file content, pausing the merge until the developer resolves it manually.

```
red
<<<<<<< HEAD
blue-light
=======
blue-dark
>>>>>>> feature-conflict
```
- `<<<<<<< HEAD` → start of the current branch's version (simple: "my side, before the line"). (technical: marks the beginning of the local/checked-out branch's content)
- `=======` → divider between the two versions (simple: the line between my side and their side). (technical: separates the two conflicting versions)
- `>>>>>>> <branch>` → end, naming the incoming branch (simple: end of their side, shows which branch it came from). (technical: marks the end of the incoming branch's content, labeled with its name)

**Resolving it, simple version:** I edit the file myself to whatever the final correct content should actually be, then delete every one of the marker lines completely, then save it normally with `add` + `commit`.
**Technical version:** manually edit the file to the intended final content, remove all conflict marker lines, then `git add` + `git commit` as normal — this produces a merge commit, reached by hand instead of automatically.

Verified with a second real conflict (`main`'s `blue-light` vs `feature-conflict`'s `blue-dark`), resolved by deliberately keeping both pieces of text.

### `>` vs `>>` (shell redirection, not Git — came up naturally while practicing)

**Simple version:** `>` wipes the file clean first, then writes the new text — the old content is gone. `>>` keeps the old content exactly as it is, and just adds the new text onto the end.
**Technical version:** `>` is shell output redirection with truncation (overwrite). `>>` is shell output redirection with append (no truncation).

## 7. GitHub — connecting the local repo to a remote

**Simple version:** Until I connect my project to GitHub, it only exists on this one computer, with no backup anywhere else. A "remote" is just a copy of the same project, stored on a server somewhere else (GitHub's computers), that I can push to and pull from.

**Technical version:** A **remote repository** is a copy of the repo hosted on a server. Connecting a local repo to one requires two steps:

1. **`git remote add <name> <url>`** — Simple: tells my computer "GitHub's copy lives at this address, and I'll call it `origin` for short." Technical: registers the remote's URL under a local alias (conventionally `origin`). Does not upload anything by itself.
2. **`git push -u origin main`** — Simple: actually uploads my saved history to that address, and also remembers the connection so I don't have to repeat the address every time. Technical: uploads local commits to the remote. `-u` (`--set-upstream`) creates a **tracking relationship** between local `main` and `origin/main`, so a future plain `git push`/`git pull` already knows where to go.

```bash
git remote add origin https://github.com/miraanz15/ai-engineer-journey.git
git remote -v                 # confirms origin registered for fetch + push
git push -u origin main       # [new branch] main -> main; branch tracks origin/main
```
Verified live afterward by requesting the repo URL directly — returned HTTP 200 (meaning the page genuinely exists and loaded, confirming the push really reached GitHub).

## 8. `fetch` vs `pull` — three separate places, not two

**Simple version:** I used to think there were only two copies of my project — the one on my computer, and the one on GitHub. Actually there are THREE. My computer secretly keeps a second, separate copy that only remembers what GitHub last looked like, and that second copy does not update by itself.

**Technical version:** Git tracks three distinct references, not two:
1. **Local branch (`main`)** — the real, current working files on this machine.
2. **Remote-tracking branch (`origin/main`)** — a *local* cached snapshot of what GitHub last showed, stored inside this machine's own `.git` folder (`.git/refs/remotes/origin/main`). Does **not** update automatically — only `fetch` or `pull` updates it.
3. **The real GitHub `main`** — the only one of the three that is NOT stored on this computer at all.

- **`git fetch`** — Simple: go check GitHub and bring back the news, but don't change my actual files yet. Technical: downloads new commits and updates `origin/main` only; the local `main` branch and working files are untouched.
- **`git pull`** — Simple: go check GitHub AND immediately apply whatever changed, in one single step. Technical: `git pull` = `git fetch` + `git merge origin/main`, performed automatically together.

### Proven live, with a real second clone (simulating a second collaborator)
```bash
git clone https://github.com/miraanz15/ai-engineer-journey.git clone-test
cd clone-test
printf "Test line...\n" >> README.md
git add -A && git commit -m "..." && git push      # simulates another collaborator
```
Back in the original folder:
```bash
git log --oneline -1              # main:         d9c52f6 (old)
git log --oneline origin/main -1  # origin/main:  d9c52f6 (old — hasn't checked yet)

git fetch                         # d9c52f6..abe2a52  main -> origin/main
git log --oneline -1              # main:         still d9c52f6 — UNCHANGED
git log --oneline origin/main -1  # origin/main:  abe2a52 — updated!
```
This is the direct proof: `fetch` alone moved `origin/main` but left `main` completely untouched.
```bash
git merge origin/main             # Fast-forward — brings the change into main for real
cat README.md                     # test line now present locally
```
Confirms `fetch` + `merge` together equal exactly what `pull` would have done in one single step.

## 9. Undo tools

### `git restore <file>` — discard an unstaged edit

**Simple version:** Throws away a change I made but never saved anywhere, putting the file back to exactly how it looked at the last save.
**Technical version:** discards an uncommitted, unstaged change, restoring the working-directory file to match the last commit. This is the modern replacement for the older `git checkout -- <file>` syntax.

```bash
printf "TEMPORARY TEST EDIT\n" >> README.md
git restore README.md   # edit gone, file back to last commit
```

### `git restore --staged <file>` — unstage, don't undo

**Simple version:** Moves a file out of the waiting room, back to "changed but not yet placed in the waiting room." The actual edit is NOT deleted — only its place in the waiting room is undone.
**Technical version:** moves a file out of the index, back to "modified, not staged." The edit itself is **not** discarded — only unstaged.

```bash
git add 00_PLAN/SKILLS_CHECKLIST.md
git restore --staged 00_PLAN/SKILLS_CHECKLIST.md   # back to "not staged", edit still there
```

### `git reset --soft <commit>` — undo a commit, keep the work staged

**Simple version:** Moves the bookmark back to an earlier save, but keeps the undone save's changes sitting ready in the waiting room, so I can save them again right away if I want. Nothing is actually deleted — only the "this was saved" marker is undone.
**Technical version:** moves the branch pointer back to an earlier commit while leaving the undone commit's changes staged in the index, ready to re-commit. (`--mixed`, the default mode, also unstages the changes; `--hard` discards them completely — not used today, handled with care since it is destructive.)

```bash
git commit -m "Temporary commit for reset --soft demo"
git reset --soft HEAD~1   # commit gone from log; file reappears as staged
```
`HEAD~1` means "one commit before `HEAD`." `HEAD~2` means two commits before, and so on.

### `git revert <commit>` — the SAFE undo for anything already pushed/shared

**Simple version — the most important interview answer in this whole topic:** `reset` changes the history book itself, as if a save never happened. This is only safe if nobody else has ever seen that save. `revert` never touches old history. Instead, it adds a brand new save that cancels out an old save's effect. The old mistake is still visible in the book, but its effect is gone. Use `revert`, never `reset`, once something has been pushed to GitHub and anyone else might have already seen it.

**Technical version:** `reset` rewrites history by moving the branch pointer backward — safe only for local, unpushed commits. `revert` never rewrites history; it computes the inverse of a target commit's diff and applies that inverse as a brand new commit, keeping the original mistake fully visible in the log.

```bash
git commit -m "Add mistake.md by accident"
git push
git revert HEAD --no-edit   # new commit undoes it; mistake.md commit still visible in history
```
Result: `mistake.md` disappeared from disk, but BOTH the original commit and the new revert commit stay visible in `git log` forever — nothing erased, fully honest history.

**How `revert` actually works, precisely:** it diffs the ONE targeted commit against its own direct parent commit, then applies the exact opposite of that one diff as a new commit — regardless of anything else that happened afterward. It can target any commit in the whole history, not only the most recent one. If other commits changed other files after the one being reverted, those other files stay completely untouched — the resulting snapshot only becomes identical to an older commit in the special case of reverting the single most recent commit with nothing else committed in between.

**File-level vs commit-level scope — an important distinction:**
- `git restore` (and `git restore --staged`) works **per file** — I can choose exactly which file(s) to act on, e.g. `git restore file1.md file2.md`, or `.` for every file.
- `git reset` and `git revert` work **per commit, all-or-nothing** — a commit is one bundled snapshot, so these tools move or reverse the entire bundle together. There is no built-in way to "undo this commit, but only for one of the files inside it."

### The full stash toolkit

**Simple version:** A stash is a temporary, safe drawer for changes I have made but not saved yet. It lets me clean up my working folder completely, without throwing the changes away and without saving half-finished work permanently. I can have more than one stash at once, stacked like a pile — the newest one is called `stash@{0}`, the next one `stash@{1}`, and so on.

**Technical version:** a **stash** temporarily shelves both staged and unstaged uncommitted changes outside the commit history, restoring a clean working directory. Multiple stashes form a stack, indexed `stash@{0}` (most recent) upward.

| Command | Simple version | Technical version |
|---|---|---|
| `git stash push -m "label"` | Puts my current unsaved changes into a new drawer, labeled, and cleans my folder | Creates a new stash entry from current staged+unstaged changes, with a message, resetting the working directory |
| `git stash list` | Shows every drawer I currently have | Lists all stash entries |
| `git stash show -p stash@{0}` | Looks inside one drawer without taking anything out | Shows the diff contents of a specific stash entry, without applying it |
| `git stash apply [stash@{n}]` | Brings the changes back out of the drawer, but keeps a copy in the drawer too | Reapplies a stash's changes to the working directory; the stash entry remains in the list |
| `git stash pop [stash@{n}]` | Brings the changes back out of the drawer, and throws the drawer away afterward | Reapplies a stash's changes AND removes that stash entry — equivalent to `apply` + `drop` combined |
| `git stash drop [stash@{n}]` | Throws away a drawer completely, without taking its contents back out first | Permanently deletes a stash entry without applying its changes |

Proven live, two separate runs: first `push` → `list`/`show` → `pop` (changes returned, list emptied automatically). Then separately: `push` → `apply` (changes returned, stash **still** present in the list) → `drop` (removed manually, afterward). This directly confirmed the `apply`-vs-`pop` difference: `apply` leaves a copy behind, `pop` removes it automatically.

## 10. `HEAD`, detached `HEAD`, `git blame`, and tags

**Simple version of `HEAD`:** `HEAD` answers one question: "where am I standing right now?" Normally it points at a branch name, and that branch name points at a commit — so it's indirect, like a signpost pointing at another signpost. Making a new branch (`git branch`) does NOT move where I'm standing. Only `git switch` actually moves me onto a different branch.

**Technical version:** `HEAD` is a reference pointer indicating the currently checked-out commit. In the normal case it points indirectly: `HEAD → branch name → commit`. `git branch <name>` creates a new pointer without moving `HEAD`; only `git switch` reattaches `HEAD` to a different branch reference. Because `HEAD` is attached to the branch name rather than a fixed commit, a new commit moves the branch pointer forward, and `HEAD` automatically follows it.

### Detached `HEAD`

**Simple version:** This happens when I check out one specific old save directly, by its ID number, instead of by a branch name. Now `HEAD` points straight at that one save, with no branch name in between at all.

**Technical version:** occurs when checking out a commit **by hash** rather than by branch name: `HEAD` points directly at that one commit, skipping the branch-name layer entirely (`HEAD → Commit`, no branch in between).

**The risk, simple version:** Any brand new save I make while in this state has no branch name holding onto it. If I move to a different branch without rescuing it first, that save can become unreachable, and Git's own cleanup process can delete it permanently later.

**Technical version:** any new commit made while detached has no branch reference anchoring it. Switching away without rescuing it makes the commit unreachable from any branch, and Git's garbage collector can eventually delete unreachable commits permanently.

**The fix, simple version:** create a brand-new branch right there, before moving away — this gives that save a proper name to hold onto, making it safe.
**Technical version:** `git switch -c <name>` right there, before switching away — anchors the commit with a real branch reference.

```bash
git checkout 879596e              # → "Note: switching to '879596e'... You are in 'detached HEAD' state..."
printf "experimental idea\n" > experiment.md
git commit -m "Experimental commit while detached"   # [detached HEAD ...] in the output; log shows just (HEAD), no branch
git switch -c rescue-branch        # commit now shows (HEAD -> rescue-branch) — rescued
git switch main
git branch -d rescue-branch        # REFUSED: "not fully merged" — the safety check working
git branch -D rescue-branch        # force-delete, since this was deliberate and the content wasn't needed
```
**Simple version of `-d` vs `-D`:** `-d` is the careful version — it refuses to delete a branch if that would lose saves that exist nowhere else. `-D` is the forceful version — it deletes anyway, no matter what, even if saves would be lost.
**Technical version:** `-d` (safe delete) refuses to delete a branch whose commits are not reachable from anywhere else. `-D` (force delete) skips that safety check entirely — genuinely destructive if those commits exist nowhere else.

After the force-delete, `experiment.md` was also gone from disk, since `main` never had that file in the first place.

### `git blame <file>`

**Simple version:** Shows, for every single line in a file, who last changed that exact line, and when.
**Technical version:** annotates every line of a file with the commit hash, author, and date of its most recent change — used to trace responsibility for a line before modifying unfamiliar code.

### Tags

**Simple version:** A tag is a fixed label stuck onto one specific save. Unlike a branch, a tag never moves to a different save later. Tags are normally used to mark official releases, like "version 1.0."

**Technical version:** a **tag** is an immutable reference to one specific commit, used to mark release points.

- **Lightweight tag** — Simple: just a name, nothing more. Technical: a simple ref pointing directly at a commit, with no extra metadata.
  ```bash
  git tag v0.1-day1
  ```
- **Annotated tag** — Simple: a name PLUS extra details — who made it, when, and why. Technical: a full Git object storing tagger name, date, and message, displayed by `git show` *before* the underlying commit's own info. Recommended for real releases.
  ```bash
  git tag -a v0.1-day1-annotated -m "End of Day 1 setup"
  git show v0.1-day1-annotated
  ```
  Output showed a `Tagger:`/`Date:`/message block that the lightweight tag does not have at all.
- **Simple version of pushing tags:** tags do NOT travel to GitHub automatically with a normal push. I have to explicitly say "also send the tags."
  **Technical version:** tags are **not** pushed automatically with a normal `git push`; they require `git push origin --tags` explicitly.
  ```bash
  git push origin --tags
  ```
