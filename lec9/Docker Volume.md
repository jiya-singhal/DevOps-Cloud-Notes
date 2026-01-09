## Docker Volumes – Class Notes

### Why Do We Need Docker Volumes?

Containers are **ephemeral**
→ they are temporary by nature

A container has:

* Read-only image layers
* One writable layer (temporary)

When a container:

* Stops
* Is removed

→ the writable layer is **deleted**
→ all data inside the container is **lost**

Examples of data that get lost without volumes:

* /var/lib/mysql
* /usr/share/nginx/html
* /app/logs

To **save data even after container stops**, we use:

* Volumes
* Bind mounts

Note:
If a container is only **stopped (not removed)**, data still exists.
But once removed → data is gone.

---

## What Is a Docker Volume?

A Docker volume is:

* A way to **store data outside the container**
* Data survives container deletion
* Managed separately from containers

Volumes solve the problem of:

* Containers being ephemeral
* Data loss

---

## Types of Docker Volumes

There are **4 types**:

1. Anonymous Volume
2. Named Volume
3. Bind Mount
4. tmpfs Volume

---

## 1. Anonymous Volume

Created:

* Automatically by Docker
* Docker gives it a random name

Stored at:

* /var/lib/docker/volumes/<random_id>/

Use case:

* Short-lived containers
* When you don’t care about volume name

Example:
docker run -v /data nginx

Here:

* /data becomes an anonymous volume

Important points:

* Name is random
* Hard to manage later
* Deleted when container is removed (with --rm)

---

## 2. Named Volume

Created:

* Explicitly by the user
* Given a meaningful name

Managed by:

* Docker

Exists:

* Even after container is removed

Best for:

* Databases
* Application data
* Production workloads

Example:
docker volume create mydata

Use with container:
docker run -v mydata:/var/lib/mysql mysql

Even if container is deleted:

* Data remains in mydata volume

---

## 3. Bind Mount

What it does:

* Maps a **host folder** directly into a container

Format:

* /host/path:/container/path

Example:
docker run -v /home/user/app:/app nginx

Behavior:

* Changes on host → instantly visible in container
* Changes in container → visible on host

Best for:

* Local development
* Logs
* Config files
* Certificates and secrets

Characteristics:

* Uses host filesystem
* Requires absolute path
* Not ideal for production
* Host controls permissions

---

## 4. tmpfs Volume

Stored:

* In RAM only
* Not on disk

Behavior:

* Extremely fast
* Data disappears when container stops

Used for:

* Sensitive data
* Cache
* Temporary files

Example:
docker run --tmpfs /cache nginx

Important:

* No persistence
* More secure
* RAM based storage

---

## Comparison of Volume Types

Anonymous Volume

* Persistent: Yes
* Managed by Docker
* Temporary use

Named Volume

* Persistent: Yes
* Managed by Docker
* Best for databases

Bind Mount

* Stored on host
* Best for development
* Not managed by Docker

tmpfs

* Stored in RAM
* Not persistent
* Best for secure temporary data

---

## Where Docker Stores Volumes (Important)

Default location:

* /var/lib/docker/volumes/

Inside this:

* volume_name/_data/

Docker handles:

* Permissions
* Metadata
* Cleanup

Note:
Bind mounts are NOT stored here
They point directly to host paths.

---

## Docker Images vs Docker Volumes

Docker Image:

* Application blueprint
* Read-only
* Used to create containers

Docker Volume:

* Stores data
* Persistent
* Independent of container lifecycle

Images = code
Volumes = data

---

## Practical Example: MySQL with Volume

Without volume:

* Database data lost on container removal

With named volume:

* Data persists

Example flow:

* Create volume
* Attach to MySQL container
* Remove container
* Recreate container using same volume
  → Data still exists

---

## Docker Volume Commands

List volumes:
docker volume ls

Inspect volume:
docker volume inspect <name>

Create volume:
docker volume create <name>

Remove volume:
docker volume rm <name>

---

## Best Practices for Volumes

Use:

* Named volumes for databases
* Bind mounts for development
* tmpfs for sensitive or temporary data

Avoid:

* Storing important data inside containers
* Using bind mounts in production unless needed

---

## Why Volumes Matter for Kubernetes

Containers in Kubernetes are also:

* Ephemeral

Persistent storage is handled via:

* Volumes
* Persistent Volume Claims (PVCs)

Understanding Docker volumes helps in:

* Kubernetes storage concepts
* Stateful applications

---

## Class Summary

* Containers are temporary
* Data inside containers can be lost
* Volumes provide persistence
* Four types of volumes exist
* Choose volume type based on use case
* Volumes are essential for real-world apps
