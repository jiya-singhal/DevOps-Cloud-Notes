## Docker Networking – Handwritten Notes

### Why Networking is Important

* Containers need networking to:

  * Talk to other containers
  * Talk to the host machine
  * Access the internet
* Very important for:

  * Microservices
  * Distributed systems
  * Kubernetes

---

## Basic Networking Concepts (Foundation)

### IP Address

* A unique address given to every device in a network
* Helps identify **who is sending** and **who is receiving** data
* Example: `192.168.1.10`

---

### Ethernet Card (NIC – Network Interface Card)

* Physical hardware that connects a machine to a network
* Every system has a NIC (laptop, server, VM, container traffic ultimately uses it)

**Functions**

* Converts data into electrical signals
* Adds MAC address to data
* Sends and receives network frames

**Types**

* Wired NIC (Ethernet / RJ45)
* Wireless NIC (Wi-Fi)

---

### Ethernet Cable

* Physical cable connecting computers, switches, routers

**Common Types**

* Cat5e → 1 Gbps
* Cat6 → 10 Gbps (short distance)
* Cat6a → 10 Gbps (longer)
* Cat7/8 → Datacenters

---

### Network Switch

* Layer-2 device (works on MAC addresses)
* Connects devices in the same LAN

**What it does**

* Maintains MAC address table
* Sends data only to the correct port
* Reduces collisions (unlike hubs)

**Flow**

1. Frame arrives at switch
2. Switch reads destination MAC
3. Finds port from MAC table
4. Sends frame only to that port

---

### ARP Protocol (Address Resolution Protocol)

* Converts **IP address → MAC address**
* Needed because:

  * Applications use IP
  * NICs understand MAC

**ARP Process**

1. Broadcast: “Who has 192.168.1.20?”
2. Target replies with MAC
3. MAC stored in ARP cache

---

### Gateway

* Router that connects your network to outside networks
* Used when destination IP is **outside your subnet**

**Example**

* Your IP: `192.168.1.10/24`
* Destination: `8.8.8.8`
* Packet goes to gateway (e.g., `192.168.1.1`)

---

### NAT (Network Address Translation)

#### SNAT / MASQUERADE

* Changes **source IP**
* Used when private IP goes to internet
* Private IP → Public IP

#### DNAT

* Changes **destination IP**
* Used in:

  * Port forwarding
  * Load balancing

---

### Subnetting

* Dividing a large network into smaller ones

**Why**

* Better security
* Fewer broadcasts
* Efficient IP usage

**CIDR Examples**

* `/24` → 256 IPs
* `/26` → 64 IPs

---

## Docker Networking Concepts

### Network Namespace

* Each container gets its own:

  * Network interface
  * IP address
  * Routing table
  * Firewall rules
* Makes container feel like its own machine

---

### veth Pair (Virtual Ethernet Cable)

* Virtual cable with two ends:

  * One inside container (`eth0`)
  * One connected to Docker bridge

---

### Docker Bridge Network

* Default network created by Docker (`docker0`)
* Acts like a virtual switch

**Features**

* Containers get private IPs
* Containers can talk to each other
* NAT allows internet access

---

## Types of Docker Networks

### Bridge (default)

* Most common
* Used for local development
* Containers communicate internally

---

### Host Network

* Container shares host’s network
* No isolation
* Very fast
* Used for performance-critical apps

---

### None Network

* No networking at all
* Fully isolated container
* Used for secure or batch jobs

---

### Overlay Network

* Used when containers are on different hosts
* Used in:

  * Docker Swarm
  * Kubernetes
* Uses VXLAN internally

---

### Macvlan Network

* Container gets:

  * Own MAC address
  * Appears like a real machine on LAN
* Used when container must be visible on network

---

## Internet Access from Container

* Docker uses **iptables + NAT**
* Outgoing traffic:

  * Container → docker0 → host → internet
* Return traffic routed back correctly

---

## Port Mapping

* Used to expose container service

**Example**

* `-p 8080:80`
* Host port 8080 → Container port 80

---

## Docker Network Commands

* `docker network ls`
* `docker network inspect <name>`
* `docker inspect <container>`

---

## Default vs Custom Bridge Network

**Default Bridge**

* No DNS resolution by container name
* Less flexible

**Custom Bridge**

* Built-in DNS
* Containers talk using names
* Better isolation
* Recommended

---

## App + DB Example

* Create network
* Run app and DB in same network
* App connects using DB container name

---

## Key Takeaways

* Docker networking uses Linux features:

  * Network namespaces
  * veth pairs
  * bridges
  * iptables
* Containers feel isolated but run on host
* Networking knowledge is critical for:

  * Docker
  * Kubernetes
  * Real production systems
