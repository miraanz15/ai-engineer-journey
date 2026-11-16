# Docker & Compose — Theory & Explanation

**Day 1** · Level 1 (concept + first hands-on)

> Every concept below has two parts: **Simple version** (easy words) and **Technical version** (real professional words, still written to stay easy to follow). Every command run live today is shown with its real output.

---

## 1. The problem Docker solves

**Simple version:** Sometimes code runs fine on one computer, but breaks on a different computer. The reason is usually something small and hidden — a different version of some tool, something missing, or a setting that is different. People call this "but it works on my machine."

**Technical version:** Docker packages an application together with its entire runtime environment — exact versions, settings, and dependencies — into one sealed, portable unit. This unit behaves identically no matter which machine runs it, eliminating environment-mismatch bugs.

## 2. Image vs container

**Simple version:** An **image** is like a frozen recipe. It describes exactly what should be inside, but it does not do anything by itself — it just sits there. A **container** is a real, running copy made from that recipe. One recipe (image) can make many separate running copies (containers) at the same time.

**Technical version:** An **image** is a read-only template describing a filesystem and startup configuration. A **container** is a runtime instance created from an image — one image can spawn many independent, isolated containers simultaneously.

```
      IMAGE (frozen recipe)
            │
       docker run
            │
            ▼
      CONTAINER (running, alive)
```

**Simple version of why containers are lighter than a full virtual machine:** A full virtual machine carries its own entire separate operating system inside it, which takes a long time (minutes) to start. A container does not carry its own operating system — it borrows the one already running on the host computer, and only packs the application itself. That is why it starts in seconds, not minutes.

**Technical version:** a VM virtualizes hardware and runs a full guest OS kernel of its own. A container shares the host machine's OS kernel and only isolates the application layer (files, processes, network namespace), which is why containers are dramatically faster to start and lighter on disk/memory than VMs.

## 3. Daemon vs CLI — client-server architecture

**Simple version:** Docker is actually made of two separate parts. One part runs quietly in the background all the time, and it does all the real work. The other part is just the `docker` word I type in the terminal — it does not do any real work itself, it just sends a message to the background part, asking it to do something.

**Technical version:** Docker uses a **client-server architecture**:
- **The daemon** (`dockerd`) — a persistent background process that performs the actual work: building images, starting/stopping containers, managing networking and storage.
- **The `docker` command** (the CLI, short for "command-line interface") — a lightweight client. Typing a command sends an API request to the daemon; the CLI itself performs no container work directly.

**Simple version of what happens if the background part is not running:** every single Docker command fails immediately, with a message saying it cannot reach the daemon.
**Technical version:** if the daemon isn't running, every command fails with *"Cannot connect to the Docker daemon."* Fix (Linux): `sudo systemctl start docker`.

**Proof seen live** — running the test image showed this exact flow in its own printed message:
```bash
docker run hello-world
```
Output included: *"1. The Docker client contacted the Docker daemon. 2. The Docker daemon pulled the `hello-world` image... 3. The Docker daemon created a new container... 4. The Docker daemon streamed that output to the Docker client."* This is the daemon doing all four real steps, with the CLI only acting as the messenger that triggered it and displayed the result.

## 4. `venv` vs Docker — what each isolates

**Simple version:** `venv` only keeps Python tools separate, project by project. Docker keeps the WHOLE environment separate — not just Python, but any program, any language, any setting, the works.

**Technical version:**

| | What it isolates | What it does NOT isolate |
|---|---|---|
| **`venv`** | Python packages, per project | The Python interpreter version itself, any non-Python software, the host OS |
| **Docker** | The entire environment — OS libraries, any language, any service | — isolates everything |

**Simple version of the rule of thumb:** if the only problem is different versions of a Python tool, `venv` alone is enough. If I need a whole separate program that is not even Python, like a database, or I need the exact same behavior guaranteed on any computer, I need Docker.
**Technical version:** this course needs Docker specifically because MongoDB is a standalone database server, not a Python library — `pip`/`venv` have zero power over installing or running it.

## 5. Core commands — full list, all run live today

| Command | Simple version | Technical version |
|---|---|---|
| `docker run <image>` | Download the recipe if I don't have it, then make and start a brand new running copy | Pulls the image if not cached locally, then creates and starts a **new** container from it |
| `docker ps` | Show only the copies that are currently running | Lists **running** containers only |
| `docker ps -a` | Show every copy, running or stopped | Lists **all** containers, including stopped ones |
| `docker run -d` | Run it in the background, give me my terminal back right away | "Detached" mode — runs the container in the background, returns the terminal immediately |
| `docker run --name <name>` | Give the copy an easy name instead of a random one | Assigns a human-readable container name instead of Docker's auto-generated one |
| `docker run -p host:container` | Open a doorway so things outside the sealed box can reach something inside it | Maps a port on the host machine to a port inside the sealed container's network namespace |
| `docker stop <name>` | Stop a running copy nicely — I can start the exact same one again later | Gracefully stops a running container; same container ID and data remain, restartable |
| `docker start <name>` | Start that exact same stopped copy again, same data as before | Restarts the **same** existing container — same ID, same data, not a new one |
| `docker rm <name>` | Delete a copy permanently — refuses if it's still running | Permanently deletes a container; refuses if the container is currently running (safety check) |
| `docker rm -f <name>` | Force it: stop AND delete in one single step | Forces stop + delete together in one command |
| `docker images` | List every recipe saved on this computer, with its size | Lists every image cached locally, with disk size |
| `docker rmi <image>` | Delete a saved recipe — refuses if any copy still uses it | Deletes a saved image; refuses if any container (running or stopped) still references it |
| `docker logs <name>` | Show everything a copy has printed out, even one running quietly in the background | Prints everything the container has written to its own standard output, even for a detached container |

