# Environment, Git & GitHub — Summary Sheet

**The 10 things to remember:**

1. Always type `python3` explicitly — plain `python` often doesn't exist on modern Linux.
2. `venv` isolates Python packages per project; `pip freeze > requirements.txt` must survive a destroy-and-rebuild test to actually be trusted.
3. `.gitignore` must exist BEFORE anything secret (`.env`) is ever committed — it only protects the future, never the past.
4. Three stages: working directory → staging (`git add`) → repository (`git commit`).
5. Fast-forward merge = target never moved. Clean three-way = both moved, different things, auto-combined. Conflict = both moved, SAME line, needs manual resolution (`<<<<<<<`/`=======`/`>>>>>>>`).
6. `origin/main` is a LOCAL cached memory of GitHub — `fetch` updates it alone; `pull` = fetch + merge in one step.
7. `restore` (per-file, discard) vs `restore --staged` (per-file, unstage only) vs `reset --soft` (per-commit, undo but keep staged) vs `revert` (per-commit, SAFE — adds a new commit, never rewrites history).
8. `reset` = only for local, unpushed commits. `revert` = always for anything already pushed/shared. This is the #1 interview question on undo.
9. Detached `HEAD` = pointing directly at a commit, skipping the branch name. A commit made there is lost forever unless rescued with `git switch -c <name>` before switching away.
10. Tags never move (unlike branches) and are NOT pushed automatically — need `git push origin --tags`.
