## Kubernetes ReplicaSet – Handwritten Notes

### 1. Quick Revision (from previous classes)

• Kubernetes works on **desired state vs actual state**
• Control Plane decides
• Worker Nodes execute
• Everything is stored in **etcd**
• Controllers constantly watch and fix problems

---

## 2. What is a ReplicaSet?

• A **ReplicaSet (RS)** ensures a **fixed number of Pods** are always running

In simple words:
If you say **“I want 3 pods”**, ReplicaSet makes sure **3 pods always exist**

If:
• Pod crashes → new pod created
• Pod deleted → new pod created
• Extra pod exists → extra pod deleted

ReplicaSet never sleeps — it always checks and fixes

---

## 3. Why ReplicaSet is Needed

If you create a **normal Pod**:
• If it crashes → nothing restarts it
• No auto-healing

With **ReplicaSet**:
• Self-healing
• High availability
• Automatic recovery

---

## 4. ReplicaSet vs Normal Pod

Normal Pod
• Created once
• If deleted → gone
• No monitoring

ReplicaSet
• Manages pods
• Auto creates missing pods
• Maintains required count

---

## 5. ReplicaSet Uses Labels

ReplicaSet does NOT track pods by name
It tracks pods using **labels**

Example:

```
app: web
```

ReplicaSet says:
“Count all pods with label `app=web`”

---

## 6. ReplicaSet YAML Structure

Basic structure:

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx
```

Meaning:
• replicas → how many pods you want
• selector → how RS finds its pods
• template → blueprint of pod to create

---

## 7. What Happens After `kubectl apply`

### Step-by-step Flow

1. `kubectl` sends YAML to **API Server**
2. API Server:
   • authenticates
   • authorizes
   • validates
3. ReplicaSet object stored in **etcd**
4. ReplicaSet Controller sees new RS
5. Controller checks:
   • desired = 3
   • current = 0
6. Creates **3 Pod objects**
7. Pods stored in etcd (Pending state)

---

## 8. Scheduler’s Role

• Scheduler sees **Pending Pods**
• Chooses best nodes
• Assigns node names to pods
• Updates etcd

Pods move from:
Pending → Scheduled

---

## 9. Kubelet’s Role

• Kubelet on each node sees assigned pod
• Pulls image
• Starts container
• Reports status back

Pod becomes:
Running

---

## 10. Continuous Reconciliation Loop

ReplicaSet controller always does:

• Count current pods
• Compare with desired pods

If mismatch:
• Create pods
• Delete pods

This loop never stops

---

## 11. Failure Scenarios

### Pod Crash

• Pod dies
• ReplicaSet notices count reduced
• New pod created

### Manual Pod Deletion

• You delete 1 pod
• ReplicaSet creates 1 new pod

### Node Failure

• Node becomes NotReady
• Pods marked Unknown
• ReplicaSet recreates pods on healthy nodes

---

## 12. Deleting a ReplicaSet

If you run:

```
kubectl delete rs nginx-rs
```

What happens:
• ReplicaSet deleted
• All pods owned by it are also deleted

Reason:
Pods have **ownerReference** pointing to ReplicaSet

---

## 13. ReplicaSet and Controllers

ReplicaSet is a **controller**

Controllers:
• Do not run containers
• Only manage objects
• Work via API Server

ReplicaSet controller runs inside:
• kube-controller-manager

---

## 14. ReplicaSet and etcd (Storage View)

Stored objects:

• ReplicaSet object
• Pod objects
• Node info

Each pod entry has:
• ownerReference → ReplicaSet
• status → Running / Pending

---

## 15. ReplicaSet vs ReplicationController

ReplicationController:
• Old
• Less flexible selectors

ReplicaSet:
• New
• Supports advanced label selectors
• Used by Deployments

---

## 16. Important Exam / Interview Points

• ReplicaSet maintains **pod count**
• Uses **labels**, not pod names
• Runs via **controller loop**
• Pods are recreated automatically
• ReplicaSet itself never runs containers

---

## 17. Best Practices

• Do not create ReplicaSets directly in real projects
• Use **Deployments** instead
• ReplicaSet works silently behind Deployments

---

### One-Line Summary

ReplicaSet =
**Controller that ensures the required number of pods are always running using labels and continuous reconciliation**
