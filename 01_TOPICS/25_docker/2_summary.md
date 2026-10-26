# Docker & Compose — Summary Sheet

> ONE PAGE. The 10 things I must remember. For fast revision.

**1. Docker solves "but it works on my machine!"** — packages an app with everything it needs into one sealed box that behaves identically everywhere.

**2. Image = frozen recipe. Container = running copy of that recipe.** One image → many containers.

**3. Containers are lighter than VMs** — they share the host's OS instead of carrying a whole separate one, so they start in seconds, not minutes.

**4. `venv` isolates only Python libraries. Docker isolates the entire environment** — any language, any non-Python software, OS-level pieces.

**5. The daemon does the real work; the `docker` command is just a messenger to it.** "Cannot connect to the Docker daemon" means the daemon isn't running — fix with `sudo systemctl start docker`.

**6. `docker run` always creates a NEW container. `docker start` resumes an EXISTING one** — same ID, same data.

**7. `docker ps` shows only running containers. `docker ps -a` shows every container ever made**, including stopped ones.

**8. `-d` (detached) frees the terminal.** Without it, the terminal stays locked to the container's live logs.

**9. `-p host:container` opens a doorway.** Containers are sealed off by default — this flag lets outside programs connect in.

**10. Always pin a version tag** (`mongo:7`, not just `mongo`) — unpinned defaults to `:latest`, which can silently change.
