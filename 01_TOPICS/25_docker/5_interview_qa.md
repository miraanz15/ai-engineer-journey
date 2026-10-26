# Docker & Compose — Interview Questions & Answers

> Format: simple answer → deeper answer → what they're really testing → likely follow-up.

---

### Q1: What's the difference between a Docker image and a Docker container?

**Simple answer:** An image is a frozen recipe. A container is a running, live copy made from that recipe.

**Deeper answer:** one image can produce many containers simultaneously, each independent and isolated. The image itself never changes once built; only new containers get created from it.

**What they're really testing:** the single most common Docker confusion — treating "image" and "container" as interchangeable.

**Likely follow-up:** "Can you have multiple containers from the same image at once?" *(Yes — this is exactly how scaling works: run 5 copies of the same web server image as 5 separate containers.)*

---

### Q2: Why use Docker instead of a virtual machine?

**Simple answer:** A container is much lighter — it shares the host's operating system instead of carrying its own separate one, so it starts in seconds instead of minutes.

**Deeper answer:** a VM virtualizes hardware and runs a full guest OS with its own kernel. A container virtualizes at the OS level, sharing the host's kernel and using OS features for isolation instead of full hardware emulation.

**What they're really testing:** whether containers are understood as "mini VMs" (wrong) or something structurally different (right).

**Likely follow-up:** "When would you still need a full VM instead of a container?" *(When a genuinely different kernel/OS is needed than the host, or stronger hardware-level security isolation.)*

---

### Q3: `docker run` vs `docker start` — what's the difference?

**Simple answer:** `run` always creates a brand-new container. `start` resumes an existing, previously-stopped container — same container, same data, same ID.

**Deeper answer:** confusing these is a real, common mistake — using `run` when you meant `start` creates a second, separate container with fresh/empty data. This is exactly why naming containers (`--name`) matters.

**What they're really testing:** have you actually run a persistent service (like a database) more than once, or only one-shot containers?

**Likely follow-up:** "What happens to a container's data if you `docker rm` it?" *(Gone, unless the data was in a volume mounted outside the container — Day 27, Docker L2.)*

---

### Q4: What does `-d` do on `docker run`?

**Simple answer:** Runs the container in the background, freeing the terminal immediately, instead of locking it to the container's live logs.

**Deeper answer:** without `-d`, the container runs in the foreground — your terminal is attached, streaming logs continuously. Closing that terminal typically also stops the container. `-d` decouples the container's lifecycle from the terminal session.

**What they're really testing:** fluency running something meant to stay alive (a database, a server), not just quick one-off commands.

**Likely follow-up:** "How do you view a detached container's logs later?" *(`docker logs <name>`.)*

---

### Q5: What does `-p 27017:27017` do, and why is it needed?

**Simple answer:** Containers are sealed off from the outside by default. This flag maps a port on the real machine to a port inside the container, so other programs can connect to what's running inside.

**Deeper answer:** the format is `host_port:container_port` — they don't have to match. Keeping them the same is just a common convention when there's no conflict.

**What they're really testing:** understanding that container networking is isolated by default, and ports are an explicit opt-in.

**Likely follow-up:** "What happens if two containers both try to map to host port 27017?" *(The second `docker run` fails — that host port is already claimed.)*

---

### Q6: Why pin `mongo:7` instead of just `mongo`?

**Simple answer:** Plain `mongo` defaults to `mongo:latest`, which points at whatever the newest version is right now — that can silently change over time, breaking reproducibility.

**Deeper answer:** "it worked yesterday, broke today, nobody changed any code" is a classic symptom of relying on `:latest` — a new upstream image version got pulled unexpectedly. Pinning keeps behavior predictable.

**What they're really testing:** production awareness around reproducibility, not just "does it run right now."

**Likely follow-up:** "Difference between pinning a major version vs. an exact version?" *(Major version still allows minor/patch updates; exact pinning freezes it completely.)*

---

### Q7: Why does `docker rm` refuse to delete a running container?

**Simple answer:** It's a safety check — deleting a running container could lose important data instantly, with no warning, so Docker makes you be deliberate about it.

**Deeper answer:** real error seen: `"cannot remove container: container is running: stop the container before removing or force remove"`. Two fixes: stop it first (`docker stop` then `docker rm`), or force it in one step (`docker rm -f`), which stops and removes together — convenient, but risky as a default habit since it skips the deliberate pause.

**What they're really testing:** awareness of Docker's built-in safety guards, not just the happy-path commands.

**Likely follow-up:** "What does `docker rmi` do, and when would it also refuse?" *(Deletes a saved image; refuses if any container — running or stopped — still references it.)*

---

### Q8: What's the difference between `docker rm` and `docker rmi`?

**Simple answer:** `rm` deletes a **container** (a running instance). `rmi` deletes an **image** (the recipe itself), freeing disk space.

**Deeper answer:** you can have zero containers from an image and the image still sits on disk, taking up space — `docker images` reveals this. Cleaning up disk space over time often means removing unused containers *and* unused images separately.

**What they're really testing:** the image/container distinction applied practically, not just as a definition.

**Likely follow-up:** "How would you see which images are currently unused by any container?" *(`docker image prune` — a cleanup command, broader scope than Day 1 but good to know exists.)*

---

### Q9: What does `docker logs` show, and why does it matter for a detached container?

**Simple answer:** It shows everything a container has printed internally — even though `-d` (detached mode) hides all of that from your terminal in real time.

**Deeper answer:** proven directly on a running MongoDB container — `docker logs learning-mongo` revealed continuous internal activity messages that were never visible otherwise. This is the main tool for debugging a detached container that isn't behaving as expected.

**What they're really testing:** do you know how to actually investigate a background service, or only how to start one.

**Likely follow-up:** "How would you watch logs live, as they happen, instead of a one-time dump?" *(`docker logs -f <name>` — `-f` for "follow.")*

---

### Q10: What's the Docker daemon, and what does "Cannot connect to the Docker daemon" mean?

**Simple answer:** The daemon is a background service doing all the real work. The `docker` command typed is just a messenger to it. The error means the daemon isn't running — nobody's there to receive the request.

**Deeper answer:** on Linux, the daemon typically runs as a systemd-managed service. If not started, every `docker` command fails this way, regardless of whether the command itself is correct. Fix: `sudo systemctl start docker`.

**What they're really testing:** can you actually troubleshoot a broken Docker setup, not just run the happy path.

**Likely follow-up:** "How do you make Docker start automatically on boot?" *(`sudo systemctl enable docker`.)*
