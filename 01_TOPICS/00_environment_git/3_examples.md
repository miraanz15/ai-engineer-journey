# Environment, Git & GitHub — Worked Examples

**Day 1** · real commands run, real mistakes made and fixed

---

## Example 1 — `python` vs `python3`

```bash
$ python --version
bash: python: command not found
$ python3 --version
Python 3.13.5
```

**Why it broke:** no plain `python` command exists on this machine — only `python3`. Modern Linux systems often remove the plain `python` alias on purpose, to avoid any confusion with the old, dead Python 2. **Lesson: always type `python3` explicitly.**

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

**Real mistake made:** typed `source venv/bin.activate` (a dot instead of a slash) → `bash: venv/bin.activate: No such file or directory`. Fixed by correcting to `venv/bin/activate` (slash). Easy typo since `.` and `/` are visually close in a fast-typed command.

**Proof this matters:** the rebuilt environment matched the original exactly — `requirements.txt` is enough, alone, to recreate the whole environment on any machine.

## Example 3 — the stuck multi-line quote (the wrong way, and why it broke)

**The wrong way:**
```bash
echo "venv/
__pycache__/
*.pyc
.env" > .gitignore
```

This command spans multiple lines inside one pair of quotes. During copy-paste into the terminal, the closing `"` got separated/lost. Bash then kept showing `>` on every new line — **not an error message, but bash saying "I'm still waiting for the closing quote you haven't given me yet."**

**The fix:** `Ctrl+C` to cancel the stuck input, then use a single-line command instead, so nothing can get split during paste:
```bash
printf "venv/\n__pycache__/\n*.pyc\n.env\n" > .gitignore
```

**Lesson:** multi-line commands inside quotes are risky to paste into a terminal. A one-line command with `\n` (or a heredoc) is safer.

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

Notice: **`venv/` and `.env` do NOT appear in this list**, even though both exist on disk. This proves `.gitignore` was already protecting them, *before* a single commit was ever made — which is the correct order (`.gitignore` must exist before anything secret is ever committed, not after).

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

`abc123` matches the value inside `.env` (`FAKE_API_KEY=abc123`), proving `load_dotenv()` really did load the file and `os.getenv()` really did fetch it. Deleted `test_env.py` afterward since it was only a proof script, not real project code.

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

Then, to see `git diff` actually show something:
```bash
$ echo "# test line for git diff" >> README.md
$ git diff
--- a/README.md
+++ b/README.md
@@ -148,3 +148,4 @@ ...
+# test line for git diff
```

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
`main` just slid forward to match — no merge commit, linear history.

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
`git log --oneline --graph` showed two lines splitting from `aafb4e9` and joining at `7e3961f` (two parents). `colors.md` has `blue`, `greeting.md` has `hello` — both kept.

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

File after the conflict:
```
red
<<<<<<< HEAD
blue-light
=======
blue-dark
>>>>>>> feature-conflict
```

Resolved by rewriting the file to the intended final content and removing the markers, then committing normally:
```bash
$ printf "red\nblue-light\nblue-dark\n" > colors.md
$ git add colors.md
$ git commit -m "Resolve merge conflict: keep both blue-light and blue-dark"
```
Produced a second merge commit (`b021bf3`), this time resolved by hand instead of automatically.

**Real mistake/decision during this session:** renamed the default branch from `master` to `main` mid-way through today's practice, after asking why it wasn't `main` by default — `git branch -m master main`, done while standing on a different branch, confirming the rename doesn't require being on that branch.
