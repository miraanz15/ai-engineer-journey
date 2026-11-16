# Docker & Compose — Resources

All links checked on 16 Nov 2026.

---

## 📺 Watch first

**1. Docker in 100 Seconds** *(Fireship, ~2 min)*
https://www.youtube.com/watch?v=Gjnup-PuquQ
The fastest correct explanation of what a container actually is.

---

## 📖 Official docs

**Docker CLI reference**
https://docs.docker.com/reference/cli/docker/
The base `docker` command and every subcommand. Use this to look up any flag — e.g. `-d` or `-p` — straight from the source. Verified live: has a real subcommands table (`container`, `image`, `network`, `compose`, etc.), not a stub page.

---

## 🔧 When stuck

- `docker ps -a` first, always — see what actually exists before guessing
- `docker logs <name>` — check what a detached container is actually saying internally
- "Cannot connect to the Docker daemon" → `sudo systemctl start docker`

---

## 🎯 What interviewers actually ask about this topic

See all 7 in `5_interview_qa.md` — image vs container, VM vs container weight, `run` vs `start`, why `rm`/`rmi` refuse, `-p` port mapping, `images` vs `ps`, and `venv` vs Docker scope.
