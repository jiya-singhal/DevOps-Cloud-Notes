## INTRODUCTION TO DOCKER

Docker is a platform used to create, run, and manage containers.

A container is a lightweight environment that has everything needed to run an application (code + dependencies + tools).

Containers are similar to Virtual Machines, but much smaller and faster.

---

## DOCKER ARCHITECTURE

1. Docker Client

   * The tool where you type commands (docker run, docker ps, etc.)

2. Docker Daemon (dockerd)

   * The background service that actually manages images and containers.
   * Client sends commands → daemon executes them.

Both communicate using REST APIs.

---

## DOCKER IMAGES AND CONTAINERS

Image

* A read-only template used to create containers
* Example: ubuntu image, nginx image
* Stored locally or pulled from remote (Docker Hub)

Container

* A running instance created from an image
* Lightweight and isolated

Basic idea:
Image = blueprint
Container = actual running instance

---

## LOCAL VS REMOTE IMAGES

Local Images

* Stored on your machine
* Listed using: docker images

Remote Images

* Stored on Docker Hub or private registries
* Pulled using: docker pull <image>

---

## BASIC DOCKER COMMANDS

docker run

* Creates and starts a new container
* Example: docker run -it ubuntu bash
* -it gives an interactive terminal inside the container

docker ps

* Shows running containers
* docker ps -a shows all containers including stopped ones

docker stop <id>

* Gracefully stops a running container

docker kill <id>

* Forcefully stops a container

docker container prune

* Removes all stopped containers

docker images

* Lists all images on the system

docker rmi <image_id>

* Deletes an image

docker pull nginx

* Downloads an image from Docker Hub

docker commit

* Create a new image from an existing container
* docker commit -m "msg" <container_id> <new_image_name>

docker push

* Uploads an image to Docker Hub

docker login

* Log into Docker Hub to push or pull private images

docker exec

* Run commands inside a running container
* Example: docker exec -it <id> bash

docker inspect

* Shows detailed JSON information about a container or image

docker info

* Shows system-wide Docker details

---

## WHAT’S INSIDE A CONTAINER

Inside a container you can explore:

* Filesystem
* Processes
* Environment variables
* Installed packages

It looks similar to a normal Linux system, but isolated from host.

Key difference:
A container shares the host OS kernel, but has its own filesystem + processes.

---

## CONTAINERS VS VIRTUAL MACHINES

Containers

* Lightweight
* Share host OS kernel
* Start in milliseconds
* Smaller size
* Ideal for microservices

Virtual Machines

* Heavy
* Have their own full OS
* Slow startup
* Larger size
* Used where full OS isolation is required

---

## DOCKER INSTALLATION (SHORT NOTES)

On Linux

* Install using apt or yum
* apt-get install docker.io (Ubuntu)

On Windows/Mac

* Install Docker Desktop

On AWS

* Can install Docker on EC2
* Or use AWS Cloud Shell which already has Docker

---

## DOCKER HUB

Docker Hub

* Online repository for images
* Public + private repositories
* Used to store, share, and download images

Commands
docker pull <image>
docker push <username>/<image>
docker login

---

## SIMPLE DOCKER WORKFLOW

1. Pull an image
   docker pull ubuntu

2. Run a container
   docker run -it ubuntu bash

3. Make changes inside container
   (install tools, modify files)

4. Commit changes into a new image
   docker commit <container_id> myimage:v1

5. Push image to Docker Hub
   docker push <username>/myimage:v1

---

## CLASS SUMMARY

* Docker provides containers which are fast and lightweight.
* Docker architecture includes client + daemon.
* Images are templates; containers are running instances.
* Docker Hub is a cloud storage for images.
* Key commands: run, ps, pull, images, exec, commit, push, inspect.
* Containers share OS kernel → faster than VMs.
