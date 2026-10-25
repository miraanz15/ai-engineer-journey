# Environment, Git & GitHub — Theory & Explanation

**Day 1** · Level 1

---

# PART A — The Environment

## A1. What is a "virtual environment" (venv)?

### The problem, first

Imagine you cook two dishes in one kitchen. Dish A needs **old** flour. Dish B needs **new** flour. You only have one shelf. So you keep replacing the flour, and every time you fix one dish, you break the other.

Python has the same problem. Project A needs `langchain` version 0.1. Project B needs version 0.3. If you install them in one place, one project always breaks.

### The fix

A **virtual environment** is a separate box for each project. Project A gets its own box with its own libraries. Project B gets a different box. They never touch each other.

```
Your computer
│
├── Project A  ──► box A ──► langchain 0.1,  pandas 2.0
│
└── Project B  ──► box B ──► langchain 0.3,  pandas 2.2
                   (they never see each other)
```

### The commands

```bash
python3 -m venv venv     # make the box (do this once, per project)
source venv/bin/activate # step inside the box
deactivate               # step out of the box
```

When you are inside, your terminal shows `(venv)` at the start of the line. **If you don't see `(venv)`, you are not inside, and `pip install` will go to the wrong place.** This is the #1 beginner mistake.

---

## A2. `pip` — the library installer

`pip` downloads Python libraries from the internet and puts them in your box.

| Command | What it does |
|---|---|
| `pip install requests` | Install a library |
| `pip list` | Show what's installed |
| `pip freeze > requirements.txt` | Write the exact list to a file |
| `pip install -r requirements.txt` | Install everything from that file |

### Why `requirements.txt` matters

It's the **recipe card**. You give someone your code plus this file, and they can rebuild your exact box. Without it, nobody — including future-you — can run your project.

### Dependencies — why `pip install requests` installs more than one thing

Libraries often need other libraries to work. Installing `requests` also installs `urllib3`, `idna`, `charset_normalizer`, and `certifi` — these are `requests`'s **dependencies**, and `pip` finds and installs them automatically.

**Interview answer:** *"`pip` resolves a package's dependency tree automatically — installing one library also installs everything that library depends on, recursively."*

### Real proof the recipe card works — destroy the box, rebuild from nothing

This was actually tested, not just described:

```bash
pip install requests python-dotenv        # installs 6 packages total (4 are dependencies)
pip freeze > requirements.txt
deactivate
rm -rf venv                                # box completely destroyed
python3 -m venv venv
source venv/bin/activate
pip list                                   # only `pip` itself — proven empty
pip install -r requirements.txt
pip list                                   # all 6 packages back, exact same versions
```

**The result:** the second `pip list` showed the identical six packages at the identical versions — proof that `requirements.txt` alone is enough to rebuild the whole environment, with zero need to remember or copy the actual `venv` folder itself.

**This is also exactly why `venv/` goes in `.gitignore`** — it's heavy, machine-specific, and fully reproducible from one small file anyway, so there's no reason to ever commit it.

---

## A3. `.env` — where secrets live

Your API key is like your house key. You **never** write it in your code, because code gets shared and uploaded.

So you put it in a separate file called `.env`:

```
GROQ_API_KEY=gsk_abc123...
```

Then your code reads it:

```python
from dotenv import load_dotenv
import os

load_dotenv()                        # read the .env file
key = os.getenv("GROQ_API_KEY")      # get the value
```

**And then you hide `.env` from Git** (next section). This is not optional.

> ⚠️ **Real example:** your own work project at `ClientProject/ScreenerAI` has a real `.env` file committed to Git — with live OpenAI and Azure keys inside. Anyone with repo access can read them. We study this properly on Day 30, but remember it now: **this is the mistake, and it happens to real teams.**

---

## A4. `.gitignore` — the "do not upload" list

A plain text file listing things Git should pretend don't exist:

```
.env            # secrets
venv/           # your box — others rebuild their own
__pycache__/    # Python's temporary files
*.pyc
.DS_Store
```

**Rule of thumb:** if it's a secret, or a machine can regenerate it, it goes in `.gitignore`.

---

# PART B — Git

## B1. What is Git, really?

### The analogy

Imagine writing an essay. You save copies as you go:

```
essay.docx
essay_v2.docx
essay_final.docx
essay_final_FINAL.docx
essay_final_FINAL_actually.docx      ← we've all done this
```

This is a mess. You can't tell what changed, you can't go back cleanly, and two people can't work on it at once.

**Git is that, done properly.** It remembers every version, who changed what, when, and why — and lets you jump back to any point.

### The mental model: three rooms

```
   WORKING AREA            STAGING AREA            REPOSITORY
   (your files)            (the box you're         (saved history,
                            packing)                permanent)

   you edit files  ──►  git add  ──►  packed  ──►  git commit  ──►  saved
```

1. **Working area** — your actual files. You edit here.
2. **Staging area** — a box where you put the changes you want to save *this time*. You might change 5 files but only want to save 2 — stage only those 2.
3. **Repository** — the permanent record. Once committed, it's safe.

**Why three rooms and not two?** Because a good commit tells one story. If you fixed a bug *and* changed a colour, staging lets you commit them separately, so your history reads like sentences, not noise.

---

## B2. The core commands

```bash
git init                 # start tracking this folder
git status               # what has changed? (use this constantly)
git add file.py          # put one file in the box
git add .                # put everything in the box
git commit -m "message"  # save the box permanently
git log                  # show the history
git diff                 # show exactly what changed
```

