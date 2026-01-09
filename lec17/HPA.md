## Kubernetes Horizontal Pod Autoscaler (HPA)

### What is HPA

* HPA = Horizontal Pod Autoscaler
* It automatically **increases or decreases number of pods**
* Scaling is based on **CPU, memory, or custom metrics**
* Horizontal = more pods (not bigger pods)

Example idea

* Low traffic → fewer pods
* High traffic → more pods
* Traffic goes down → pods reduce automatically

---

### Why HPA is Needed

* Manual scaling is slow
* Traffic is unpredictable
* Saves cost + improves performance
* Used in real production systems

---

### HPA Architecture (Simple Flow)

User Load
↓
Service
↓
Pods (Deployment)
↓
Metrics Server
↓
HPA Controller
↓
Pods scale up or down

---

## Kubernetes Setup (From Scratch – Practical)

### EC2 Setup

* Create EC2 instance
* Open required ports in Security Group:

  * 6443 (API Server)
  * 10250, 10251, 10252
  * NodePort range: 30000–32767

---

### Install Required Components

* Container runtime: **containerd**
* Kubernetes tools:

  * kubeadm
  * kubelet
  * kubectl

---

### Create Kubernetes Cluster

* Use `kubeadm init`
* Configure kubeconfig
* Join worker nodes using `kubeadm join`

---

## Metrics Server (Very Important)

### Why Metrics Server is Mandatory

* HPA depends on live CPU/memory data
* Without it:

  * CPU shows 0%
  * HPA does not work

---

### Install Metrics Server

* Apply metrics server YAML
* Verify:

  * Metrics server pod running
  * `kubectl top nodes`
  * `kubectl top pods`

If metrics are visible → metrics server is working

---

## Creating Deployment for HPA

### Purpose

* Run a **CPU-intensive application**
* Used to test autoscaling

---

### Important Point: CPU Requests

* HPA calculates:

  * CPU Usage / CPU Request
* If `requests.cpu` is missing → HPA will not scale

---

### Deployment Key Points

* Starts with 1 replica
* CPU request defined
* CPU limit defined
* Uses sample HPA image

---

## Creating Service

### Why Service is Needed

* Pods are ephemeral
* Service gives:

  * Stable IP
  * Load balancing
* Used to send traffic to pods

---

## Creating HPA Resource

### What HPA Does Here

* Watches CPU usage
* Target CPU: 50%
* Min pods: 1
* Max pods: 10

---

### HPA Behavior

* If CPU > 50% → scale up
* If CPU < 50% → scale down
* Scaling is **gradual**, not instant

---

## Load Generation

### Why Load Generator

* To simulate real user traffic
* Increases CPU usage

---

### What Happens Internally

1. CPU crosses threshold
2. Metrics Server reports usage
3. HPA controller reacts
4. New pods are created

---

## Monitoring Scaling

### Useful Commands

* Check HPA status
* Watch deployment replicas
* View pod CPU usage
* Describe deployment to see events

---

## Scale Down Behavior

* Stop load
* HPA waits for cooldown period
* Pods reduce slowly
* Prevents frequent up/down scaling

---

## Common Issues & Fixes

### CPU shows 0%

* Cause: Metrics server missing
* Fix: Install metrics server

---

### No Scaling

* Cause: CPU request missing
* Fix: Add `requests.cpu`

---

### HPA Not Working

* Cause: Label mismatch
* Fix: Match service, deployment, HPA labels

---

## Key Takeaways

* Metrics Server is mandatory
* CPU requests are required
* HPA uses **average pod CPU**
* Scaling is automatic
* Used heavily in production

---

## Interview-Ready Line

“HPA automatically adjusts pod replicas based on CPU metrics collected by the Metrics Server to handle dynamic workloads.”

---

## Mentioned Advanced Topics (Upcoming)

* Memory-based HPA
* Custom metrics
* Prometheus Adapter
* HPA vs VPA
* HPA with KEDA
