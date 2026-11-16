# Environment, Git & GitHub — Worked Examples

**Day 1** · real commands run, real mistakes made and fixed

> Each example's lesson is given twice: **Simple version** (easy words) then **Technical version** (real terms, still easy to follow) — same as `1_theory.md`.

---

## Example 1 — `python` vs `python3`

```bash
$ python --version
bash: python: command not found
$ python3 --version
Python 3.13.5
```

**Simple version — why it broke:** this computer has no short word `python` at all. Only the full word `python3` works here.
**Technical version:** modern Linux systems deliberately remove the plain `python` alias, to avoid any confusion with the old, dead Python 2 (end-of-life January 2020). **Lesson: always type `python3` explicitly.**

## Example 2 — venv create → activate → destroy → rebuild

```bash
$ python3 -m venv venv
$ source venv/bin/activate
(venv) $ pip install requests
# installed requests + 4 dependencies: urllib3, idna, charset_normalizer, certifi
(venv) $ pip freeze > requirements.txt
(venv) $ deactivate
$ rm -rf venv                     # destroy completely
$ python3 -m venv venv            # rebuild from nothing
$ source venv/bin/activate
(venv) $ pip install -r requirements.txt
# all 5 packages reinstalled at the EXACT same pinned versions
```

**Real mistake made, simple version:** I typed a dot instead of a slash — `source venv/bin.activate` — and got told the file does not exist. I fixed it by typing the correct slash: `venv/bin/activate`.
**Technical version:** `bash: venv/bin.activate: No such file or directory` — an easy typo since `.` and `/` sit visually close together when typed quickly.

**Proof this matters, simple version:** the rebuilt box came back looking exactly identical to the original box. This proves the saved file alone is enough to recreate the whole project setup on any computer, from nothing.
**Technical version:** `requirements.txt` is a complete, accurate, reproducible environment specification — validated by the destroy-and-rebuild test matching exactly.

## Example 3 — the stuck multi-line quote (the wrong way, and why it broke)

**The wrong way:**
```bash
echo "venv/
__pycache__/
*.pyc
.env" > .gitignore
```

**Simple version of what happened:** this command's text is spread across many lines, but all of it is supposed to be inside one pair of quote marks. When I pasted it into the terminal, the closing quote mark got separated or lost. After that, the terminal kept showing `>` on every new line. **This `>` is not an error — it just means the terminal is still waiting for me to give it the missing closing quote.**

**Technical version:** bash was waiting for the string literal to be closed; without it, it stays in an open-quote continuation state (`>` prompt), rather than throwing an error.

**The fix, simple version:** press `Ctrl+C` to cancel the stuck command completely, then use one single line instead, so nothing can get split apart during a paste:
```bash
printf "venv/\n__pycache__/\n*.pyc\n.env\n" > .gitignore
```

**Lesson, technical version:** multi-line commands wrapped in quotes are risky to paste into a terminal. A single-line command using `\n` escape sequences (or a heredoc) is safer against paste corruption.

## Example 4 — proving `.gitignore` works *before* the first commit

```bash
$ git init
$ git status
On branch master
No commits yet
Untracked files:
        .claude/
        .gitignore
        00_PLAN/ ... (other real folders) ...
        requirements.txt
```

**Simple version:** notice that `venv/` and `.env` do NOT show up anywhere in this list, even though both really exist on the computer. This shows that `.gitignore` was already protecting them, even before a single save ever happened.
**Technical version:** this proves `.gitignore` enforcement predates the first commit — confirming the correct order (`.gitignore` must exist before anything secret is ever committed, never after).

## Example 5 — `python-dotenv` actually reading `.env`

```python
# test_env.py
from dotenv import load_dotenv
import os

load_dotenv()
print(os.getenv("FAKE_API_KEY"))
```

```bash
$ pip install python-dotenv
$ python3 test_env.py
abc123
```

**Simple version:** the printed value `abc123` is exactly the same value that is sitting inside the `.env` file (`FAKE_API_KEY=abc123`). This proves the code really did open that file and really did pull the value out of it — not guessed, not hardcoded.
**Technical version:** confirms `load_dotenv()` successfully parsed `.env` into the process's environment variables, and `os.getenv()` successfully retrieved the value from there at runtime. `test_env.py` was deleted afterward since it was only a proof script, not real project code.

## Example 6 — the full add → commit → log → diff cycle

