# Docker & Compose — Worked Examples

> Real examples, run live — not hypothetical.

---

## Example 1 — A one-shot container: `hello-world`

```bash
docker run hello-world
```

**Result:**
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image...
 4. The Docker daemon streamed that output to the Docker client...
```

This output walks through the exact theory: messenger → daemon → pull image → create container → stream output back.

```bash
docker ps -a
```
Shows it as `Exited (0)` — finished successfully, since its only job was printing one message.

---

## Example 2 — A real, long-running service: MongoDB

```bash
docker run -d --name learning-mongo -p 27017:27017 mongo:7
```
**Result:** a long container ID string, nothing else — because `-d` suppressed the normal logs.

**Verify it's running:**
```bash
docker ps
```
```
CONTAINER ID   IMAGE     STATUS          PORTS                                NAMES
ebb472addae4   mongo:7   Up 25 seconds   0.0.0.0:27017->27017/tcp, [::]...   learning-mongo
```
`Up 25 seconds` confirms it's alive and staying alive. The `PORTS` column confirms the doorway from `-p 27017:27017` is open.

---

## Example 3 — Stop, start, and prove it's the SAME container

```bash
docker stop learning-mongo
docker ps -a       # shows learning-mongo as "Exited"

docker start learning-mongo
docker ps
```

**Result:**
```
CONTAINER ID   IMAGE     CREATED              STATUS         NAMES
ebb472addae4   mongo:7   About a minute ago   Up 7 seconds   learning-mongo
```

**The key proof:** `CONTAINER ID` — `ebb472addae4` — is identical both before and after. `CREATED` still says "about a minute ago" (when it was first made); `STATUS` says "Up 7 seconds" (when it was most recently restarted). This is direct evidence that `docker start` reuses the existing container, rather than making a new one.

**The wrong way:** using `docker run` again when you actually wanted to resume the existing MongoDB — that creates a **second, separate** container with its own empty data, not the one already running.

---

## Example 4 — `docker images`: listing saved recipes

```bash
docker images
```
Shows every image saved on disk — not containers, the recipes themselves. Real output included both learning images (`hello-world`, `mongo:7`) and real work images (`screener-ai-backend`, `screener-scraping`), each with its disk size (`mongo:7` at 1.18GB, `hello-world` at just 21.8kB).

## Example 5 — `docker logs`: seeing a detached container's hidden output

```bash
docker logs learning-mongo
```
Printed MongoDB's internal activity logs — proof it had been working continuously, even though `-d` hid all of this from the terminal the whole time. Useful for debugging a container that isn't behaving as expected.

## Example 6 — `docker rm`: a real safety check, proven

**Deleting a stopped container works directly:**
```bash
docker rm vigilant_payne
```

**But deleting a RUNNING container is refused:**
```bash
docker rm test-remove
```
**Result:**
```
Error response from daemon: cannot remove container "test-remove": container is running: stop the container before removing or force remove
```

**Two ways to actually do it:**
```bash
docker stop test-remove && docker rm test-remove    # two steps, explicit
docker rm -f test-remove2                            # one step, forced
```
`-f` stops and removes in one command — convenient, but less safe to use as a habit, since it skips the deliberate pause.

## Example 7 — `docker rmi`: deleting an image

```bash
docker rmi hello-world
```
**Result:** `Untagged: hello-world:latest` then `Deleted: sha256:...` — confirmed gone from a follow-up `docker images`. (This would fail if any container, running or stopped, still referenced that image — another safety check.)
