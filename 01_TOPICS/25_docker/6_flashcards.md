# Docker & Compose — Flashcards

> Short Q -> A for daily spaced-repetition revision.

1. Image vs container? → Image = frozen recipe (doesn't run). Container = a real, running copy made from that recipe.
2. Why is a container lighter than a VM? → A VM carries its own full OS; a container borrows the host's OS and only packs the app.
3. Daemon vs CLI? → Daemon (`dockerd`) does the real work, running in the background; the `docker` command is just a messenger sending requests to it.
4. `venv` vs Docker — what does each isolate? → `venv` = Python packages only. Docker = the entire environment (OS libraries, any language, any service).
5. `docker run` vs `docker start`? → `run` always creates a brand NEW container. `start` restarts an EXISTING, stopped one — same ID, same data.
6. Why does `hello-world` not show in `docker ps` but does in `docker ps -a`? → `ps` shows only running containers; `hello-world` already exited, so only `ps -a` (all) shows it.
7. What does `-d` do? → "Detached" — runs the container in the background, returns the terminal immediately.
8. What does `-p host:container` actually connect? → Host's network port to the container's internal port — containers are network-isolated by default, so this opens the only doorway in.
9. Why does code on the host need `-p` to reach a database inside a container? → The code runs outside the container; without a port mapping, the sealed container is unreachable, even on the same machine.
10. Why does `docker rm` sometimes refuse? → Safety check — refuses to delete a container that's still running. Fix: stop first, or `rm -f` to force both in one step.
11. Why does `docker rmi` sometimes refuse? → Safety check — refuses to delete an image still used by any container, running or stopped.
12. `docker images` vs `docker ps`? → `images` lists saved recipes (with disk size); `ps`/`ps -a` lists running/all containers made from them.
13. Why use `docker logs` instead of just watching the terminal? → A detached (`-d`) container's output isn't streamed to your terminal at all; `logs` is the only way to see what it printed internally.
14. If you delete every container made from an image, does the image disappear too? → No — the image stays cached on disk until explicitly removed with `docker rmi`.
