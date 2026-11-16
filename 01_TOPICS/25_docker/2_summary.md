# Docker & Compose — Summary Sheet

**The 10 things to remember:**

1. **Image** = frozen recipe (doesn't run). **Container** = running copy made from it.
2. Container is lighter than a VM because it borrows the host's OS instead of carrying its own.
3. **Daemon** does the real work in the background; the `docker` command is just a messenger.
4. `venv` isolates only Python packages; **Docker isolates everything** (OS, any language, any service).
5. `docker run` always makes a NEW container; `docker start` restarts the SAME one (same ID, same data).
6. `ps` = running only; `ps -a` = everything, including stopped.
7. `-d` detached, `--name` readable identity, `-p host:container` opens the only doorway into a sealed container.
8. `rm`/`rmi` both have safety refusals: won't delete a running container, won't delete a still-used image. `-f` forces it.
9. `docker logs` is the ONLY way to see a detached container's internal output — it's not streamed to your terminal.
10. Deleting every container made from an image does NOT delete the image — it stays cached until `docker rmi`.
