# Environment, Git & GitHub — Worked Examples

> Real examples, run live in `~/git-practice`, with real commit hashes — not hypothetical.

---

## Example 1 — Branching: a commit's files disappear and reappear

```bash
git branch feature-colors
git switch feature-colors
echo "blue" >> colors.md
git add colors.md && git commit -m "Add colors file with blue"

git switch main
ls          # colors.md is GONE
```

**Why `colors.md` disappeared:** a branch is a pointer to a commit, not a copy of files. Switching branches physically rewrites your working files to match whatever that branch's last commit looked like. `main` never had `colors.md` in its own history — it genuinely was never part of `main`'s story.

```bash
git merge feature-colors
ls          # colors.md is BACK
```

**Result:** `Fast-forward` — because `main` hadn't moved since the branch split, Git just slid its pointer forward. No real combining needed.

---

## Example 2 — A clean three-way merge (both sides moved, different files)

```bash
git switch -c feature-extra
echo "a feature line" >> notes.md
git commit -am "Add a line on feature-extra"

git switch main
echo "a main-only line" >> colors.md
git commit -am "Add a line on main, independently"

git merge feature-extra
```

**Result:** `Merge made by the 'ort' strategy` (a real merge commit, not a fast-forward).

**The picture that explains why a new commit was needed:**
```
        [colors commit] Start
        /        \
  [main's commit]  [feature-extra's commit]
        \        /
      [MERGE commit] — has TWO parents
```

`feature-extra`'s commit permanently remembers it was built on the shared starting point — not on `main`'s new commit. That memory cannot change. So Git cannot simply place one after the other in a single line; it must create one new commit with two parents, to honestly record that two separate stories joined here.

**Verified in real history** (`git log --oneline --graph`):
```
*   4df8eb0 (HEAD -> main) Merge branch feature-extra into main
|\
| * 1a76563 (feature-extra) Add a line on feature-extra
* | 770e889 Add a line on main, independently
|/
* 481b3bc (feature-colors) Add colors file with blue
```
The merge commit (`4df8eb0`) has two lines feeding into it — real, structural proof of two parents.

---

## Example 3 — A real conflict (both sides moved, SAME line)

```bash
git switch main
echo "hello from main" > greeting.md
git commit -am "Add greeting from main"

git switch -c feature-greeting
echo "hello from feature" > greeting.md
git commit -am "Change greeting to feature version"

git switch main
echo "hello from main, edited" > greeting.md
git commit -am "Edit greeting on main"

git merge feature-greeting
```

**Result:**
```
CONFLICT (content): Merge conflict in greeting.md
Automatic merge failed; fix conflicts and then commit the result.
```

**What `cat greeting.md` showed:**
```
<<<<<<< HEAD
hello from main, edited
=======
hello from feature
>>>>>>> feature-greeting
```

- Between `<<<<<<< HEAD` and `=======` — `main`'s version (`HEAD` = "where you are now")
- Between `=======` and `>>>>>>> feature-greeting` — the incoming branch's version

**Resolving it:**
1. Delete all three marker lines
2. Write the final content yourself — there is no "correct" answer, that's the whole point of a conflict
3. `git add greeting.md` (this also tells Git "I resolved this file")
4. `git commit -m "..."` — give the message directly to avoid the editor opening

**The wrong way, actually made during this session:** writing a note describing the edit ("...and from feature removed") instead of just the clean final content. The goal is plain, normal text — not a description of what you changed.

**Verified in real history:**
```
*   d395655 (HEAD -> main) resolve merge conflict in greeting.md
|\
| * b04513e (feature-greeting) Change greeting to feature version
* | baaad5d Edit greeting on main
|/
* 6da5f80 Add greeting from main
```
Same two-parent shape as Example 2 — proving a conflict-resolution merge and a clean merge produce the identical structure; only the *process* of getting there differs.

---

## Example 4 — The `nano` editor opening unexpectedly

When running `git merge <branch>` **without** `-m`, and the merge genuinely needs a new two-parent commit (not a fast-forward), Git opens a text editor for the commit message. This is normal, and has **nothing to do with whether there was a conflict** — it happens for clean three-way merges too.

