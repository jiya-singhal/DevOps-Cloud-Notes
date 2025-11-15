## WHAT EXACTLY IS A CONTAINER?

A container is not a virtual machine.

A container is simply:

a normal Linux process

* an isolated filesystem
* isolation provided by Linux namespaces
* resource limits using cgroups.

Containers feel like separate machines, but they are not.

---

## WHY CONTAINERS ARE EPHEMERAL

A container has:

1. Read-only image layers
2. A temporary writable layer

When the container stops:

* its main process ends
* writable layer is removed
* network namespace is deleted
* cgroup limits removed

So containers are stateless and temporary unless data is stored in a volume.

---

## CONTAINER = JUST A PROCESS ON THE HOST

When you run:

docker run nginx

Docker does not create a VM.
It simply starts an nginx process on the host with isolation.

On host process list, you can still see container processes:

ps -ef
shows nginx running under containerd-shim.

Container = process + isolation.

---

## CONTAINER ISOLATION IS PSEUDO-ISOLATION

Containers share the host kernel.
This means:

* Not as secure as a VM
* Kernel vulnerabilities affect containers
* All container processes can be seen from host

Isolation type: process-level, not OS-level.

---

## PROCESS TREE (HOW CONTAINERS START)

docker (client)
→ containerd
→ containerd-shim
→ runc
→ main container process (PID 1 inside container)

Purpose of each:

containerd

* manages containers, images, networking

containerd-shim

* parent of container process
* allows containerd to restart without killing containers

runc

* sets up namespaces
* applies cgroups
* launches the process inside isolated environment

---

## WHY KILLING PID 1 STOPS THE CONTAINER

Inside container, the main application becomes PID 1.

If PID 1 exits, container stops.

Examples:

nginx dies → container stops
python app.py exits → container stops
sleep 5 finishes → container stops

Container = lifecycle of its PID 1.

---

## WHY CONTAINERS LOOK LIKE SEPARATE MACHINES

Because Linux namespaces give “fake” isolated worlds.

Namespaces provide isolation at different levels.

---

## LINUX NAMESPACES (MOST IMPORTANT TOPIC)

Namespaces change what the container can see.

Each namespace isolates a specific part of the system.

---

### 1. PID Namespace

* Isolates process IDs
* Container sees its own PID tree
* Inside container, app becomes PID 1
* But on host, it’s just another process

---

### 2. Network Namespace

Each container gets:

* its own network stack
* its own IP
* its own routing table
* its own virtual ethernet pair (veth)

Host creates a veth0 <-> eth0 binding.

---

### 3. Mount / Filesystem Namespace

Container sees its own root filesystem.

This is created using:

* OverlayFS layers
* chroot
* bind mounts

Host filesystem stays separate.

---

### 4. UTS Namespace

* Isolates hostname
* Container can have its own hostname
* Host keeps its own name

---

### 5. IPC Namespace

* Isolates inter-process communication
* Semaphores
* Shared memory
* Message queues

---

### 6. USER Namespace

* Allows root inside container
* But mapped to non-root UID on host
* Increases security

---

Overall effect:
Container “feels” like its own machine, but it is only a process with a limited view.

---

## CGROUPS (CONTROL GROUPS)

Namespaces = isolation
Cgroups = resource limits

Cgroups restrict:

* CPU usage
* Memory
* Disk I/O
* Number of processes (PIDs)
* Network limits (depending on version)

Example limits:

256MB memory
0.5 CPU
Limited PIDs

If container exceeds limit → kernel kills container process.

---

## HOW DOCKER CREATES A CONTAINER (STEP-BY-STEP)

1. Pull image
2. Unpack layers into OverlayFS
3. Create cgroups for resource limits
4. Create namespaces (PID, NET, MOUNT, USER, IPC, UTS)
5. chroot into overlay filesystem
6. Start process using runc

All this happens when you run:

docker run nginx

---

## WHY CONTAINERS START FAST

Compared to virtual machines:

No BIOS
No bootloader
No full OS
No kernel boot
No hardware emulation

Container start = launching one process with isolation.

Start time ~50ms.

---

## WHY CONTAINERS ARE LIGHTWEIGHT

Containers share the host:

* kernel
* drivers
* CPU scheduler
* memory management

Only filesystem + process environment is isolated.

This is why images are small and containers run many times faster.

---

## VM VS CONTAINER (INTERNAL DIFFERENCE)

VIRTUAL MACHINE
Hardware
→ Hypervisor
→ Guest OS
→ App
Has separate kernel.

CONTAINER
Hardware
→ Host Linux Kernel
→ App (isolated via namespaces + cgroups)
No separate kernel.

---

## CONTAINERS ON DIFFERENT OPERATING SYSTEMS

Linux

* Native, uses real namespaces + cgroups

Windows

* Uses Windows containers (different kernel)
* Also supports Linux containers via a hidden Linux VM

Mac

* No Linux kernel
* Runs Docker inside a small Linux VM (Lima / HyperKit / Colima)

---

## CONTAINERS ARE EPHEMERAL (REPEATED KEY POINT)

Data disappears unless stored in:

* Volumes
* Bind mounts
* External DBs
* Cloud storage

This is why upcoming topics include:

* Docker volumes
* Container networking

---

## CLASS SUMMARY

* Container = process + isolation
* Works using namespaces + cgroups
* Not a VM
* Root in container ≠ root on host
* Containers share host kernel
* Fast startup due to no OS boot
* Ephemeral writable layer
* Networking and volumes coming next class
