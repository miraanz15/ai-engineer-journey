# Docker & Compose — Worked Examples

**Day 1** · real commands run, real output

> Each example's lesson is given twice: **Simple version** (easy words) then **Technical version** (real terms, still easy to follow) — same as `1_theory.md`.

---

## Example 1 — `hello-world`: pull + run + proof of daemon/CLI split

```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
...
Hello from Docker!
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from Docker Hub.
 3. The Docker daemon created a new container from that image...
 4. The Docker daemon streamed that output to the Docker client...
```

**Simple version:** Docker's own printed message directly tells me, step by step, that the background part (daemon) does all the real work, and the part I typed (client) only sent the request and showed me the result.
**Technical version:** this output literally confirms the client-server architecture taught in theory — the CLI sent the request, the daemon performed all four real actions (contact, pull, create, stream).

## Example 2 — `ps` vs `ps -a`, and "run always creates NEW"

```bash
$ docker ps
CONTAINER ID   IMAGE   ...   (empty — hello-world already exited)
$ docker ps -a
b853aa1e7bc6   hello-world   Exited (0)   distracted_rosalind
a1cdc86ce51d   hello-world   Exited (0)   distracted_grothendieck
```

**Simple version:** I ran the same image twice, and I got two completely separate copies, each with its own random name and its own ID. This proves `run` never reuses an old copy — it always makes a brand new one.
**Technical version:** two independent containers exist from two separate `docker run` invocations, each exited and each given Docker's auto-generated name since `--name` wasn't used — direct proof that `run` always instantiates a new container.

## Example 3 — running MongoDB detached, named, port-mapped

```bash
$ docker run -d --name learning-mongo -p 27017:27017 mongo:7
2909209e8005...
$ docker ps
2909209e8005   mongo:7   Up 4 seconds   0.0.0.0:27017->27017/tcp   learning-mongo
```

**Simple version:** `-d` means run it quietly in the background. `--name` means give it an easy name instead of a random one. `-p 27017:27017` means connect port 27017 on my normal computer to port 27017 inside the sealed box.
**Technical version:** `-d` = detached mode; `--name` = assigns a human-readable identifier; `-p host:container` = publishes host port 27017, forwarding to the container's internal port 27017.

## Example 4 — stop/start proves it's the SAME container

```bash
$ docker stop learning-mongo
$ docker ps -a          # 2909209e8005   Exited (0)   learning-mongo
$ docker start learning-mongo
$ docker ps             # 2909209e8005   Up 6 seconds learning-mongo
```

**Simple version:** the exact same ID number (`2909209e8005`) appears both before stopping and after restarting. This proves it is the same copy, with the same data, not a brand new one.
**Technical version:** identical `CONTAINER ID` before and after the stop/start cycle confirms `start` resumes the existing container rather than instantiating a new one.

## Example 5 — 💥 BREAK IT: `docker rm` refuses a running container

```bash
$ docker rm learning-mongo
Error response from daemon: cannot remove container "learning-mongo": container is running: stop the container before removing or force remove
```

**Simple version:** I tried to delete a copy while it was still running, and Docker refused, on purpose, as a safety check.
**Technical version:** a genuine safety check, triggered deliberately. Fixed two ways: stop-then-remove (two steps), or `rm -f` (one step, force):
```bash
$ docker run -d --name throwaway-test mongo:7
$ docker rm -f throwaway-test     # stopped AND removed in one command
$ docker ps -a                    # throwaway-test gone; learning-mongo untouched
```

## Example 6 — 💥 BREAK IT: `docker rmi` refuses a still-used image

```bash
$ docker images
mongo:7   c53852c00c23   1.18GB
$ docker rmi mongo:7
Error response from daemon: conflict: unable to delete mongo:7 (must be forced) - container 2909209e8005 is using its referenced image c53852c00c23
```

**Simple version:** I tried to delete the saved recipe while a running copy still depended on it, and Docker refused, again on purpose.
**Technical version:** refused because `learning-mongo` still references this image. Deliberately left `mongo:7` installed — needed again for Day 16's database work.

## Example 7 — `docker logs` on a detached container

```bash
$ docker logs learning-mongo
{"t":{"$date":"2026-11-16T16:12:48..."},"s":"I","c":"WTCHKPT",...,"msg":"saving checkpoint..."}
```

**Simple version:** this showed me real messages that MongoDB printed on the inside, even though nothing at all appeared in my terminal while it was starting up — because it was running in the background.
**Technical version:** real internal MongoDB (WiredTiger storage engine) logs — output otherwise invisible since the container runs detached (`-d`), with nothing streamed to the terminal.

## Example 8 — the wrong way: forgetting `-p`

**Simple version:** if I start the database without opening a doorway (`-p 27017:27017`), the database itself works fine on the inside. But my own code outside the box, trying to reach it, gets told "connection refused" — because no doorway was ever opened between the two.

**Technical version:** running
```bash
docker run -d --name learning-mongo mongo:7
```
without `-p 27017:27017` leaves MongoDB fully functional *inside* the container, but any host-side code (e.g. a Python script using `pymongo`) trying to connect to `localhost:27017` gets **connection refused**, because no port mapping exists. The container is alive; it's just unreachable from outside its own network namespace.
