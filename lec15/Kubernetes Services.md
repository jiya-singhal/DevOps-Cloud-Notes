KUBERNETES SERVICES

Why Services are Needed

• Pods are ephemeral
– Pod IP changes when pod restarts
– Pods can be deleted and recreated
– Scaling adds/removes pods dynamically

• Because of this, we **cannot directly talk to pods**

• Kubernetes Service solves this problem by giving
– Stable IP
– Stable DNS name
– Load balancing across pods

---

What is a Kubernetes Service

• A Service is an abstraction layer over pods

• It provides
– One stable virtual IP (ClusterIP)
– One DNS name
– Load balancing
– Service discovery

• Internally handled by **kube-proxy**

---

How a Service Works Internally

• You create a service with a selector
selector:
app: backend

• Kubernetes finds all pods with this label

• kube-proxy creates routing rules using
– iptables (older)
– IPVS (faster, newer)

• Any request to Service IP
→ forwarded to one of the matching pods

---

Types of Kubernetes Services

There are mainly 3 service types:

1. ClusterIP (default)
2. NodePort
3. LoadBalancer

(External IP mentioned briefly, not detailed)

---

ClusterIP Service (Default)

• Used for **internal communication only**
• Accessible only inside the cluster

Use cases
• Microservice to microservice calls
• Backend services
• Databases
• Frontend → backend communication

Example flow
frontend → backend → database

ClusterIP YAML (basic idea)

• port
– Port exposed by service

• targetPort
– Container port inside pod

Access inside cluster
• [http://service-name](http://service-name)
• DNS format
service.namespace.svc.cluster.local

---

NodePort Service

• Exposes service on **every node’s IP**
• Uses a fixed port range
30000 – 32767

Traffic flow
External Client
→ NodeIP : NodePort
→ Service
→ Pod

Use cases
• Local testing
• Bare-metal clusters
• Debugging

NodePort example access
http://<NodeIP>:32080

Limitations
• Exposed on all nodes
• High port numbers
• Not secure for public production
• No advanced routing

---

LoadBalancer Service

• Used in **cloud environments**
• Automatically creates a cloud load balancer

Supported on
• AWS (ELB / ALB / NLB)
• GCP
• Azure

Traffic flow
Internet
→ Cloud Load Balancer
→ NodePort
→ Service
→ Pods

• Internally still uses NodePort

After creation
• Kubernetes assigns an EXTERNAL-IP
• Publicly accessible

Best for
• Production workloads
• Public internet access

---

Service Discovery & DNS

• Every service gets a DNS name

Format
service-name.namespace.svc.cluster.local

Example
redis.master.svc.cluster.local

• Pods use DNS instead of IPs

---

Other Important Concepts

Headless Service
• clusterIP: None
• No load balancing
• Used for
– StatefulSets
– Databases
– Direct pod discovery

Multi-Port Services
• One service can expose multiple ports
• Example
– HTTP
– HTTPS

Service Without Selector
• Used when endpoints are managed manually
• ExternalName service
• Redirects service name to external DNS

---

When to Use Which Service

• Internal microservices → ClusterIP
• Local / testing access → NodePort
• Production external access → LoadBalancer
• Databases / Stateful apps → Headless
• External DB access → ExternalName

---

Quick Revision

• Pods change → Services are stable
• Service = IP + DNS + Load balancing
• ClusterIP = internal
• NodePort = node level access
• LoadBalancer = cloud public access