**To avoid the editor entirely**, always supply the message directly:
```bash
git commit -m "Merge branch feature-extra into main"
```

**If stuck inside the editor:** `Ctrl+O` then `Enter` saves; `Ctrl+X` exits. If the terminal display seems frozen or unresponsive while inside it, the most reliable fix is closing the terminal window entirely, opening a new one, reactivating the venv, and checking `git status` — it will correctly show `"All conflicts fixed but you are still merging"` (a generic message shown for *any* unfinished merge, resolved or not) with instructions to finish via a plain `git commit -m "..."`.

---

## Example 5 — Undo, three ways, same starting mistake

```bash
# (a) Not staged yet — discard completely
echo "a mistake" >> notes.md
git restore notes.md
# proven: cat notes.md shows the line is gone entirely

# (b) Already staged — un-box it, keep the edit
echo "a staged test line" >> colors.md
git add colors.md
git restore --staged colors.md
git status
# proven: colors.md moved back to "not staged," but is still listed as modified — the edit survived

# (c) Already committed — undo the commit, keep the work
git add colors.md && git commit -m "A commit I will immediately regret"
git reset --soft HEAD~1
git log --oneline -3    # the "regret" commit is gone
git status               # colors.md is back in the staging box
```

**Common typo to watch for:** `HEAD~1` uses a **tilde** (`~`), not a hyphen (`-`). `HEAD-1` is not valid syntax and produces `fatal: ambiguous argument`.

---

## Example 6 — `git revert`: undoing safely, without erasing history

```bash
git commit -am "Add something we will revert"    # creates commit 58da20f
git revert 58da20f
git log --oneline -5
```

**Result (the proof that matters):**
```
2cae90d  Revert "Add something we will revert"   ← new commit, cancels the change
58da20f  Add something we will revert             ← STILL VISIBLE, not erased
```

**The key difference from `reset --soft`:** with `reset`, `58da20f` would have vanished from the log completely — the branch pointer moves backward, forgetting it existed. With `revert`, it stays forever, as an honest record. A new commit (`2cae90d`) is added on top, which cancels out the old one.

Confirmed the actual file content reverted too — `cat notes.md` no longer shows the "something to revert" line, even though the commit describing it still appears in history.

**When to use which:** `revert` on anything already shared with others (pushed, or pulled by a teammate) — never rewrites, always safe. `reset` only on commits still fully private to you, never pushed anywhere.

---

## Example 7 — The full stash toolkit

```bash
echo "idea one: improve colors" >> colors.md
git stash push -m "idea one"      # working tree clean — change is tucked away

echo "idea two: improve notes" >> notes.md
git stash push -m "idea two"
git stash list                     # two stashes: stash@{0} "idea two", stash@{1} "idea one"

git stash show stash@{1} -p        # preview "idea one" without touching anything

git stash apply stash@{1}          # brings it back into files
git stash list                     # BOTH stashes still listed — apply never deletes

git stash pop stash@{0}            # brings "idea two" back, AND removes it
git stash list                     # only one left — and its index SHIFTED from {1} to {0}

git stash drop                     # discards the last one, without ever applying it
git stash list                     # empty
```

| Command | Touches your files? | Removes from the list? |
|---|---|---|
| `show` | No | No |
| `apply` | Yes | No |
| `pop` | Yes | Yes |
| `drop` | No | Yes |

**Key fact confirmed live:** `git stash push` with no file named bundles **every** uncommitted change, across every modified file, into **one single stash** — not one stash per file. To stash only a specific file, name it: `git stash push -- colors.md`.

**Stash index numbers are not fixed labels** — they're just "position from the newest." Removing an earlier stash shifts every later one's number.

---

## Example 8 — Detached HEAD: the risk, and the fix

**Normal state (three boxes):**
```
HEAD ──► main ──► [commit]
```

**Detached state (two boxes — the branch name is missing):**
```
HEAD ──────────► [commit]
```

