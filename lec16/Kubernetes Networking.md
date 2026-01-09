KUBERNETES NETWORKING – INTERNALS (DEEP DIVE)

---

WHY NETWORKING IS IMPORTANT IN KUBERNETES

• Pods are temporary (ephemeral)
• Pod IPs change when pods restart
• Pods run on different nodes
• Containers must still communicate reliably

Because of this, Kubernetes defines **clear networking rules and models**.

---

KUBERNETES NETWORKING BASIC RULES

Kubernetes enforces 3 important rules:

• All pods can talk to all other pods
• All nodes can talk to all pods
• A pod IP is the same inside and outside the pod

No NAT is used for pod-to-pod communication.

---

POD AND CONTAINER BASICS (RECAP)

Pod
• Smallest unit in Kubernetes
• Contains one or more containers

Containers inside a pod:
• Share the same network
• Share the same IP address
• Share localhost
• Can talk using localhost:port

---

CONTAINER TO CONTAINER COMMUNICATION (INSIDE POD)

How it works:

• Linux creates a **network namespace**
• All containers in a pod share this namespace

Result:

• Same IP
• Same ports
• Same loopback (127.0.0.1)

So containers talk using:

localhost

---

POD TO POD COMMUNICATION (SAME NODE)

Each pod has:

• Its own network namespace
• One virtual interface (eth0)

How traffic flows:

Pod eth0
→ virtual ethernet (veth pair)
→ Linux bridge (inside node)
→ veth of destination pod
→ destination pod namespace

ARP is used to find the correct pod.

---

POD TO POD COMMUNICATION (DIFFERENT NODES)

Each node gets a **Pod CIDR range**

Example:

Node 1 → 10.0.1.0/24
Node 2 → 10.0.2.0/24

Traffic flow:

• Pod sends packet
• Goes through veth and bridge
• Bridge cannot find pod locally
• Packet routed via node network
• Reaches destination node
• Delivered to destination pod via veth

How does Kubernetes know which node owns which IP range?
→ This is handled by **CNI plugins**

---

CNI (CONTAINER NETWORK INTERFACE)

CNI is responsible for pod networking.

Examples:

• AWS VPC CNI
• Calico
• Flannel
• Weave

AWS VPC CNI:

• Pods get real VPC IPs
• No overlay network
• Pods behave like normal EC2 resources

---

POD TO SERVICE COMMUNICATION (CLUSTER IP)

Problem:

• Pods restart
• Pod IPs change
• Cannot rely on pod IPs

Solution:

Kubernetes Service provides:

• Stable virtual IP (ClusterIP)
• Load balancing
• DNS name

---

HOW SERVICE TRAFFIC WORKS

Service traffic is handled by **kube-proxy**

Two implementations:

1. IPTABLES (older)
   • kube-proxy creates iptables rules
   • Traffic to Service IP is DNATed to a pod
   • Random pod is selected

2. IPVS (modern, faster)
   • Kernel-level load balancing
   • More scalable
   • Better for large clusters

---

ROLE OF KUBE-PROXY

• Runs on every node
• Watches services and endpoints
• Programs iptables or IPVS rules
• Handles service load balancing

---

DNS IN KUBERNETES (COREDNS)

Every service gets a DNS name:

service.namespace.svc.cluster.local

Example:

backend.default.svc.cluster.local

CoreDNS:

• Runs as a pod
• Provides service discovery
• Automatically updated

Pods use DNS instead of IPs.

---

INTERNET TO POD COMMUNICATION

Two directions:

1. EGRESS (Pod → Internet)

Problem:

• Internet only knows node IPs

Solution:

• Pod traffic is SNATed to node IP
• Node sends traffic to internet

2. INGRESS (Internet → Pod)

Two methods:

• Service type LoadBalancer (Layer 4)
• Ingress Controller (Layer 7)

---

LOADBALANCER VS INGRESS

LoadBalancer:

• Cloud-managed
• Layer 4 (TCP/UDP)
• Simple use cases

Ingress Controller:

• Layer 7 (HTTP/HTTPS)
• Path-based routing
• Host-based routing
• SSL termination

Examples:

NGINX
AWS ALB
Traefik
Istio Gateway

---

IMPORTANT SUMMARY TABLE (MENTAL MAP)

Container → Container
• Same pod
• Same namespace
• localhost

Pod → Pod (same node)
• veth → bridge → veth

Pod → Pod (different node)
• Routed using Pod CIDR

Pod → Service
• ClusterIP
• kube-proxy
• iptables / IPVS

DNS
• CoreDNS
• Service name resolution

Egress
• Pod → Node → Internet

Ingress
• LoadBalancer or Ingress Controller

---

KEY TAKEAWAY

• Containers are just Linux processes
• Namespaces provide isolation
• cgroups manage resources
• Kubernetes networking is rule-based
• Services and DNS make pods reachable
• kube-proxy is the backbone of service networking
