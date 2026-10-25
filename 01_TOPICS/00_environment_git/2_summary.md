# Environment, Git & GitHub — Summary Sheet

> ONE PAGE. The things I must remember. For fast revision.

## Environment

**1. A venv isolates one project's libraries from every other project's.** Always check `(venv)` shows in the prompt before installing — if it's missing, `pip install` silently goes to the wrong place.

**2. `pip install <name>` also installs that library's dependencies automatically** — installing `requests` pulled in 4 extra packages it needs underneath.

**3. `requirements.txt` is the only thing worth saving, not the `venv` folder itself.** Proven live: deleted the whole `venv`, rebuilt an empty one, ran `pip install -r requirements.txt`, and got the exact same 6 packages back.

**4. `.gitignore` must exist BEFORE the first commit of anything sensitive.** Adding a rule *after* a file is already committed does nothing — Git is already tracking it. Fix requires `git rm --cached <file>`. This is the exact real mistake sitting in the ScreenerAI repo (a committed `.env`).

**5. `.env` holds secrets; code never does.** `load_dotenv()` reads the file; `os.getenv("NAME")` reads one value back into code — the secret itself is never typed directly in the code.

*(Git and Docker sections added as those parts are completed.)*
