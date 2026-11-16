# Docker & Compose — Theory & Explanation

**Day 1** · Level 1 (concept + first hands-on)

---

## The problem Docker solves

Code that runs fine on one computer can break on another — wrong version of something, a missing piece, a different setting. This is the classic "but it works on my machine" problem.

**Docker** packs an application together with everything it needs — the right version of everything, all its settings — into one sealed package. This package behaves exactly the same, no matter which computer runs it.

## Image vs container

- **Image** — a frozen recipe. Describes what should be inside. Does not run by itself.
- **Container** — a real, running copy, made from that recipe. One image can make many independent running containers.

```
      IMAGE (frozen recipe)
            │
       docker run
            │
            ▼
      CONTAINER (running, alive)
```

A container is much lighter than a full virtual machine: a VM carries its own entire operating system (slow, minutes to start); a container borrows the host's OS and only packs the application itself (fast, seconds to start).

## Daemon vs CLI — client-server architecture

Docker has two parts:
- **The daemon** (`dockerd`) — a persistent background process that does the real work: building images, starting containers, managing everything.
- **The `docker` command** (the CLI) — a lightweight messenger. Typing a command sends a request to the daemon; the CLI itself does no actual container work.

If the daemon isn't running, every command fails with *"Cannot connect to the Docker daemon."* Fix (Linux): `sudo systemctl start docker`.

**Proof seen live:** `hello-world`'s own output describes this exact flow: *"1. The Docker client contacted the Docker daemon. 2. The Docker daemon pulled the image... 3. The Docker daemon created a new container... 4. The Docker daemon streamed that output to the Docker client."*

## `venv` vs Docker — what each isolates

| | What it isolates | What it does NOT isolate |
|---|---|---|
| **`venv`** | Python packages, per project | Python version itself, any non-Python software, the OS |
| **Docker** | The entire environment — OS libraries, any language, any service | — isolates everything |

Rule of thumb: different Python package versions → `venv` is enough. Non-Python software (e.g. a database), or needing identical behavior across machines → Docker. This course needs Docker specifically because MongoDB is not a Python library — `pip`/`venv` have no power over it.

## Core commands, all run live today

| Command | What it does |
|---|---|
| `docker run <image>` | Download if needed, create and start a **new** container |
| `docker ps` | Show **running** containers only |
| `docker ps -a` | Show **all** containers, including stopped ones |
| `docker run -d` | Run detached — background, terminal returned immediately |
| `docker run --name <name>` | Give the container a readable name instead of a random one |
| `docker run -p host:container` | Open a doorway — map a port on the host to a port inside the sealed container |
| `docker stop <name>` | Stop a running container gracefully (same container, can restart) |
| `docker start <name>` | Restart the **same** container — same ID, same data |
| `docker rm <name>` | Delete a container permanently — refuses if running (safety check) |
| `docker rm -f <name>` | Force: stop + delete in one step |
| `docker images` | List every image saved locally, with disk size |
| `docker rmi <image>` | Delete a saved image — refuses if any container still uses it |
| `docker logs <name>` | Show everything a container has printed internally, even detached |

## `-p host:container` — why the host needs to reach the container

A container is sealed off from the host by default, for safety, even though both run on the same physical machine. Your actual code (e.g. a Python script, running in `venv`, directly on the host — NOT inside the container) needs to reach the database running inside the container. Without `-p`, that code gets "connection refused," even though the database is technically alive.

`-p 27017:27017` means: "connect port 27017 on my host to port 27017 inside the container." The two numbers don't have to match (`-p 27018:27017` is valid) — but MongoDB's own default port is 27017, so we map it directly.

This is exactly how the real ScreenerAI job's Flask app reaches its MongoDB container.
