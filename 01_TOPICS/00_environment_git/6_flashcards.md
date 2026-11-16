# Environment, Git & GitHub — Flashcards

> Short Q -> A for daily spaced-repetition revision.

1. Why does `python` not work but `python3` does, on this machine? → Many Linux systems remove plain `python` to avoid confusion with dead Python 2; always use `python3` explicitly.
2. What problem does `venv` solve? → Different projects may need different versions of the same library; `venv` creates a sealed, per-project folder so they don't clash.
3. What does activating a `venv` actually change? → It rewrites `PATH` for the current terminal session so `python`/`pip` point inside `venv/`, not system-wide.
4. `pip list` vs `pip freeze`? → `list` = human-readable table. `freeze` = `name==version` format, the same format `pip install -r` can read back in.
5. What is the destroy-and-rebuild test, and why does it matter? → Delete `venv`, rebuild from `requirements.txt` alone, confirm `pip list` matches — proves the file is a complete, accurate environment spec.
6. Why must `.gitignore` exist BEFORE `.env` is ever committed? → `.gitignore` only affects future commits; a secret committed even once is permanently in history, requiring a destructive rewrite to remove.
7. What are Git's three stages? → Working directory → Staging area (index) → Repository (committed history).
8. `git add -A` vs `git commit`? → `add` stages changes (moves to index); `commit` permanently saves staged changes as a new snapshot.
9. What does `HEAD -> branch` mean in `git log`? → `HEAD` is attached to that branch name; new commits move the branch forward and `HEAD` follows.
10. Does creating a branch (`git branch x`) move `HEAD`? → No — only `git switch`/`git switch -c` moves `HEAD` onto a different branch.
11. Fast-forward merge — when does it happen? → When the target branch has had no new commits since the other branch diverged; Git just slides the pointer forward, no merge commit.
12. Clean three-way merge — when does it happen? → Both branches have new commits since diverging, but touch different things; Git auto-combines and creates a merge commit (two parents).
13. What causes a real merge conflict? → Both branches change the exact same line(s) differently; Git can't auto-pick, so it inserts conflict markers.
14. What do `<<<<<<< HEAD`, `=======`, `>>>>>>> branch` mean? → Your current branch's version / divider / the incoming branch's version. Delete all markers after resolving.
15. `git branch -d` vs `-D`? → `-d` refuses to delete an unmerged branch (safety check); `-D` force-deletes regardless.
16. Why `master` locally but `main` on GitHub? → Git's own local default is still `master`; GitHub changed only its platform-level default to `main` around 2020.
17. `git remote add` vs `git push -u`? → `remote add` just registers the URL locally under an alias (e.g. `origin`); `push -u` actually uploads commits AND sets up branch tracking.
18. What is `origin/main`? → A LOCAL remote-tracking branch — your computer's cached memory of what GitHub last showed, updated only on `fetch`/`pull`.
19. `git fetch` vs `git pull`? → `fetch` downloads and updates `origin/main` only (your files untouched); `pull` = fetch + merge, done automatically.
20. `git restore <file>` vs `git restore --staged <file>`? → `restore` discards an unstaged edit entirely; `--staged` only unstages it — the edit itself survives.
21. What does `git reset --soft HEAD~1` do? → Moves the branch pointer back one commit, but keeps that commit's changes staged, ready to re-commit. Nothing is deleted.
22. `reset` vs `revert` — the key interview answer? → `reset` rewrites history (only safe if NOT pushed/shared); `revert` never rewrites history — it adds a new commit that cancels the old one's effect, keeping it visible.
23. Does `git restore`/`reset`/`revert` work on individual files or whole commits? → `restore` is per-file (your choice); `reset`/`revert` are per-commit, all-or-nothing.
24. `git stash apply` vs `git stash pop`? → `apply` restores changes but keeps a copy in the stash list; `pop` restores AND removes it (apply+drop combined).
25. What is detached `HEAD`? → `HEAD` points directly at one specific commit, skipping the branch-name layer entirely (checked out by hash, not branch name).
26. Why is detached `HEAD` risky? → A new commit made while detached has no branch anchoring it; switching away without rescuing it can make it unreachable and eventually garbage-collected.
27. How do you rescue a commit made in detached `HEAD`? → `git switch -c <new-branch>` right there, before switching away — anchors the commit with a real branch reference.
28. What does `git blame <file>` show? → Each line annotated with the commit hash, author, and date of its last change.
29. Lightweight vs annotated tag? → Lightweight = just a name on a commit. Annotated = a full object with tagger, date, and message (shown by `git show`).
30. Are tags pushed automatically with `git push`? → No — need `git push origin --tags` explicitly.
