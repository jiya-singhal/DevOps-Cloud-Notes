Kubernetes – Deployments (Very Important Topic)

Pod (Basic Unit)

A Pod is the smallest thing Kubernetes runs.

• Runs one or more containers (mostly one)
• Containers inside a pod share
– Network
– Storage
• One Pod = One IP address

Important:

Pods are **not self-healing**

• If a pod crashes → it is gone
• If a node dies → its pods disappear
• Kubernetes will NOT recreate a standalone pod automatically

This is why we need controllers.

---

ReplicaSet (Keeps Pods Alive)

ReplicaSet says:
“I always want X number of pods running”

Example: replicas = 3

• If 1 pod dies → new pod is created
• If extra pods exist → extra pods are deleted

ReplicaSet ensures **count**, nothing else.

Limitations of ReplicaSet:

• No rolling updates
• No rollback
• No advanced deployment strategy

Because of this, engineers usually **do not create ReplicaSets directly**.

Deployments manage ReplicaSets for us.

---

Deployment (Most Important Controller)

Deployment is the recommended way to run applications.

Deployment can:

• Create and manage ReplicaSets
• Perform rolling updates
• Roll back to old versions
• Pause and resume updates
• Ensure self-healing
• Support Zero Downtime Upgrade (ZDU)

Deployment = Master controller

---

Basic Deployment YAML (Concept)

Deployment ensures:

• Required number of pods always run
• New ReplicaSet is created on update
• Old ReplicaSet is kept for rollback

Flow:

Deployment
→ ReplicaSet
→ Pods

---

What Happens Internally (Very Simple Flow)

When we run:

kubectl apply -f deployment.yaml

Step 1: API Server
• YAML goes to API Server
• Validation happens
• Object stored in etcd

Step 2: Deployment Controller
• Checks if ReplicaSet exists
• If not → creates new ReplicaSet

Step 3: ReplicaSet
• Ensures desired pod count
• Creates or deletes pods

Step 4: Scheduler
• Assigns pods to nodes

Step 5: Kubelet
• Pulls image
• Starts container

---

Rollout Strategies

1. RollingUpdate (Default)

• Old pods replaced slowly
• No downtime
• Controlled using:
– maxUnavailable
– maxSurge

2. Recreate

• All old pods deleted first
• New pods created later
• Causes downtime
• Used when two versions cannot run together

---

Why Deployments are Powerful

• Automatic recovery
• Version history maintained
• Easy rollback
• Declarative (desired state driven)

---

etcd (Where Everything Is Stored)

Kubernetes stores everything in etcd as key-value data.

Stored objects:

• Deployments
• ReplicaSets
• Pods
• Nodes
• Services

Deployment path example:

/registry/deployments/default/webapp

ReplicaSet path example:

/registry/replicasets/default/webapp-xxxx

Pod path example:

/registry/pods/default/webapp-xxxx

---

Revisions & Rollbacks

Each deployment update creates:

• New ReplicaSet
• New revision number

Revision stored as annotation.

Rollback means:

• Deployment points to older ReplicaSet
• Old version pods come back

---

Self-Healing Concept

Kubernetes continuously compares:

• Desired state (from etcd)
• Actual state (from nodes)

If mismatch → Kubernetes fixes it automatically.

Example:

Desired pods = 3
Running pods = 2

→ Kubernetes creates 1 new pod

This loop runs **all the time**.

---

Common Deployment Commands

Check rollout status
kubectl rollout status deployment <name>

View rollout history
kubectl rollout history deployment <name>

Rollback
kubectl rollout undo deployment <name>

Pause rollout
kubectl rollout pause deployment <name>

Resume rollout
kubectl rollout resume deployment <name>

Scale deployment
kubectl scale deployment <name> --replicas=10

Delete deployment
kubectl delete deployment <name>

Deletes:

• Pods
• ReplicaSets
• Deployment entry from etcd

---

Key Difference Summary

Pod
• Single unit
• No self-healing

ReplicaSet
• Maintains pod count
• No rollout / rollback

Deployment
• Manages ReplicaSets
• Supports updates
• Supports rollback
• Best practice for production