```bash
$ git add -A
$ git status          # 280 files now under "Changes to be committed"
$ git commit -m "Day 1 restart: fresh environment, git, and docker setup"
[master (root-commit) c5c94c9] ... 280 files changed, 5040 insertions(+)
$ git log
commit c5c94c9... (HEAD -> master)
Author: MiraanEjaz <miraanejaz15@gmail.com>
Date:   ...
    Day 1 restart: fresh environment, git, and docker setup
```

**Simple version:** I moved all my changed files into the waiting room, then permanently saved everything in the waiting room as one entry in the history book. The history book now shows that one entry.
**Technical version:** staged all 280 files via `git add -A`, then created the root commit (`c5c94c9`, no parent) via `git commit`, visible afterward in `git log`.

Then, to see `git diff` actually show something:
```bash
$ echo "# test line for git diff" >> README.md
$ git diff
--- a/README.md
+++ b/README.md
@@ -148,3 +148,4 @@ ...
+# test line for git diff
```

**Simple version:** I added one new line to a file without saving it yet, and `git diff` showed me exactly that one new line, marked with a `+`, before I decided whether to save it.

Cleaned up afterward (plain bash, not a Git undo tool yet):
```bash
$ head -n -1 README.md > /tmp/readme_fix && mv /tmp/readme_fix README.md
$ git diff        # empty output — confirms working directory matches the last commit exactly
```

## Example 7 — fast-forward merge

```bash
$ git branch practice-feature
$ git switch practice-feature
$ git commit -m "Document Day 1: ..."   # main never moved
$ git switch main
$ git merge practice-feature
Updating c5c94c9..dfad6d1
Fast-forward
```

**Simple version:** `main` had not moved at all while the other branch was being worked on, so when I merged, Git just slid `main`'s bookmark forward to the same spot — no extra combining save was needed.
**Technical version:** `main`'s pointer fast-forwarded to `dfad6d1` — no merge commit, history stays linear.

## Example 8 — clean three-way merge (real mistake: renamed `master`→`main` mid-session)

```bash
$ git switch -c feature-colors         # from main (colors.md = "red")
$ printf "red\nblue\n" > colors.md
$ git commit -m "Add blue to colors list"
$ git switch main
$ printf "hello\n" > greeting.md       # different file — no overlap
$ git commit -m "Add greeting file"
$ git merge feature-colors
Merge made by the 'ort' strategy.
```

**Simple version:** both branches got new saves, but they changed two completely different files, so Git was able to combine both sides automatically, without needing me to decide anything by hand.
**Technical version:** `git log --oneline --graph` showed two lines splitting from `aafb4e9` and joining at `7e3961f` (two parents). `colors.md` has `blue`, `greeting.md` has `hello` — both kept, confirming a true auto-merged two-parent commit.

**Real mistake/decision made during this exact example's session, simple version:** partway through today's practice, I renamed the default branch from `master` to `main`, after asking why it wasn't already called `main`.
**Technical version:** `git branch -m master main`, run while standing on a different branch — confirming the rename command works by explicitly naming both the old and new branch, without requiring that branch to be currently checked out.

## Example 9 — 💥 a real conflict, caused on purpose, and resolved

```bash
$ git switch -c feature-conflict        # from main (colors.md = "red\nblue")
$ printf "red\nblue-dark\n" > colors.md
$ git commit -m "Change blue to blue-dark"
$ git switch main
$ printf "red\nblue-light\n" > colors.md   # SAME line 2, different value
$ git commit -m "Change blue to blue-light"
$ git merge feature-conflict
CONFLICT (content): Merge conflict in colors.md
```

**Simple version:** both branches changed the exact same line of the exact same file, but to two different values. Git could not guess which one I actually wanted, so it stopped and put both versions directly inside the file for me to decide.

File after the conflict:
```
red
<<<<<<< HEAD
blue-light
=======
blue-dark
>>>>>>> feature-conflict
```

**Simple version of resolving it:** I rewrote the file myself to whatever I actually wanted the final content to be, deleted every marker line, and saved it normally.
**Technical version:**
```bash
$ printf "red\nblue-light\nblue-dark\n" > colors.md
$ git add colors.md
$ git commit -m "Resolve merge conflict: keep both blue-light and blue-dark"
```
Produced a second merge commit (`b021bf3`), this time resolved by hand instead of automatically — the key difference from Example 8's auto-merge.
