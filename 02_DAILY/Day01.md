# Day 1 — Environment, Git, GitHub & Docker

**Confidence:** 4/5 overall · **Status: fully complete**

## Covered
- Environment: venv, pip (proven with a destroy-and-rebuild test), `.env`, `.gitignore` → `01_TOPICS/00_environment_git/`
- Full Git: branching, all 3 merge types, a real conflict, all 3 undo methods, `revert`, full stash toolkit, detached HEAD (risk + fix), `git blame`, tags, `fetch` vs `pull`
- Real GitHub setup from a fresh `git init`, under own identity
- Docker: image vs container, daemon vs CLI, full container lifecycle with MongoDB → `01_TOPICS/25_docker/`

## Built
- `~/MiraanLearning/Learning` — real repo, real history, pushed to GitHub
- `~/git-practice` — disposable practice repo for every merge/stash/undo/tag drill
- `learning-mongo` — a real MongoDB container, pulled, run, stopped, restarted

## Key moments worth remembering
- **Full restart of Day 1** — redone from scratch (GitHub repo deleted and recreated, local git history wiped and reinitialized, Docker containers removed and rebuilt) after the first pass, to fix concentration/distraction issues. The 45-day plan scaffold, `CLAUDE.md`, and all other topic folders were deliberately kept untouched.
- **Picture-by-picture teaching method** worked where plain sentences didn't — specifically for understanding why a merge commit needs two parents, and for detached HEAD. Confirmed step by step with yes/no checks rather than long paragraphs.
- **Real confusion handled live:** got stuck inside the `nano` editor during a merge commit message — resolved by closing the terminal and redoing the commit with `-m` directly instead of the interactive editor.
- **A genuine typo caught:** `HEAD-1` (hyphen) vs `HEAD~1` (tilde) — these are different characters with different meanings to Git.
- **Teaching style correction locked into `CLAUDE.md` permanently:** "simple" means easy words, not short answers — full depth must stay, even in simple language.

## Revised today
N/A — Day 1, nothing earlier to revise yet.

## Tomorrow
Day 2 — Python for AI.
