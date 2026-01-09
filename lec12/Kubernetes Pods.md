Below are **handwritten-style notes** for this class.
Simple language, clean spacing, easy to revise, and **no point from your material is skipped**.
You can directly write this in your notebook.

---

## Kubernetes Pods – Deep Dive (Handwritten Notes)

### 1. What is a Pod?

* Pod is the **smallest unit** that Kubernetes can create and manage
* A pod can have:

  * One container (most common)
  * Multiple containers (sidecar pattern)
* All containers in a pod:

  * Run on the **same node**
  * Start and stop **together**
  * Share **network, storage, and IPC**

Think of a pod as
“one logical application unit”

---

### 2. Pod vs Container

* Container:

  * Single process
  * Runs inside Docker / container runtime
* Pod:

  * Wrapper around one or more containers
  * Kubernetes manages **pods**, not containers directly

Kubernetes never talks to containers directly
It always works with **pods**

---

### 3. Why Multiple Containers in One Pod?

* Containers in a pod:

  * Share **localhost**
  * Share **same IP**
  * Share **volumes**
* Used when containers are tightly coupled

Example:

* Main app container
* Sidecar container (logging, monitoring, proxy)

---

### 4. Kubernetes Architecture (Quick Revision)

#### Control Plane (Brain)

* API Server
* etcd
* Scheduler
* Controller Manager
* Cloud Controller Manager

#### Data Plane (Workers)

* Kubelet
* Container Runtime
* Kube-proxy
* Pods

---

### 5. What is Stored in etcd?

* etcd is Kubernetes’ **database**
* Stores:

  * Pod definitions
  * Desired state
  * Actual state
  * Cluster metadata
* Pod data path example:

  * /registry/pods/<namespace>/<pod-name>

If etcd is lost → cluster state is lost

---

### 6. Pod Creation Flow (Step by Step)

1. User runs:

   * kubectl apply -f pod.yaml
2. Request goes to:

   * API Server
3. API Server:

   * Authenticates user
   * Authorizes request (RBAC)
4. Admission Controllers run:

   * Mutating (can change pod)
   * Validating (can reject pod)
5. Pod object saved in:

   * etcd
6. Scheduler:

   * Finds pod with no node
   * Chooses best node
   * Writes nodeName
7. Kubelet on that node:

   * Sees assigned pod
   * Pulls image
   * Creates containers
   * Sets up networking (CNI)
8. Pod starts running
9. Status updated back to API Server

---

### 7. Role of Key Components in Pod Creation

#### API Server

* Entry point for everything
* Validates and stores pod data

#### Scheduler

* Decides **where** pod should run
* Checks CPU, memory, taints, affinity

#### Kubelet

* Runs on worker node
* Actually **creates and manages containers**
* Updates pod status

#### Container Runtime

* Pulls images
* Starts containers
* Uses namespaces and cgroups

---

### 8. Namespaces in Kubernetes

Namespaces help organize and isolate resources.

Default namespaces:

* default → user workloads
* kube-system → system components
* kube-public → public info
* kube-node-lease → node heartbeats

You can create your own namespace:

* For teams
* For environments (dev, test, prod)

---

### 9. Why Namespaces are Important

* Logical isolation
* Resource control
* Better security
* Clean organization

Same pod name can exist
in different namespaces

---

### 10. Basic Pod Commands

* Create pod:

  * kubectl run nginx --image=nginx
* List pods:

  * kubectl get pods
* List pods in all namespaces:

  * kubectl get pods -A
* Describe pod:

  * kubectl describe pod <name>
* Delete pod:

  * kubectl delete pod <name>

---

### 11. Pod Networking (Intro Level)

* Each pod gets:

  * Unique IP
* Pods can talk:

  * Same node
  * Different nodes
* Networking handled by:

  * CNI plugins

Pods communicate using IP, not container IDs

---

### 12. Containers vs Pods (Important Difference)

* Containers:

  * Runtime concept
  * Managed by Docker / containerd
* Pods:

  * Kubernetes concept
  * Managed by Kubernetes

Kubernetes schedules **pods**, not containers

---

### 13. Pods are Ephemeral

* Pods are temporary
* If pod dies:

  * New pod is created
  * IP changes
* Never store important data inside pod

Use:

* Volumes
* Databases
* External storage

---

### 14. SQL vs NoSQL (etcd Context)

* etcd uses:

  * Key-value (NoSQL style)
* Reason:

  * Fast reads/writes
  * Distributed
  * Strong consistency

Not suitable for large relational queries
Perfect for cluster state

---

### 15. Advanced Topics Mentioned (Intro Only)

* RBAC:

  * Controls who can do what
* Service Mesh:

  * Traffic management
* Networking Plugins:

  * Calico, Flannel, Cilium

These will be covered later

---

### 16. Best Practices with Pods

* Don’t create raw pods in production
* Use:

  * Deployments
  * ReplicaSets
  * StatefulSets
* Always define:

  * Resource requests
  * Resource limits
* Use probes:

  * Readiness
  * Liveness

---

### 17. One-Line Summary

* Pod = smallest Kubernetes unit
* One or more containers
* Shared network and storage
* Managed fully by Kubernetes

