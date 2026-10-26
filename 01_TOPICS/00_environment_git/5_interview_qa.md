# Environment, Git & GitHub — Interview Questions & Answers

> Format: simple answer → deeper answer → what they're really testing → likely follow-up.

---

### Q1: What's the difference between `git add` and `git commit`?

**Simple answer:** `git add` puts a change into a staging box — a holding area. `git commit` saves whatever's in that box permanently, with a message.

**Deeper answer:** Git has three states a change can be in — working directory, staging area, and the committed repository. Separating "add" from "commit" lets you build a commit out of only some of your changes, so one commit can tell one clean story.

**What they're really testing:** whether you understand Git has more than one stage, or think `add`+`commit` is just "save, twice."

**Likely follow-up:** "What does `git add -A` do differently from `git add .`?"

---

### Q2: Why can't you add a secret file to `.gitignore` after it's already committed?

**Simple answer:** `.gitignore` only tells Git "don't start tracking this." If Git already tracks a file, the rule does nothing — it's already saved in history.

**Deeper answer:** fixing it requires `git rm --cached <file>` to stop tracking it going forward, and if the secret must be scrubbed from history entirely, history-rewriting tools are needed — but the real first step is **rotating the leaked key immediately**, since anyone who cloned the repo already has the old history.

**What they're really testing:** real production awareness — this is a genuine, common mistake, not a textbook question.

**Likely follow-up:** "What would you do if you found a leaked API key in a repo's history right now?"

---

### Q3: What is `requirements.txt`, and why does it matter more than the `venv` folder itself?

**Simple answer:** `requirements.txt` lists every library a project needs and its version. Anyone, anywhere, can rebuild the exact same environment from this one file — the `venv` folder itself never needs to be shared or saved.

**Deeper answer:** proven directly — deleting an entire `venv`, creating a fresh empty one, then running `pip install -r requirements.txt` restores the identical set of packages at identical versions. This is exactly why `venv/` belongs in `.gitignore` — it's heavy, machine-specific, and fully reproducible from one small file.

**What they're really testing:** do you understand environment reproducibility, or do you think you need to copy the whole folder around?

**Likely follow-up:** "What's the difference between `pip freeze` and `pip list`?"

---

### Q4: What is a branch, technically?

**Simple answer:** A branch is a label, pointing at a specific commit — not a copy of files.

**Deeper answer:** switching branches physically rewrites your working files to match whatever commit that label points to. This is why files from one branch can appear to "disappear" when you switch to another — they were never part of that branch's own history.

**What they're really testing:** whether a branch is understood structurally, or just as "a parallel folder."

**Likely follow-up:** "What does `HEAD` actually point to?"

---

### Q5: What is `HEAD`?

**Simple answer:** `HEAD` is a reference to your current position. It normally points at a branch name, which in turn points at a commit.

**Deeper answer:** because of this two-step pointing, `HEAD` moves automatically whenever the branch it's attached to gets a new commit. `HEAD` can also point directly at a commit with no branch name in between — this is called a detached HEAD.

**What they're really testing:** understanding Git's reference model, not just treating `HEAD` as a synonym for "the latest commit."

**Likely follow-up:** "What's risky about making new commits in a detached HEAD state?"

---

### Q6: Fast-forward vs. three-way merge — what's the difference?

**Simple answer:** Fast-forward happens when the branch being merged into hasn't moved at all since the split — Git just slides the pointer forward. A three-way merge happens when both sides have moved, so Git actually combines two sets of changes.

**Deeper answer:** "three-way" refers to the three commits Git examines — the common ancestor, and the tip of each branch. It computes what changed from the ancestor to each tip, and if those changes don't overlap, combines them into a new merge commit with two parents.

**What they're really testing:** whether merging is understood as a structural 3-point comparison, not "Git magic."

**Likely follow-up:** "Why does a merge commit need two parents instead of one?" *(Because the incoming branch's commit permanently remembers being built on the shared ancestor, not on the other branch's current tip — Git cannot simply place one after the other without lying about that history, so it creates one new commit connecting both.)*

---

### Q7: What's a merge conflict, and how do you resolve one?

**Simple answer:** It happens when both branches changed the exact same line of the same file. Git can't guess which version you want, so it marks both inside the file and asks you to decide.

**Deeper answer:** Git inserts `<<<<<<< HEAD`, `=======`, `>>>>>>> branch-name` markers around both versions. You delete all three markers, decide the final content, then `git add` + `git commit`.

**What they're really testing:** have you actually hit and fixed a real conflict, not just heard the term.

**Likely follow-up:** "Why did a text editor open when you ran `git merge` without `-m`?" *(Any genuine two-parent merge commit needs a message — this happens for clean merges too, not just conflicts. Passing `-m` directly avoids the editor.)*

---

### Q8: `git restore`, `git restore --staged`, and `git reset --soft` — what's the difference?

**Simple answer:** They undo different *amounts* of a mistake, depending on how far it's already traveled: not staged → staged → committed.

**Deeper answer:** `restore` discards an uncommitted edit completely. `restore --staged` un-boxes a staged change but keeps the edit. `reset --soft HEAD~1` undoes an entire commit but keeps everything staged, ready to redo. (`~1` means "one commit before `HEAD`" — note the tilde, not a hyphen.)