**`git status` is your best friend.** Run it before and after everything. It tells you which room your changes are in.

### Writing a good commit message

A commit message answers: *"what would this change do if applied?"*

| ❌ Bad | ✅ Good |
|---|---|
| `update` | `Add retry logic to Groq client` |
| `fixed stuff` | `Fix crash when .env is missing` |
| `asdf` | `Add chunk overlap to RAG indexer` |

Future-you reads these when something breaks at 2am. Be kind to future-you.

---

## B3. Branches

A **branch** is a parallel copy where you can experiment without breaking the working version.

```
main      ──●──●──●─────────────●──►   (always works)
               \               /
feature         ●──●──●────────        (you experiment here)
                              ↑
                            merge back when it works
```

```bash
git branch feature-x        # create a branch
git switch feature-x        # move onto it
git switch main             # go back
git merge feature-x         # bring the changes into main
```

**Why bother?** So `main` always runs. You can try something wild on a branch, and if it fails, you just delete the branch — `main` never knew.

---

## B4. Merge conflicts — don't panic

A conflict happens when **two branches changed the same line**. Git can't guess which you want, so it asks you.

It marks the file like this:

```
<<<<<<< HEAD
print("hello from main")
=======
print("hello from feature")
>>>>>>> feature-x
```

**To fix it:**
1. Open the file
2. Decide what the line should actually say
3. Delete the `<<<<<<<`, `=======`, `>>>>>>>` markers and the version you don't want
4. `git add file.py`
5. `git commit`

That's it. A conflict is not an error — it's Git asking you a question.

---

## B5. Undoing things

This is the part that makes Git feel safe.

| I want to… | Command |
|---|---|
| Throw away changes to a file (not yet staged) | `git restore file.py` |
| Take a file out of the staging box | `git restore --staged file.py` |
| Undo the last commit, keep the changes | `git reset --soft HEAD~1` |
| Undo a commit by adding an opposite commit (safe for shared code) | `git revert <commit-id>` |
| Save half-done work and come back later | `git stash` then `git stash pop` |

**`reset` vs `revert`:** `reset` rewrites history — fine on your own machine, dangerous once pushed. `revert` adds a new commit that cancels the old one — always safe.

---

# PART C — GitHub

## C1. Git vs GitHub — not the same thing

| | What it is |
|---|---|
| **Git** | The tool on your computer that tracks versions. Works offline. |
| **GitHub** | A website that stores a copy of your Git repo online. |

Git is the camera. GitHub is the photo album you share.

```
Your computer                      GitHub (internet)
┌─────────────┐    git push  ──►   ┌─────────────┐
│ local repo  │                    │ remote repo │
└─────────────┘  ◄──  git pull     └─────────────┘
```

```bash
git remote add origin <url>   # connect local to GitHub (once)
git push -u origin main       # send your commits up
git pull                      # bring down others' commits
git clone <url>               # download a repo for the first time
```

## C2. Why GitHub matters for your job hunt

**This is not just a backup.** Recruiters and interviewers look at your GitHub. A green, active profile with real projects and clean READMEs is often what gets you the interview in the first place.

Research on 2026 AI hiring said it plainly: selection is **portfolio-driven, not degree-driven**. Your GitHub *is* the portfolio.

---

# PART D — Docker (concept only, 1 hour)

## D1. The problem Docker solves

> *"But it works on my machine!"*

Your code runs on your laptop. You send it to a colleague. It breaks. Why? Different Python version, different OS, a missing library, a different setting.

## D2. The fix

A **container** is a sealed box containing your code **and** everything it needs to run — the right Python, the right libraries, the right settings. It runs identically everywhere.

### The analogy

A **shipping container**. It doesn't matter if it's on a ship, a truck, or a train — the container is the same size and shape, so everything handles it the same way. What's inside is your business.

### Image vs container — the one distinction to remember

| | What it is |
|---|---|
| **Image** | The recipe. A frozen snapshot. Doesn't run. |
| **Container** | The meal. A running copy of an image. |

**One image → many containers.** Like one recipe → many cakes.

### Container vs virtual machine

```
VIRTUAL MACHINE                   CONTAINER
┌───────────────┐                 ┌───────────────┐
│  Your app     │                 │  Your app     │
│  Libraries    │                 │  Libraries    │
│  FULL OS      │  ← heavy, slow  └───────────────┘
└───────────────┘                   shares the host OS
   ~GBs, minutes                    ~MBs, seconds
```

A VM carries a whole operating system. A container borrows the one already running. That's why containers start in a second and a VM takes a minute.

### The commands for today

```bash
docker run <image>     # start a container
docker ps              # what's running?
docker ps -a           # everything, including stopped
docker stop <id>       # stop one
```

We go deep on Docker on **Day 27**. Today you only need the idea, plus one running container — MongoDB — because you'll need it on Day 11.

---

# The one-paragraph summary

A **venv** keeps each project's libraries separate. **pip** installs them, and `requirements.txt` records them so others can rebuild your setup. Secrets go in **`.env`**, which **`.gitignore`** hides from Git. **Git** saves versions of your work in three stages — working area → staging → repository — and **branches** let you experiment without breaking what works. **GitHub** stores a copy online, and for a job hunt it *is* your portfolio. **Docker** packages your code with everything it needs so it runs the same everywhere; an **image** is the recipe and a **container** is the running meal.