## 6. `docker run` vs `docker start` — the key distinction

**Simple version:** `run` always makes a brand new copy from the recipe, every single time I type it. `start` does NOT make anything new — it just wakes up one specific copy that already exists but is currently stopped, with all its old data still there.

**Technical version:** `docker run` always instantiates a **new** container from the image. `docker start` resumes an **existing**, stopped container by its ID/name — same filesystem state, same data, not a fresh instance.

## 7. Why `hello-world` shows in `ps -a` but not plain `ps`

**Simple version:** `hello-world`'s whole job is to print one message and then stop immediately — it does not keep running. So right after it runs, it is already a stopped copy. `docker ps` only shows copies that are CURRENTLY running, so it shows nothing. `docker ps -a` shows every copy, running or stopped, so it shows `hello-world` there.

**Technical version:** `hello-world`'s container process exits immediately after printing its message, transitioning to the "Exited" state. `docker ps` filters to running containers only; `docker ps -a` includes all states.

```bash
docker ps
```
Output: empty table (headers only) — nothing running.
```bash
docker ps -a
```
Output: showed the `hello-world` container, status `Exited (0) ...`.

## 8. `-p host:container` — why the host needs to reach the container

**Simple version:** A container is sealed off from the rest of the computer by default, even though it is technically running on the exact same physical machine, as a safety measure. My actual code (for example a Python script, running directly on the normal computer, in `venv` — NOT inside the container) needs to reach a database that is running inside the container. Without opening a doorway with `-p`, my code gets told "connection refused" — even though the database inside is genuinely alive and working.

**Technical version:** containers are network-isolated by default — each container has its own private network namespace, separate from the host's. `-p host_port:container_port` publishes a port, creating a mapping that forwards traffic arriving on the host's port into the container's internal port. Without this mapping, the sealed container is completely unreachable from the host, even on the same machine.

**Simple version of the exact numbers:** `-p 27017:27017` means "connect port 27017 on my normal computer, to port 27017 INSIDE the sealed box." The two numbers do not have to be the same number — `-p 27018:27017` is also valid — but MongoDB's own default port is 27017, so it is simplest to map it straight across unchanged.

**Technical version:** this is exactly how the real ScreenerAI job's Flask application, running on the host, reaches its MongoDB container.

```bash
docker run -d --name learning-mongo -p 27017:27017 mongo:7
```
Output: a long container ID, confirming it started detached.
```bash
docker ps
```
Output: showed `learning-mongo`, image `mongo:7`, status `Up`, ports `0.0.0.0:27017->27017/tcp`.

## 9. Stop, start, and the two "break it on purpose" safety refusals

```bash
docker stop learning-mongo
docker ps -a   # status now "Exited"
docker start learning-mongo
docker ps      # status "Up" again, same container, same data
```

**Break it #1 — `docker rm` on a running container, simple version:** I tried to delete the container while it was still running, and Docker said no.
**Technical version:**
```bash
docker start learning-mongo
docker rm learning-mongo
```
Output: an error refusing the deletion because the container is still running — a genuine safety check, not a bug.
Fix:
```bash
docker rm -f learning-mongo    # (on a disposable throwaway container, not reused data)
```

**Break it #2 — `docker rmi` on an image still in use, simple version:** I tried to delete the saved recipe while a copy made from it still existed, and Docker said no.
**Technical version:**
```bash
docker images
docker rmi mongo:7
```
Output: an error refusing the deletion because a container (even a stopped one) still references that image — another genuine safety check.

## 10. `docker images` vs `docker ps`

**Simple version:** `images` shows the saved recipes sitting on disk. `ps`/`ps -a` shows the actual running (or all) copies made from those recipes. Deleting every copy made from a recipe does NOT delete the recipe itself — the recipe stays saved until I explicitly delete it with `docker rmi`.

**Technical version:**
```bash
docker images
```
Output: showed `mongo` and `hello-world` images, each with their tag and disk size.

## 11. `docker logs` — why it's needed for detached containers

**Simple version:** When a container runs in the background (detached, with `-d`), whatever it prints is NOT shown in my terminal at all — my terminal already moved on. `docker logs` is the only way to go back and see what that background copy actually printed.

**Technical version:** a detached (`-d`) container's standard output is not streamed to the invoking terminal. `docker logs <name>` retrieves everything the container has written internally, retroactively.

```bash
docker logs learning-mongo
```
Output: MongoDB's own internal startup log lines, proving it is genuinely running and initialized correctly, even though nothing was visible in the terminal when it started.