```bash
git switch --detach d395655
```
**Result:** `HEAD is now at d395655 ...`

```bash
git log --oneline -3
```
**Result:** `d395655 (HEAD) ...` — notice **no branch name** next to `HEAD` this time, unlike every earlier log (`HEAD -> main`).

```bash
git status
```
**Result:** `HEAD detached at d395655` — Git's own words, confirming it explicitly.

**The risk, proven:**
```bash
echo "a commit made while detached" >> detached_test.md
git commit -am "A commit made in detached HEAD state"
# commit message itself says: [detached HEAD 26bb0b6] ...

git switch main
git log --oneline -3
# 26bb0b6 is COMPLETELY GONE from this history — lost, because nothing protected it
```

**The fix, proven:**
```bash
git switch --detach d395655
echo "a safe commit while detached" >> detached_test2.md
git commit -am "A commit I will protect with a branch"
git switch -c save-my-work    # create a branch HERE, before switching away

git switch main
git log --oneline --all | grep 51528f1
# FOUND — this one survived, because save-my-work protects it
```

**The rule:** if you commit while detached and want to keep it, create a branch pointing at it **before** switching anywhere else. (A detached commit isn't instantly deleted forever — `git reflog` can sometimes recover it — but that's a safety net, not something to rely on; covered properly on Day 43.)

---

## Example 9 — `git blame`: who wrote this line, and when

```bash
git blame notes.md
```

**Real result:**
```
^c14b324 (MiraanEjaz 2026-10-25 21:34:57 +0000 1) This is practice
1a76563c (MiraanEjaz 2026-10-25 21:40:34 +0000 2) a feature line
```

Each line shows: the commit that added it, who made that commit, the exact timestamp, and the line number. The `^` before the first hash marks it as the repository's very first (root) commit — no parent before it.

---

## Example 10 — Tags: lightweight vs. annotated

```bash
git tag v1.0
git log --oneline -3
```
**Result:** `2cae90d (HEAD -> main, tag: v1.0) Revert "..."` — the tag is permanently attached to this one commit. Unlike a branch, this tag will never move, even as `main` gains new commits.

**Annotated tag — carries real metadata:**
```bash
git tag -a v1.0-annotated -m "First real version"
git show v1.0-annotated
```
**Result:**
```
tag v1.0-annotated
Tagger: MiraanEjaz <miraanejaz15@gmail.com>
Date:   Mon Oct 26 21:37:10 2026 +0000

First real version

commit 2cae90d... (HEAD -> main, tag: v1.0-annotated, tag: v1.0)
...
```

**The difference:** a lightweight tag is just a name on a commit. An annotated tag carries its own tagger, date, and message — the recommended choice for real version releases.

---

## Example 11 — `fetch` vs `pull`, proven with a real second clone

**Setup:** cloned the real GitHub repo to a second folder, pushed a change from the original.

**`fetch` first — look before touching anything:**
```bash
git fetch
```
**Result:** `d43af8a..f91bf01  main -> origin/main` — this means: "GitHub's `main` moved from the old commit to the new one. I've updated my own memory of that (`origin/main`) to match." It says nothing about the local working `main` branch, because `fetch` never touches that.

```bash
git log --oneline -2    # still shows the OLD commit — files untouched
ls                        # FETCH_PULL_TEST.md still missing — proven
```

**Inspect before merging:**
```bash
git log main..origin/main --oneline
# shows exactly the one new commit, before deciding anything
```

**Then merge deliberately:**
```bash
git merge origin/main
# Fast-forward — file appears
```

**`pull` — the same two steps, automatically, in one command:**
```bash
git pull
```
**Result (both steps visible in one output):**
```
f91bf01..b1c7759  main -> origin/main     ← this IS the fetch step
Updating f91bf01..b1c7759
Fast-forward                              ← this IS the merge step
```

**The complete rule:** `fetch` alone downloads history and updates `origin/main`, but never touches your files or local `main`. `pull` does that same fetch, then immediately merges, with no pause to inspect first. Prefer `fetch` + inspect when working with a team and you want to review incoming changes before merging them in.
