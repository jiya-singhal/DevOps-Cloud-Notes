## DOCKERFILE BASICS

A Dockerfile is a simple text file with instructions to build a Docker image.

Image is created step-by-step using these instructions.
Each instruction creates a new layer.

A Dockerfile is like a recipe to prepare the final image.

---

## COMMON DOCKERFILE COMMANDS

FROM

* Defines base image (starting point)
* Every Dockerfile must start with FROM
* Examples:
  FROM ubuntu:22.04
  FROM python:3.12-slim

Best practice: use lightweight base images.

RUN

* Executes commands while building the image
* Used to install software or configure system
* Example:
  RUN apt-get update && apt-get install -y curl

Best practice: combine related commands to reduce layers.

CMD

* Default command to run when container starts
* Can be overridden when running container
* Only one CMD is considered (last one)

ENTRYPOINT

* Fixed main command that always runs
* CMD usually provides default arguments
* ENTRYPOINT is preferred for defining main application

COPY

* Copies files from host to image
* Use COPY for normal local file copying

ADD

* Similar to COPY but supports remote URLs and auto-extracts tar files
* Should be used only when these extra features are needed

ENV

* Sets environment variables inside image
* Example:
  ENV PORT=8080

---

## CREATING IMAGES AND CONTAINERS

Build image:
docker build -t <image_name> .

Run container:
docker run <image_name>

Example:
docker build -t myapp .
docker run myapp

---

## REAL-WORLD EXAMPLE (Tomcat / Flask style)

Typical steps for real application:

1. Choose base image
2. Set working directory
3. Copy dependency file
4. Install dependencies
5. Copy source code
6. Expose port
7. Set ENTRYPOINT and CMD

Image can then be started and accessed using exposed port.

---

## COPY-ON-WRITE (CoW) MECHANISM

Image layers are read-only.

When container modifies a file:

* Original file is copied from read-only layer
* Modification happens in writable layer

This saves space and improves performance.

---

## IMAGE LAYERS (IMPORTANT)

Each instruction (FROM, RUN, COPY, etc.) creates a new layer.

Layers are cached.
If a layer does not change, Docker reuses the cache.
This speeds up image builds.

Good practice:
Place frequently-changing lines (COPY . .) at the bottom.
Place dependency files (requirements.txt, pom.xml) before code for caching.

---

## JDK VS JRE (IN DOCKER)

JRE

* Only runs Java applications
* No compilers or dev tools
* Smaller image

JDK

* Includes compiler, tools, debugging
* Needed at build time
* Larger image

Best practice:
Use JDK during build stage
Use JRE or smaller runtime in final image
(usually done via multi-stage builds)

---

## MULTI-STAGE DOCKER BUILDS

Used to keep final image small.

Idea:

1. First stage builds the app (uses JDK, tools)
2. Final stage only copies the built output (uses JRE or minimal image)

Only final stage goes to production.

Helps reduce size, improves security, and performance.

---

## DOCKER IMAGE CACHING

Docker caches each layer.
If instructions remain same, Docker does not rebuild that layer.

Order matters:

* Stable instructions (install dependencies) should be higher
* Frequently changing parts (copying full source code) should be lower
  This reduces rebuild time.

---

## DOCKERFILE BEST PRACTICES

1. Use minimal base images (alpine, slim)
2. Combine RUN commands to reduce layers
3. Use multi-stage builds
4. Clean caches inside RUN
5. Do not hardcode secrets
6. Use non-root user inside container
7. Pin exact image versions
8. Keep Dockerfile instructions in logical order
9. Expose only necessary ports
10. Use HEALTHCHECK for reliability

---

## DOCKER NETWORKING (BRIEF MENTION)

Container networking allows containers to talk to each other.
Types include: bridge, host, overlay.
More details covered in later class.

---

## DOCKER INSPECTION AND ANALYSIS

View image layers:
docker history <image>

Inspect metadata:
docker inspect <image>

Scan for vulnerabilities:
docker scan <image>
or
trivy image <image>

---

## CLASS SUMMARY

* Dockerfile commands and real usage
* Creating images and running containers
* Understanding layers and caching
* Copy-on-write filesystem
* JDK vs JRE for image building
* Multi-stage builds and optimization
* Best practices for clean, secure, lightweight images
* Basic networking intro
* Inspection and image analysis tools
