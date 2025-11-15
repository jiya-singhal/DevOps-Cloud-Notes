## DOCKER IMAGES AND CONTAINERS 

Image

* Blueprint for a container.
* Contains OS files, libraries, dependencies, configs.
* Images are read-only and do not change.

Container

* A running instance created from an image.
* Has its own filesystem, processes, environment.
* Gets a small writable layer on top of the image.

Image = template
Container = running copy

---

## CREATING, STARTING, STOPPING CONTAINERS

Create + start
docker run <image>

Interactive terminal
docker run -it ubuntu bash

List running containers
docker ps

List all containers
docker ps -a

Stop container
docker stop <id>

Force stop
docker kill <id>

Remove container
docker rm <id>

---

## INSTALLING SOFTWARE INSIDE CONTAINERS

You can install tools (like Java) inside a container:

Example:
apt update
apt install openjdk-17-jdk -y

Any change made inside container stays in the container’s writable layer.

To save these changes as a new image, use docker commit.

---

## CREATING IMAGES FROM CONTAINERS (docker commit)

After modifying a container → create new image:

docker commit -m "added java" <container_id> myimage:v1

This captures the container’s current filesystem and turns it into a new image.

---

## PUSHING & PULLING IMAGES (DOCKER HUB)

Login
docker login

Pull image
docker pull <name>

Push image
docker push <username>/<image>

Docker Hub = online storage for images (public or private repos).

---

## INTERNAL STRUCTURE OF DOCKER IMAGES (LAYERS)

Docker images are made up of multiple layers stacked together.

Each Dockerfile instruction creates one layer:

* FROM → base layer
* RUN → layer
* COPY / ADD → layer
* CMD / ENTRYPOINT → metadata layer

Layers are read-only.

When a container runs, Docker adds:

* A writable container layer on top of the read-only image layers.

Copy-on-write mechanism:
If you modify a file, it copies the original file into the writable layer first.

---

## DOCKER STORAGE DRIVERS (overlay2)

On Linux, Docker uses overlay2 file system.

Image + layer data stored under:

/var/lib/docker/overlay2/

Inside overlay2:

* diff/ → actual filesystem changes
* lower/ → links to layers beneath
* work/ → temp area for overlay operations

Image metadata stored under:

/var/lib/docker/image/overlay2/

---

## DOCKER ROOT DIRECTORY

Main Docker data lives in:

/var/lib/docker/

Contains folders for:

* overlay2 (layers and container filesystems)
* containers (config + logs)
* images (metadata)
* volumes
* networks

Understanding this helps with debugging and disk cleanup.

---

## INSPECTING IMAGES + CONTAINERS

Inspect an image:
docker inspect <image>

Show only layers:
docker image inspect <image> --format='{{json .RootFS.Layers}}'

View image history (each layer):
docker history <image>

Inspect container:
docker inspect <container>

---

## BASIC DOCKER COMMANDS (RECAP + NEW ONES)

docker run
docker ps
docker stop
docker kill
docker rm
docker rmi
docker images
docker pull
docker push
docker login
docker exec
docker inspect
docker info
docker commit
docker container prune

---

## DOCKERFILE (INTRODUCTION)

A Dockerfile is a script with steps to build an image.

Each instruction = a new image layer.

Common instructions:

FROM

* Defines the base image
* Example: FROM ubuntu:22.04

RUN

* Executes commands inside image
* Example: RUN apt-get update

COPY

* Copies files/folders into image
* Example: COPY . /app

ADD

* Like COPY but can extract archives or download URLs

CMD

* Default command for container startup
* Can be overridden at runtime

ENTRYPOINT

* Command that always runs
* Cannot be overridden unless --entrypoint used

---

## BUILDING & RUNNING DOCKER IMAGES

Build
docker build -t myapp .

-t → tag for the image
.  → location of Dockerfile

Run
docker run myapp

---

## KEY DOCKER CONCEPTS

Container

* Running instance of an image.

Portability

* Docker images run the same on any system with Docker installed.

Layers

* Make images space-efficient.
* Shared layers are cached and reused across images.

---

## CMD VS ENTRYPOINT (DIFFERENCE)

CMD

* Provides default command/arguments.
* Can be overridden easily at runtime.

ENTRYPOINT

* Defines the main executable.
* Always runs.
* Not overridden by args (unless --entrypoint is used).

Example difference:

CMD ["echo", "hello"]
entrypoint ["echo"]

---

## DOCKER ARCHITECTURE (RECAP)

Docker Client

* Where we type commands.

Docker Daemon

* Runs in background.
* Builds images, manages containers.

Container Runtime

* Applies layers, manages container filesystem.

Host OS

* Kernel shared with containers.

Containers share kernel with host → reason why they’re lightweight compared to VMs.

---

## CONTAINERS VS VIRTUAL MACHINES (RECAP)

Containers

* Share host OS kernel
* Lightweight
* Start fast
* Smaller size
* Ideal for microservices

Virtual Machines

* Each VM has its own OS
* Heavy
* Slow startup
* Larger resource usage

---

## CLASS SUMMARY

* Created and ran containers
* Installed software inside containers
* Saved changes using docker commit
* Pushed and pulled images from Docker Hub
* Explored image layers and storage structure
* Discussed overlay2 and Docker root directory
* Basic inspection commands
* Introduction to Dockerfile
* Clear difference between CMD and ENTRYPOINT
* Compared containers with virtual machines
* Understood Docker’s architecture and kernel sharing model
