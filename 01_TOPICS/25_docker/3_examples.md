# Docker & Compose — Worked Examples

**Day 1** · real commands run, real output

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
Docker's own output literally confirms the client (CLI) vs daemon split taught in theory.

## Example 2 — `ps` vs `ps -a`, and "run always creates NEW"

```bash
$ docker ps
CONTAINER ID   IMAGE   ...   (empty — hello-world already exited)
$ docker ps -a
b853aa1e7bc6   hello-world   Exited (0)   distracted_rosalind
a1cdc86ce51d   hello-world   Exited (0)   distracted_grothendieck
```
Two separate `hello-world` containers exist from two separate `docker run` calls — proof that `run` always creates a brand new container, never reuses an old one, each getting its own random name since `--name` wasn't used.

## Example 3 — running MongoDB detached, named, port-mapped

```bash
$ docker run -d --name learning-mongo -p 27017:27017 mongo:7
2909209e8005...
$ docker ps
2909209e8005   mongo:7   Up 4 seconds   0.0.0.0:27017->27017/tcp   learning-mongo
```
`-d` = background. `--name` = readable identity. `-p 27017:27017` = host port 27017 connected to container's internal port 27017.

## Example 4 — stop/start proves it's the SAME container

```bash
$ docker stop learning-mongo
$ docker ps -a          # 2909209e8005   Exited (0)   learning-mongo
$ docker start learning-mongo
$ docker ps             # 2909209e8005   Up 6 seconds learning-mongo
```
`CONTAINER ID` (`2909209e8005`) identical before stop and after restart — same container, same data, not a new one.

## Example 5 — 💥 BREAK IT: `docker rm` refuses a running container

```bash
$ docker rm learning-mongo
Error response from daemon: cannot remove container "learning-mongo": container is running: stop the container before removing or force remove
```
Real safety check, triggered on purpose. Fixed two ways: stop-then-remove (two steps), or `rm -f` (one step, force):
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
Refused because `learning-mongo` still depends on this image. Deliberately left `mongo:7` installed — needed again for Day 16's database work.

## Example 7 — `docker logs` on a detached container

```bash
$ docker logs learning-mongo
{"t":{"$date":"2026-11-16T16:12:48..."},"s":"I","c":"WTCHKPT",...,"msg":"saving checkpoint..."}
```
Real internal MongoDB (WiredTiger storage engine) logs — output otherwise invisible, since the container runs detached (`-d`), with nothing streamed to the terminal.

## Example 8 — the wrong way: forgetting `-p`

If `docker run -d --name learning-mongo mongo:7` is run **without** `-p 27017:27017`, MongoDB still starts fine inside the container — but any code on the host (e.g. a Python script using `pymongo`) trying to connect to `localhost:27017` gets **connection refused**, because no doorway between host and container was ever opened. The container is alive; it's just unreachable from outside itself.
