# Docker & Compose — Theory & Explanation

**Day 1** · Level 1 (concept + first hands-on)

---

## The problem Docker solves

Your code runs fine on your laptop. You give it to someone else, or put it on a different computer. It breaks — wrong version of something, a missing piece, a different setting. This is a very common, very old problem in programming, often called *"but it works on my machine!"*

## What Docker actually is

Docker packs your application together with everything it needs — the right version of everything, all its settings — into one sealed box. This box works exactly the same, no matter which computer it runs on.

## The two core ideas

- **Image** — a frozen recipe. It describes what should be inside the box. It does not run by itself.
- **Container** — a real, running copy, made from that recipe. One recipe can make many running copies.

```
      IMAGE (frozen recipe)
            │
       docker run
            │
            ▼
      CONTAINER (running, alive)
```

## Why a container is lighter than a full virtual computer

A full virtual machine carries its own entire operating system inside it — heavy, slow to start, taking minutes. A container is much lighter. It borrows your real computer's operating system, and only packs the application itself. This is why it starts in seconds, not minutes.

## Why this course needs it, specifically

Later in this course, a database called MongoDB is needed. MongoDB is not Python. `pip` and `venv` cannot help install it — they only handle Python libraries. Docker gives a working MongoDB in seconds, with nothing permanently installed on the real computer.

## venv vs Docker — why both exist

| | What it isolates | What it does NOT isolate |
|---|---|---|
| **`venv`** | Python libraries, per project | Python version itself, any non-Python software, the OS |
| **Docker** | The *entire* environment — OS-level pieces, any language, any software | — isolates everything |

**Rule of thumb:** "different Python packages" → `venv` is enough. "Software that isn't Python at all," or "must behave identically on a different machine" → Docker.

## The daemon vs. the `docker` command

Docker has two parts. The **daemon** is a background program doing the real work. The `docker` command typed is just a messenger, sending requests to the daemon. If the daemon isn't running, commands fail with *"Cannot connect to the Docker daemon."* Fix on Linux: `sudo systemctl start docker`.

## The core commands

| Command | What it does |
|---|---|
| `docker run <image>` | Download (if needed) and start a **new** container |
| `docker ps` | Show **running** containers |
| `docker ps -a` | Show **all** containers, including stopped ones |
| `docker stop <name>` | Stop a running container gracefully |
| `docker start <name>` | Restart the **same** container (same ID, same data) |
| `docker rm <name>` | Delete a container permanently — **refuses if the container is still running** (a safety check) |
| `docker rm -f <name>` | Stop and delete in one step, forced |
| `docker images` | List every **image** saved on this computer, with disk size |
| `docker rmi <image>` | Delete a saved image permanently — refuses if any container still uses it |
| `docker logs <name>` | Show everything a container has printed internally, even one running detached (`-d`) |

## Key flags on `docker run`

- **`-d`** — "detached," runs in the background, frees the terminal immediately
- **`--name <name>`** — a readable name, instead of a random auto-generated one
- **`-p host:container`** — opens a doorway; containers are sealed off by default, so this maps a port on the real machine to a port inside
- **`image:tag`** (e.g. `mongo:7`) — pins a specific version, so behavior never silently changes
