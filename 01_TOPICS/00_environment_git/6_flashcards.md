# Environment, Git & GitHub — Flashcards

> Short Q → A for daily spaced-repetition revision. Cover the answer, say it out loud, score Easy / OK / Forgot.

| # | Q | A |
|---|---|---|
| 1 | Why does a venv exist? | So each project's libraries stay isolated from every other project's |
| 2 | How do you know a venv is active? | Terminal prompt shows `(venv)` at the start |
| 3 | Why does `pip install requests` install more than one package? | `requests` has dependencies (`urllib3`, `idna`, etc.) that `pip` installs automatically |
| 4 | What's the only thing truly worth saving — `venv` or `requirements.txt`? | `requirements.txt` — proven by deleting `venv` entirely and rebuilding it identically from this one file |
| 5 | Does adding `.gitignore` remove an already-committed secret? | No — only `git rm --cached` stops tracking it; the old commit still has it forever |
| 6 | What are Git's three "rooms"? | Working area → staging area → repository |
| 7 | What is a branch, technically? | A label pointing at a specific commit — not a copy of files |
| 8 | Why do files "disappear" when you switch branches? | `switch` rewrites your actual working files to match that branch's last commit |
| 9 | What does `HEAD` normally point to? | A branch name, which in turn points to a commit |
| 10 | What makes a merge a "fast-forward"? | `main` never moved since the branch split — Git just slides the pointer forward |
| 11 | What makes a merge "three-way"? | Both branches moved; Git compares 3 points (ancestor + both tips) and combines non-overlapping changes |
| 12 | Why does a merge commit need two parents? | The incoming commit's parent is permanently the shared ancestor, not the other branch's tip — Git can't lie about that, so it creates a new joining commit |
| 13 | When does a merge conflict happen? | Both branches changed the exact same line after diverging |
| 14 | What does `<<<<<<< HEAD` mark in a conflict? | The start of your current branch's version |
| 15 | How do you resolve a conflict? | Delete all 3 marker lines, decide final content, `git add`, `git commit` |
| 16 | `git restore <file>` — what survives? | Nothing — edit discarded completely |
| 17 | `git restore --staged <file>` — what survives? | The edit itself; only staged status is undone |
| 18 | `git reset --soft HEAD~1` — what survives? | Everything, now staged, ready to redo |
| 19 | `HEAD~1` uses which character — tilde or hyphen? | Tilde (`~`), not a hyphen (`-`) |
| 20 | How is `git revert` different from `git reset`? | `revert` adds a new commit that cancels an old one — nothing erased. `reset` moves the pointer backward, old commit disappears |
| 21 | Does `git stash` (no file named) make one stash per file? | No — one single stash, bundling every uncommitted change at that moment |
| 22 | `apply` vs `pop`? | `apply` keeps a copy in the stash list; `pop` removes it after restoring |
| 23 | What does `git stash drop` do? | Discards a stash entirely, without ever applying it to files |
| 24 | What is a detached HEAD? | `HEAD` points directly at a commit, with no branch name in between |
| 25 | What's risky about committing in a detached HEAD state? | The new commit isn't protected by any branch — switching away can make it unreachable |
| 26 | How do you protect a commit made while detached? | Create a new branch pointing at it (`git switch -c`) BEFORE switching away |
| 27 | Lightweight vs annotated tag? | Lightweight = just a name on a commit. Annotated = also has tagger, date, message |
| 28 | `git fetch` vs `git pull`? | `fetch` downloads only, updates `origin/main`, touches nothing else. `pull` = fetch + merge, automatically |
| 29 | What does `origin/main` actually represent? | Your computer's saved memory of GitHub's `main` branch — a separate label on the same commit chain |
| 30 | What does `git blame` show? | For each line: which commit last changed it, who, and when |
| 31 | Is Git the same as GitHub? | No — Git is local and offline; GitHub is a separate hosted copy you explicitly push to |
| 32 | What does `-u` in `git push -u origin main` do? | Links local `main` to remote `main` permanently, so future `git push` needs no arguments |
| 33 | What does `git config --global` do? | Applies the setting to every repo on this machine, not just one |
