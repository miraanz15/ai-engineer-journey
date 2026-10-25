# Environment, Git & GitHub — Practice Drills

**Day 1** · Do these in order. ~60 minutes.

Work inside a throwaway folder first: `~/git-practice`. Break things freely there.

---

## Drill 1 — Make a box and look inside it

```bash
mkdir ~/git-practice && cd ~/git-practice
python3 -m venv venv
source venv/bin/activate
```

**Check:** does your terminal line now start with `(venv)`?

```bash
pip list
```

**Question to answer in your own words:** why is this list so short compared to `pip list` outside the venv?

- [ ] Done

---

## Drill 2 — Install, freeze, rebuild

```bash
pip install requests python-dotenv
pip freeze > requirements.txt
cat requirements.txt
```

Now delete the box and rebuild it from the recipe:

```bash
deactivate
rm -rf venv
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pip list
```

**Done when:** `requests` is back, and you understand that `requirements.txt` was the only thing that survived.

- [ ] Done

---

## Drill 3 — Ten commits with real messages

Create a file, then make **10 separate commits**, each changing something small.

```bash
git init
echo "# Practice" > notes.md
git add notes.md
git commit -m "Add notes file"
```

Then repeat 9 more times with small edits.

**Rules:**
- Every message says *what the change does*
- No message may be `update`, `fix`, `asdf`, or `changes`

```bash
git log --oneline
```

**Done when:** `git log --oneline` reads like 10 clear sentences.

- [ ] Done

---

## Drill 4 — Watch the three rooms

```bash
echo "line one" >> notes.md
git status          # which room is this change in?

git add notes.md
git status          # now which room?

git commit -m "Add line one to notes"
git status          # and now?
```

**Write one sentence** for each of the three `git status` outputs, in your own words, saying where the change was.

- [ ] Done

---

## Drill 5 — Branch, change, merge

```bash
git branch feature-colors
git switch feature-colors
echo "blue" >> colors.md
git add colors.md
git commit -m "Add colors file with blue"

git switch main
ls                  # where did colors.md go?
```

**Question:** why did `colors.md` disappear?

```bash
git merge feature-colors
ls                  # and now?
```

- [ ] Done

---

## Drill 6 — 💥 BREAK IT: force a merge conflict and fix it

This is the most important drill today. **Do not skip it.**

```bash
# On main, change a line
git switch main
echo "hello from main" > greeting.md
git add greeting.md && git commit -m "Add greeting from main"

# On a branch, change the SAME line differently
git switch -c feature-greeting
echo "hello from feature" > greeting.md
git add greeting.md && git commit -m "Change greeting to feature version"

# Go back and merge — this will conflict
git switch main
echo "hello from main, edited" > greeting.md
git add greeting.md && git commit -m "Edit greeting on main"
git merge feature-greeting
```

Git will refuse and mark the file. **Now:**
1. `cat greeting.md` — see the `<<<<<<<`, `=======`, `>>>>>>>` markers
2. Open it, decide what the line should say, delete the markers
3. `git add greeting.md`
4. `git commit`

**Done when:** `git log --oneline` shows the merge, and `greeting.md` has no markers left.

**Then write one line:** what was Git actually asking you?

- [ ] Done

---

## Drill 7 — Undo, three ways

```bash
# (a) Throw away an unstaged change
echo "mistake" >> notes.md
git status
git restore notes.md
git status          # gone

# (b) Unstage something
echo "another mistake" >> notes.md
git add notes.md
git restore --staged notes.md
git status          # still changed, but out of the box

# (c) Undo a commit but keep the work
git add notes.md && git commit -m "A commit I regret"
git reset --soft HEAD~1
git status          # the change is back in the staging box
```

**Question:** when would `git revert` be the right choice instead of `git reset`?

- [ ] Done

---

## Drill 8 — Stash

```bash
echo "half-finished idea" >> notes.md
git stash
git status          # clean — where did it go?
git stash pop
git status          # back
```

- [ ] Done

---

## Drill 9 — Write a `.gitignore` that actually works

```bash
echo "GROQ_API_KEY=secret123" > .env
git status          # ⚠️ Git can see your secret
```

Now create `.gitignore`:

```
.env
venv/
__pycache__/
*.pyc
```

```bash
git status          # .env should now be invisible
```

**Done when:** `.env` no longer appears in `git status`.

**💥 Bonus break-it:** what happens if you `git add .env` *before* adding it to `.gitignore`, commit it, and only then add the ignore rule? Try it. (Answer: the ignore rule does nothing — Git already tracks it. You have to `git rm --cached .env`. **This is exactly the ScreenerAI mistake.**)

- [ ] Done

---

## Drill 10 — Docker: run something

```bash
docker run hello-world
docker ps -a
```

Then start MongoDB, which you'll need on Day 11:

```bash
docker run -d --name learning-mongo -p 27017:27017 mongo:7
docker ps
docker stop learning-mongo
docker ps -a        # stopped, but still there
docker start learning-mongo
```

**Question:** `mongo:7` is the **image**. `learning-mongo` is the **container**. Explain the difference in one sentence.

- [ ] Done

---

## Scoring

Count your completed drills.

| Score | What it means |
|---|---|
| 9–10 | Ready for Day 2 |
| 7–8 | Fine — note the gaps and revise tomorrow morning |
| **under 7** | **We re-teach before moving on.** Not a failure — the rule exists so nothing is built on sand. |

Record your score in `00_PLAN/PROGRESS.md`.
