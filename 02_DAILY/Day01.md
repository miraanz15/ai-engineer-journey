# Day 1 — Environment, Git, GitHub & Docker

**Status: fully complete.**

## Covered
- Environment: `python3` vs `python`, VS Code, terminal → `01_TOPICS/00_environment_git/`
- `venv` + `pip`, proven with a real destroy-and-rebuild test
- `.env` + `.gitignore` ordering rule, proven live before the first commit; `python-dotenv` read-in test
- Full Git: `init`/`add`/`commit`/`log`/`diff`, three-stage model, good commit message feedback
- Branching/merging: fast-forward, clean three-way, and a real caused-and-resolved conflict
- GitHub: new repo created (old one deleted and recreated from scratch), real first push, verified live
- `fetch` vs `pull`, proven with a real second clone acting as a separate collaborator
- Full undo toolkit: `restore`, `restore --staged`, `reset --soft`, `revert` (incl. the reset-vs-revert safety distinction), full stash toolkit (`push`/`show`/`apply`/`pop`/`drop`)
- `HEAD`, detached `HEAD` (risk caused live, then rescued with a branch), `git blame`, lightweight + annotated tags
- Docker: image vs container, daemon vs CLI, `venv` vs Docker scope, full MongoDB container lifecycle incl. two real safety-refusal "break it" moments (`rm` on a running container, `rmi` on a used image)

## Real mistakes made and fixed (today)
- `python --version` failed (`command not found`) — no plain `python` on this machine, only `python3`.
- `source venv/bin.activate` typo (dot instead of slash) — corrected to `/`.
- A multi-line `echo "..."` with quotes got mangled on paste, leaving bash stuck waiting for a closing quote — fixed with `Ctrl+C` and a single-line `printf` instead.
- Renamed the default branch `master` → `main` mid-session, after asking why it wasn't `main` by default locally.
- Deliberately entered detached `HEAD`, made an unanchored commit, and rescued it with `git switch -c` before it could be lost.
- `git branch -d rescue-branch` correctly refused (unmerged) — force-deleted with `-D` on purpose.
- Two transient `git push` failures ("Could not resolve host") — succeeded on retry, confirmed as a network hiccup, not a Git problem.

## Plan change made today
Added **rule 8b** to `CLAUDE.md` — a same-day, end-of-day revision pass over *today's own* new flashcards, separate from tomorrow morning's full cumulative pass over all prior days.

## Where everything lives
- `01_TOPICS/00_environment_git/` — all 8 files, fully written
- `01_TOPICS/25_docker/` — all 8 files, fully written
- `05_REVISION/flashcards_all.md` — Day 1 section added (44 new cards total)
- `00_PLAN/SKILLS_CHECKLIST.md` — all 18 Day 1 boxes checked
