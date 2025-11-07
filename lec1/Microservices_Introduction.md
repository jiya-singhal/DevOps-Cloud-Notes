# Microservices – Overview

→ Microservices = breaking one big (monolithic) app into small, independent parts.
→ Each service does one job only – like Auth, Payments, Orders, etc.
→ They communicate using APIs.
→ Easier to scale, update, and deploy independently.

---

## Core Components

### 1. API Gateway

* Entry point for all requests.
* Routes requests to correct service.
* Handles authentication, rate limiting, caching, etc.
* Examples → AWS API Gateway, Kong, Nginx.

**Analogy: It’s like a receptionist who knows which department (microservice) handles what.**

---

### 2. Service Registry / Discovery

* Service Registry: Keeps record of all running services and their IPs.
* Service Discovery: Helps services find each other automatically.
* Examples → Consul, Eureka.

---

### 3. DNS & Load Balancer

* **DNS:** Converts domain name → IP address.
* Works in steps:
  Client → Root Server → TLD Server (.com) → Domain Server → Returns IP → Browser connects.

* Cloud tools: AWS Route 53, Cloudflare, Azure DNS.

**Analogy: Like saving your friend’s name in your phone instead of their number.**

* **Load Balancer:** Distributes incoming traffic evenly.
* Ensures high availability and no downtime even if one instance fails.
Types:
* L4 (Transport Layer): Works on TCP/UDP (e.g., AWS NLB, HAProxy) – faster, less smart.
* L7 (Application Layer): Works on HTTP/HTTPS (e.g., AWS ALB, Nginx, Traefik) – smarter, reads headers, cookies, URLs.
* Features:
    * Health checks
    * SSL termination
    * Sticky sessions
    * Auto-scaling integration

**Analogy: Like a traffic cop sending cars to different lanes to avoid jams.**

---

### 4. Database per Service

* Each service has its own database (not shared).
* Makes services independent.
* **SQL (Structured):** MySQL, PostgreSQL → good for transactions.
* **NoSQL (Flexible):** MongoDB, Cassandra → good for scalability.
* Can use both = Polyglot Persistence.

**Example: Payments service may use SQL for transactions; Logging service may use NoSQL for high volume data.**

---

## Communication Between Services

### A. Synchronous (Real-Time)

* One service calls another and waits for response.
* Request–response type (REST, gRPC).
* Good for: Real-time actions (e.g., login).
* Drawback: Tight coupling – if one service fails, others may wait.

**Example: “Hey Payment, did the transaction succeed?” → waits → gets response.**

### B. Asynchronous (Event-Based)

* Service sends a message → continues work without waiting.
* Another service picks it up later.
* Uses message queues or event streams.
* Good for: Notifications, background processing, billing, etc.
* Tools: Kafka, RabbitMQ, AWS SQS.
* Drawback: Slight delay, eventual consistency (data syncs later).
* Works well for high-volume systems.

**Example: “Hey Notification, send email when you can” → continues doing other work.**

---

### 5. Message Broker

* Manages asynchronous communication.
* Stores and delivers messages between services.
* Uses queues or pub/sub model.
* Ensures messages are not lost.
* Examples →Kafka, RabbitMQ, AWS SQS.

**Analogy: Like a post office holding your letter until the receiver is ready.**

---

### 6. Event Bus

* An event bus lets services publish and subscribe to events.
* “Publisher” service announces something happened (e.g., order created).
* “Subscriber” services listen and react (e.g., send confirmation email).
* Ensures eventual consistency between services.
* Examples: Kafka, NATS.

**Analogy: Like a group chat — anyone can post an event, and whoever cares reacts.**

---

### 7. Swagger / OpenAPI

* Tool for documenting APIs.
* Shows endpoints, inputs, outputs.
* Makes integration easy for developers.

---

## Configuration, Logs & Monitoring

### Externalized Config

* Store configs(like URLs, credentials) outside the app (not in code).
* So you can update them without redeploying the app.
* Example → Spring Cloud Config.

---

### Centralized Logs

* Containers lose logs after restart → use centralized logging.
* Common setup:
    * Fluentd / Logstash: collect logs
    * Elasticsearch / Loki: store logs
    * Kibana / Grafana: visualize logs
* Cloud tools: AWS CloudWatch, Dynatrace.
* Gives one place to see all logs and debug easily.

**Analogy: Like having all CCTV cameras feed into one big screen.**
---

### Monitoring & Tracing

* Monitors health, metrics, and errors.
* Monitoring: Collects performance and health metrics (CPU, memory, errors).
* Tracing: Follows a request’s path through all services (helps debug slow points).
* Tools → Prometheus (metrics), Grafana (dashboards), Jaeger (tracing).

**Analogy: Like a fitness tracker for your services.**

---

### Reporting & Analytics

* Collects and analyzes data from all services.
* Used for dashboards, business intelligence, etc.
* Tools → Redshift, Snowflake, ELK Stack.

---

## SQL vs NoSQL

| Feature  | SQL                           | NoSQL                      |
| -------- | ----------------------------- | -------------------------- |
| Schema   | Fixed                         | Dynamic                    |
| Scaling  | Vertical                      | Horizontal                 |
| Examples | MySQL, PostgreSQL             | MongoDB, Cassandra         |
| Best for | Structured data, transactions | Scalability, flexible data |

---

## 12-Factor App Principles (for Cloud Microservices)

1. Codebase: One codebase, many deploys.
2. Dependencies: Declare all dependencies clearly.
3. Config: Keep config in environment variables.
4. Backing Services: Treat DBs/queues as replaceable resources.
5. Separate build → release → run stages.
6. Processes: Processes should be stateless.
7. Port Binding: App should run on its own port.
8. Concurrency: Scale by adding more processes.
9. Disposability: Fast startup and graceful shutdown.
10. Dev/Prod Parity: Keep Dev, Test, and Prod similar.
11. Logs: Treat logs as continuous event streams.
12. Admin Processes: Run admin/maintenance tasks separately.