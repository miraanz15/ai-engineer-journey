# Environment, Git & GitHub — Interview Questions & Answers

Format: **Simple answer → Deeper answer → What they're really testing → Likely follow-up**

---

**1. What's the difference between `git add` and `git commit`?**
- **Simple:** `add` moves a change into the staging area, ready to be saved. `commit` actually saves it permanently as a snapshot.
- **Deeper:** `add` stages a file into the index. `commit` creates a new commit object from whatever's currently staged, with author/timestamp/message metadata, and a pointer to its parent commit.
- **Testing:** Whether you understand Git's three-stage model, not just the two commands in isolation.
- **Follow-up:** "What if you edit a file after staging it, then commit?" → Only the staged version gets committed; the newer unstaged edit stays as a pending change.

**2. What's the difference between `git reset` and `git revert`?**
- **Simple:** `reset` rewrites history, as if a commit never happened — safe only if it's still private. `revert` adds a new commit that undoes the old one, keeping history honest — safe even if already pushed.
- **Deeper:** `reset --soft/--mixed/--hard` moves the branch pointer backward (varying what happens to staged/working changes). `revert` computes the inverse diff of a target commit and applies it as a brand new commit, never deleting the original.
- **Testing:** The single most common Git interview question — tests whether you understand the danger of rewriting shared history.
- **Follow-up:** "What happens if you `reset` a commit that's already been pushed and someone else pulled it?" → Their history and yours now disagree; their next push/pull gets confusing conflicts — this is exactly why `revert` exists.

**3. What's a merge conflict, and how do you resolve one?**
- **Simple:** Two branches changed the same line differently; Git can't guess which to keep, so it asks you to decide.
- **Deeper:** Git inserts conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) directly into the file, showing both versions. You edit the file to the intended final content, delete the markers, then `git add` + `git commit` — which produces a merge commit with two parents.
- **Testing:** Whether you've actually resolved one by hand, not just know the term.
- **Follow-up:** "Can you abort a merge mid-conflict?" → Yes, `git merge --abort` reverts to the state before the merge attempt (not practiced hands-on yet).

**4. `git fetch` vs `git pull` — what's the real difference?**
- **Simple:** `fetch` checks for updates but doesn't touch your files. `pull` checks AND immediately merges them in.
- **Deeper:** `fetch` only updates the local remote-tracking branch (`origin/main`); your working branch is untouched. `pull` = `fetch` + `merge origin/main`, automatically, in one step.
- **Testing:** Whether you understand that `origin/main` is a local cache, not a live connection to GitHub.
- **Follow-up:** "Why would you ever prefer `fetch` over `pull`?" → To inspect incoming changes before merging, rather than merging blindly.

**5. What is detached `HEAD`, and why is it risky?**
- **Simple:** `HEAD` points directly at one commit instead of a branch name. Any new commit made there has nothing anchoring it, and can be lost.
- **Deeper:** Commits made in detached `HEAD` state aren't reachable from any branch reference; Git's garbage collector can eventually delete unreachable commits. The fix is creating a branch (`git switch -c <name>`) before switching away, to anchor the commit.
- **Testing:** Whether you've actually hit this scenario and recovered from it, a real practical safety issue.
- **Follow-up:** "How would you even notice you were in detached HEAD?" → Git prints an explicit warning on checkout, and `git log`/`git branch` show `(HEAD)` with no branch name attached.

**6. What's the difference between a lightweight and an annotated tag?**
- **Simple:** Lightweight is just a name on a commit. Annotated carries extra info — who made it, when, and why.
- **Deeper:** A lightweight tag is a simple ref pointing at a commit. An annotated tag is a full Git object storing tagger, date, and message, shown separately by `git show` before the commit info — recommended for real releases.
- **Testing:** Whether you know tags exist for a reason (marking releases) and aren't just "another branch."
- **Follow-up:** "Do tags get pushed with a normal `git push`?" → No — need `git push origin --tags` explicitly.

**7. Why must `.gitignore` be created before a sensitive file like `.env`?**
- **Simple:** `.gitignore` only protects future commits. If a secret is committed even once, it's permanently in history.
- **Deeper:** Removing an already-committed secret requires rewriting history (`filter-branch`/`BFG`, destructive and advanced) — and if it was ever pushed, the secret should be treated as compromised regardless, since someone may have already seen or cloned it.
- **Testing:** Real-world security hygiene, not just Git mechanics — a genuinely common incident (happened on the real ScreenerAI project).
- **Follow-up:** "What would you do if you discovered a secret already committed and pushed?" → Rotate/invalidate the actual secret immediately (new API key, etc.) — rewriting history alone doesn't undo exposure.
