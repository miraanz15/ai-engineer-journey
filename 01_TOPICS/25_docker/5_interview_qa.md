# Docker & Compose — Interview Questions & Answers

Format: **Simple answer → Deeper answer → What they're testing → Likely follow-up**

---

**1. What's the difference between an image and a container?**
- **Simple:** An image is a frozen recipe. A container is a real, running copy made from that recipe.
- **Deeper:** An image is an immutable, layered filesystem snapshot — a build artifact. A container is a running instance of that image, with its own process, filesystem view, and network namespace. One image can spawn many independent containers.
- **Testing:** Whether you understand Docker's core abstraction, not just command syntax.
- **Follow-up:** "Can you change a running container's image?" → No, not directly; you'd build a new image and run a new container from it.

**2. Why is a container lighter than a virtual machine?**
- **Simple:** A VM brings its own whole operating system inside it. A container borrows the host's OS and only packs the app.
- **Deeper:** VMs virtualize hardware and run a full guest OS kernel — heavy, slow (minutes to boot). Containers share the host kernel and use OS-level isolation (namespaces, cgroups on Linux) — lightweight, fast (seconds to start).
- **Testing:** Whether you understand *why* containers exist, not just that they do.
- **Follow-up:** "Then why aren't containers a security risk, sharing the kernel?" → They are more surface area than a VM, but namespaces/cgroups provide strong isolation for most use cases; VMs are stronger isolation when truly needed.

**3. `docker run` vs `docker start` — what's the real difference?**
- **Simple:** `run` always makes a brand new container. `start` turns an existing, stopped one back on.
- **Deeper:** `run` = create + start, always producing a new container ID. `start` restarts an existing container object, preserving its ID and filesystem state (e.g., a database's data).
- **Testing:** A very common practical mistake — people `run` repeatedly thinking they're "restarting" the same thing.
- **Follow-up:** "What happens if you `run` the same `--name` twice?" → Error: name already in use — you'd need to remove the first or pick a new name.

**4. Why does `docker rm` sometimes refuse to delete something?**
- **Simple:** It won't delete a container that's still running — you have to stop it first, or force it.
- **Deeper:** This is a built-in safety check against accidental data loss. `rm -f` bypasses it by stopping and removing in one step.
- **Testing:** Whether you've actually hit this error live, not just read about it.
- **Follow-up:** "When would you actually want `-f`?" → Cleanup scripts, CI pipelines, disposable test containers — anywhere accidental running-state doesn't matter.

**5. What does `-p host:container` do, and why is it needed?**
- **Simple:** Containers are sealed off by default. `-p` opens a doorway, connecting a port on your real computer to a port inside the container.
- **Deeper:** Containers run in an isolated network namespace. `-p <host>:<container>` sets up a port forward on the Docker network bridge, so traffic to the host port reaches the container's internal port. Without it, even code running on the same physical machine can't reach the service inside.
- **Testing:** Whether you understand container networking isolation, a frequent real debugging scenario.
- **Follow-up:** "Can host and container ports differ?" → Yes — `-p 27018:27017` is valid; only the container-side number has to match what the app inside actually listens on.

**6. What's the difference between `docker images` and `docker ps`?**
- **Simple:** `images` lists the saved recipes. `ps` lists the running (or all, with `-a`) copies made from them.
- **Deeper:** Images are static, cached build artifacts on disk. Containers are runtime instances. Deleting all containers made from an image does NOT delete the image itself — it stays cached until explicitly removed with `rmi`.
- **Testing:** Whether you conflate images and containers, a very common beginner mix-up.
- **Follow-up:** "How do you free up disk space from old images?" → `docker image prune`, or `docker system prune` for a broader cleanup (not covered hands-on yet — flagged for Day 43's Level 2).

**7. `venv` vs Docker — when do you actually need Docker instead of just a virtual environment?**
- **Simple:** `venv` only isolates Python packages. Docker isolates everything — any software, any OS-level dependency.
- **Deeper:** If the issue is "different versions of a Python library," `venv` is sufficient. If the dependency isn't Python at all (a database, a specific OS library, a different language's runtime), or you need guaranteed-identical behavior across machines/deployments, Docker is the right tool.
- **Testing:** Whether you can scope a tool to the actual problem, not just default to "use Docker for everything."
- **Follow-up:** "Could you theoretically install MongoDB directly instead of via Docker?" → Yes, but then it's permanently installed on the host, version-pinning is manual, and it's not portable across machines the way a pinned image (`mongo:7`) is.
