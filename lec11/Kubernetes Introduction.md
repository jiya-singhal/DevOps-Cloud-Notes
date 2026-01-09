## Kubernetes – Introduction

Kubernetes (K8s) is an **open-source container orchestration platform**.

It is used to:

* Deploy containers
* Scale applications
* Restart failed containers (self-healing)
* Manage networking and storage
* Perform rolling updates with zero downtime

In simple words:
Kubernetes manages **containers at scale automatically**.

---

## Why Kubernetes is Needed

Problems without Kubernetes:

* Too many containers to manage manually
* No auto-scaling
* No self-healing
* Hard to manage networking
* Manual deployments cause downtime

Kubernetes solves this by:

* Automating everything
* Maintaining desired state
* Healing failures automatically

---

## Kubernetes Architecture (Big Picture)

Kubernetes follows **Master–Worker architecture**.

Two main parts:

* Control Plane (Master Node) → Brain
* Data Plane (Worker Nodes) → Muscles

---

## Control Plane (Master Node)

The control plane **decides and manages** everything.

### Components of Control Plane

#### 1. kube-apiserver

* Entry point of Kubernetes
* All commands go through API server
* Validates requests
* Talks to etcd
* Exposes REST APIs

Example:
kubectl create pod → API server

It is stateless and scalable.

---

#### 2. etcd

* Distributed key-value database
* Stores the **entire cluster state**

Stores:

* Pod definitions
* Node information
* Configurations
* Secrets
* Desired state

Important:
If etcd is lost → cluster is lost
So backups are very important.

---

#### 3. kube-scheduler

* Decides **which node** a pod should run on

Checks:

* CPU and memory
* Node health
* Taints and tolerations
* Affinity rules
* Existing workload

Selects the **best node** for each pod.

---

#### 4. kube-controller-manager

* Runs multiple controllers
* Continuously checks:
  desired state vs actual state

If mismatch → controller fixes it

Examples:

* Node controller
* ReplicaSet controller
* Deployment controller
* Job controller

Works in a continuous loop.

---

#### 5. cloud-controller-manager

(Only for cloud platforms like AWS, Azure, GCP)

Handles cloud-specific tasks:

* Load balancers
* Cloud routes
* Storage volumes
* Cloud node failures

---

## Data Plane (Worker Nodes)

Worker nodes **run the actual applications**.

---

### Components of Worker Node

#### 1. kubelet

* Agent running on each node
* Communicates with API server
* Ensures pods are running
* Restarts containers if needed
* Pulls images
* Mounts volumes

If pod crashes → kubelet restarts it.

---

#### 2. Container Runtime

* Actually runs containers
* Kubernetes talks via CRI

Examples:

* containerd
* CRI-O
* Docker (runtime deprecated, images still used)

Responsibilities:

* Pull images
* Start containers
* Manage namespaces and cgroups

---

#### 3. kube-proxy

* Manages networking
* Enables Service → Pod communication
* Load balances traffic across pods

Uses:

* iptables or ipvs

Keeps networking stable even when pods change.

---

## Pod Concept

Pod = **smallest deployable unit in Kubernetes**

A pod contains:

* One or more containers
* Shared network
* Shared storage
* Same IP address

Important:
Kubernetes does **not manage containers directly**, it manages **pods**.

---

## Containers vs Pods

Container:

* Single running process
* Created by Docker/container runtime

Pod:

* Wrapper around container(s)
* Managed by Kubernetes
* Provides networking and storage

One pod can have:

* One container (most common)
* Multiple containers (sidecar pattern)

---

## Pod Creation Flow (Very Important)

When you run:
kubectl apply -f pod.yaml

Flow:

1. kubectl sends request to API server
2. API server validates request
3. Desired state stored in etcd
4. Scheduler selects a node
5. kubelet on that node creates the pod
6. Container runtime pulls image
7. CNI sets up networking
8. kube-proxy updates routing
9. Controllers keep monitoring health

---

## Kubernetes Networking (Intro)

Basic ideas:

* Each pod gets a unique IP
* Pods can talk to each other
* Services provide stable access
* Networking handled by CNI plugins

Examples:

* Calico
* Flannel
* Cilium
* AWS VPC CNI

---

## Basic kubectl Commands

kubectl get pods
kubectl describe pod <pod-name>
kubectl delete pod <pod-name>
kubectl apply -f file.yaml

kubectl communicates only with API server.

---

## Key Takeaways (Exam / Interview)

* Kubernetes orchestrates containers
* Control plane manages state
* Worker nodes run applications
* Pod is the smallest unit
* etcd is the source of truth
* kubelet ensures containers run
* Scheduler decides placement
* Self-healing is automatic
