# Environment, Git & GitHub — Practice Drills

**Day 1** · Live Practice Log — every drill below was actually run today, not just explained.

---

## Drill 1 — venv destroy-and-rebuild proof
```bash
pip install requests && pip freeze > requirements.txt
deactivate && rm -rf venv
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt && pip list
```
**Done when:** rebuilt `pip list` matches the original exactly. ✅ Confirmed — all 5 packages, same pinned versions.
- [x] Done

## Drill 2 — `.gitignore` before `.env`, proven via `git status`
```bash
printf "venv/\n__pycache__/\n*.pyc\n.env\n" > .gitignore
printf "FAKE_API_KEY=abc123\n" > .env
git init && git status
```
**Done when:** `venv/` and `.env` do NOT appear in untracked files, even before the first commit. ✅ Confirmed.
- [x] Done

## Drill 3 — `python-dotenv` real read-in test
```bash
pip install python-dotenv
python3 test_env.py   # load_dotenv() + os.getenv("FAKE_API_KEY")
```
**Done when:** script prints `abc123`, matching `.env`'s real value. ✅ Confirmed.
- [x] Done

## Drill 4 — 💥 BREAK IT: stuck multi-line quote in the terminal
Pasted a multi-line `echo "..."` command; the closing quote got lost, bash hung showing `>` on every line.
**Fix:** `Ctrl+C` to cancel, then used a single-line `printf "...\n..."` instead.
- [x] Done

## Drill 5 — fast-forward, clean three-way, and a real conflict (all three merge types)
```bash
git switch -c practice-feature && git commit ... && git switch main && git merge practice-feature   # Fast-forward
git switch -c feature-colors && ... && git switch main && ... && git merge feature-colors            # clean 3-way merge commit
git switch -c feature-conflict && (edit same line differently on both sides) && git merge feature-conflict  # CONFLICT
```
**Done when:** all three outcomes seen live — "Fast-forward", "Merge made by the 'ort' strategy", and `CONFLICT (content)` with markers, resolved by hand. ✅ Confirmed, with `git log --oneline --graph` showing the two-parent merge commits.
- [x] Done

## Drill 6 — real GitHub push + a real second clone (fetch vs pull)
```bash
git remote add origin <url> && git push -u origin main
git clone <url> ~/clone-test   # simulates a second computer
# in clone-test: edit, commit, push
# back in original: git fetch  → origin/main moves, main does NOT
git merge origin/main          → main catches up (fast-forward)
```
**Done when:** `origin/main` and `main` showed DIFFERENT commits right after `fetch`, proving fetch alone doesn't touch `main`. ✅ Confirmed.
- [x] Done

## Drill 7 — undo tools, all four, live
```bash
git restore README.md                        # discard unstaged edit
git restore --staged file.md                  # unstage, edit survives
git reset --soft HEAD~1                       # undo commit, keep staged
git revert HEAD --no-edit                     # safe undo on a PUSHED commit
```
**Done when:** `mistake.md`'s commit reverted — file gone from disk, but BOTH the original commit and the revert commit stayed visible in `git log`. ✅ Confirmed.
- [x] Done

## Drill 8 — full stash toolkit
```bash
git stash push -m "..." && git stash list && git stash show -p stash@{0} && git stash pop
git stash push -m "..." && git stash apply && git stash list   # still shows it
git stash drop stash@{0}
```
**Done when:** proved `apply` leaves a copy, `pop` removes it automatically. ✅ Confirmed.
- [x] Done

## Drill 9 — 💥 BREAK IT: detached HEAD, risk, and the rescue
```bash
git checkout <old-commit-hash>          # → "You are in 'detached HEAD' state"
git commit -m "Experimental commit while detached"   # shows (HEAD), no branch
git switch -c rescue-branch             # now shows (HEAD -> rescue-branch) — rescued
git switch main
git branch -d rescue-branch             # REFUSED — "not fully merged" (safety check)
git branch -D rescue-branch             # force-delete, deliberate
```
**Done when:** the unanchored commit's risk was shown, then genuinely rescued, then safely cleaned up. ✅ Confirmed.
- [x] Done

## Drill 10 — `git blame` and both tag types
```bash
git blame README.md
git tag v0.1-day1                                          # lightweight
git tag -a v0.1-day1-annotated -m "End of Day 1 setup"      # annotated
git show v0.1-day1-annotated   # shows Tagger/Date/message block the lightweight one lacks
git push origin --tags
```
**Done when:** annotated tag's extra metadata block seen, lightweight tag's absence of it confirmed. ✅ Confirmed.
- [x] Done

---

## Scoring

**Score: 10/10** — every drill run live with real output, both "break it" moments were genuine mistakes/situations caught and resolved in the moment, not staged.