**What they're really testing:** precise understanding of Git's three-stage model.

**Likely follow-up:** "When would you use `git revert` instead of `git reset`?"

---

### Q9: When do you use `git revert` instead of `git reset`?

**Simple answer:** `revert` creates a new commit that cancels out an old one, without erasing anything. `reset` moves the branch pointer backward, making the old commit disappear from history.

**Deeper answer:** `reset` is only safe on commits still fully private to you. `revert` is the safe choice once a commit has been pushed or pulled by anyone else — it never rewrites shared history, so other people's copies are never thrown out of sync.

**What they're really testing:** awareness of the risk of rewriting shared history — a real team-Git concern, not just a solo-Git trick.

**Likely follow-up:** "Prove, from `git log`, that `revert` didn't erase the original commit." *(The reverted commit's hash still appears in the log, with a new commit directly after it that cancels its changes — unlike `reset`, where it would vanish entirely.)*

---

### Q10: `git stash apply` vs `git stash pop` — what's the difference?

**Simple answer:** Both bring stashed changes back into your files. `apply` keeps a copy in the stash list afterward; `pop` removes it.

**Deeper answer:** use `apply` when unsure the reapplication will go cleanly — the stash stays as a backup if something goes wrong. Use `pop` once confident it's a clean, final restoration.

**What they're really testing:** whether `pop` is used out of habit without understanding the real difference.

**Likely follow-up:** "What does `git stash drop` do differently from both?" *(Discards a stash entirely without ever applying it to files.)*

---

### Q11: Does `git stash` (with no file named) create one stash per file, or one stash for everything?

**Simple answer:** One single stash, bundling every uncommitted change across every modified file at that moment.

**Deeper answer:** to stash only a specific file, it must be named directly: `git stash push -- colors.md`. Without naming anything, everything uncommitted goes into one entry.

**What they're really testing:** precise understanding of what a stash entry actually contains, not just "it saves my changes somewhere."

**Likely follow-up:** "How would you selectively stash just one of three modified files?"

---

### Q12: What is a detached HEAD, and why is it risky?

**Simple answer:** It's when `HEAD` points directly at a commit, with no branch name in between. It's risky because a new commit made in this state isn't protected by any branch — switching away can leave it unreachable and eventually deleted.

**Deeper answer:** proven directly — a commit made while detached, then abandoned by switching to `main` without protection, completely disappeared from `git log`. A second commit, protected by creating a new branch (`git switch -c`) *before* switching away, survived and was found via `git log --all`.

**What they're really testing:** have you encountered this real risk, and do you know the fix (branch it before you leave).

**Likely follow-up:** "Is a detached commit deleted immediately?" *(No — `git reflog` can often recover it for a while, as a safety net, but it shouldn't be relied upon.)*

---

### Q13: What's the difference between a lightweight tag and an annotated tag?

**Simple answer:** A lightweight tag is just a name pointing at a commit. An annotated tag also carries a tagger, a date, and a message.

**Deeper answer:** unlike a branch, neither kind of tag ever moves once created — they're frozen at the exact commit tagged. Annotated tags are the recommended choice for real version releases, since they carry proper metadata.

**What they're really testing:** whether tags are understood as permanent markers distinct from branches, which do move.

**Likely follow-up:** "How would you see the message attached to an annotated tag?" *(`git show <tag-name>`.)*

---

### Q14: What's the difference between `git fetch` and `git pull`?

**Simple answer:** `fetch` downloads the latest changes but doesn't touch your files. `pull` does that same download, then immediately merges it in, in one step.

**Deeper answer:** `fetch` only updates your computer's saved memory of the remote branch (called `origin/main`) — your own `main` and your actual files stay untouched. `git log main..origin/main` lets you inspect what's new before deciding to merge. `pull` skips that inspection step, doing both automatically.

**What they're really testing:** whether `pull` is used by habit without understanding it's really two steps combined.

**Likely follow-up:** "When would you prefer `fetch` + inspect over plain `pull`?" *(Working with a team, wanting to review incoming changes before merging — especially with your own uncommitted work present.)*

---

### Q15: What does `git blame` show you?

**Simple answer:** For each line in a file, which commit last changed it, who made that commit, and when.

**Deeper answer:** useful for understanding the history and reasoning behind a specific piece of code — e.g., finding out why a strange line exists, and who to ask about it.

**What they're really testing:** awareness of basic debugging/investigation tools beyond just writing code.

**Likely follow-up:** "What does the `^` symbol before a commit hash mean in `blame` output?" *(Marks it as the repository's very first, root commit.)*

---

### Q16: Is Git the same thing as GitHub?

**Simple answer:** No. Git tracks versions entirely on your own computer, with no internet or account needed. GitHub is a separate website hosting a copy of your repository online.

**Deeper answer:** `git init`, `add`, `commit`, branching, merging — all work offline. GitHub enters only when a "remote" is explicitly connected (`git remote add origin <url>`) and you `push`/`pull` to sync with it.

**What they're really testing:** a genuinely common beginner confusion — are these understood as two separable things?

**Likely follow-up:** "What would happen to your local history if you deleted your GitHub repo right now?" *(Nothing — local history is completely independent and fully intact.)*
