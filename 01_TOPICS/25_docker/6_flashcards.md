# Docker & Compose — Flashcards

> Short Q → A for daily spaced-repetition revision.

| # | Q | A |
|---|---|---|
| 1 | What problem does Docker solve? | "Works on my machine" — packages an app with everything it needs so it runs the same everywhere |
| 2 | Image vs container? | Image = frozen recipe; container = a running copy made from it |
| 3 | Why are containers lighter than VMs? | They share the host's OS instead of carrying their own full separate one |
| 4 | What can venv isolate that Docker doesn't need for? | Only Python libraries |
| 5 | What can Docker isolate that venv can't? | Any non-Python software, any OS-level piece, the whole environment |
| 6 | What's the Docker daemon? | The background service doing the real work; `docker` command is just a messenger to it |
| 7 | Fix for "Cannot connect to the Docker daemon"? | `sudo systemctl start docker` |
| 8 | `docker run` vs `docker start`? | `run` always creates a new container; `start` resumes an existing one, same ID |
| 9 | `docker ps` vs `docker ps -a`? | `ps` = running only; `-a` = every container, including stopped |
| 10 | What does `-d` do? | Runs the container in the background, frees the terminal immediately |
| 11 | What does `--name` do? | Gives a readable name instead of a random auto-generated one |
| 12 | What does `-p host:container` do? | Maps a port on the real machine to a port inside the sealed container |
| 13 | Why pin `mongo:7` instead of just `mongo`? | Unpinned defaults to `:latest`, which can silently change |
| 14 | What confirms two `docker ps` outputs show the SAME container? | Identical `CONTAINER ID` |
| 15 | What does "Exited (0)" mean? | The container finished successfully, no errors |
| 16 | Why does `docker rm` refuse a running container? | Safety check — deleting a running container could lose data with no warning |
| 17 | How do you force-delete a running container in one step? | `docker rm -f <name>` |
| 18 | `docker rm` vs `docker rmi`? | `rm` deletes a container; `rmi` deletes an image (the recipe) |
| 19 | What does `docker images` show? | Every saved image on this computer, with disk size |
| 20 | What does `docker logs <name>` show? | Everything a container has printed internally, even one running detached (`-d`) |
| 21 | How do you watch logs live instead of a one-time dump? | `docker logs -f <name>` |
