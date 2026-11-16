# Docker & Compose — Practice Drills

**Day 1** · containers made here can be deleted afterward (except `learning-mongo`, kept for Day 16)

---

## Drill 1 — Run and inspect a one-shot container
```bash
docker run hello-world
docker ps
docker ps -a
```
**Question:** why does `hello-world` not appear in `docker ps`, but does appear in `docker ps -a`?
- [x] Done

## Drill 2 — Run a real, long-running service, detached and named
```bash
docker run -d --name learning-mongo -p 27017:27017 mongo:7
docker ps
```
**Question:** what does `-p 27017:27017` actually connect, and why is it needed?
- [x] Done

## Drill 3 — Stop, start, and verify identity
```bash
docker stop learning-mongo
docker ps -a
docker start learning-mongo
docker ps
```
**Done when:** the `CONTAINER ID` is identical in both `docker ps` outputs.
- [x] Done

## Drill 4 — 💥 BREAK IT: try to delete a running container
```bash
docker rm learning-mongo
```
**Expected:** refused, with an error about the container still running.
- [x] Done

## Drill 5 — Force-remove a disposable container in one step
```bash
docker run -d --name throwaway-test mongo:7
docker rm -f throwaway-test
docker ps -a
```
**Done when:** `throwaway-test` is gone; `learning-mongo` untouched.
- [x] Done

## Drill 6 — Images vs containers
```bash
docker images
```
**Question:** if every container made from `mongo:7` were deleted, would `mongo:7` disappear from this list too? Why or why not?
- [x] Done

## Drill 7 — 💥 BREAK IT: try to delete a still-used image
```bash
docker rmi mongo:7
```
**Expected:** refused, since `learning-mongo` still depends on it. Did NOT force-remove — `mongo:7` is needed again for Day 16.
- [x] Done

## Drill 8 — Read a detached container's hidden output
```bash
docker logs learning-mongo
```
**Question:** why can't you just look at your terminal to see this output, the way you could with `hello-world`?
- [x] Done

---

## Scoring

| Score | What it means |
|---|---|
| 7–8 | Ready for Day 2 |
| 5–6 | Fine — revise the gaps tomorrow morning |
| under 5 | Re-teach before moving on |

**Score: 8/8** — every drill run live, both break-it drills triggered real safety refusals, both explained correctly before running.
