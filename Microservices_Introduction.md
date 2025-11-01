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

---

### 2. Service Registry / Discovery

* Keeps record of all running services and their IPs.
* Helps services find each other automatically.
* Examples → Consul, Eureka.

---

### 3. DNS & Load Balancer

* **DNS:** Converts domain name → IP address.
* **Load Balancer:** Distributes incoming traffic evenly.
* Increases availability and fault tolerance.
* **L4:** Works on TCP/UDP (NLB, HAProxy).
* **L7:** Works on HTTP/HTTPS (ALB, Nginx, Traefik).
* Supports SSL termination, sticky sessions, and health checks.

---

### 4. Database per Service

* Each service has its own database (not shared).
* Makes services independent.
* **SQL (Structured):** MySQL, PostgreSQL → good for transactions.
* **NoSQL (Flexible):** MongoDB, Cassandra → good for scalability.
* Can use both = Polyglot Persistence.

---

## Communication Between Services

### A. Synchronous (Real-Time)

* Request–response type (REST, gRPC).
* Fast and real-time.
* But tightly coupled → if one service fails, others may wait.

### B. Asynchronous (Event-Based)

* Services send messages or events (Kafka, RabbitMQ, AWS SQS).
* Decoupled and scalable.
* Works well for high-volume systems.
* Follows “eventual consistency”.

---

### 5. Message Broker

* Manages asynchronous communication.
* Uses queues or pub/sub model.
* Ensures messages are not lost.
* Examples → Kafka, RabbitMQ.

---

### 6. Event Bus

* Services publish and subscribe to events.
* Helps keep data in sync across services.

---

### 7. Swagger / OpenAPI

* Tool for documenting APIs.
* Shows endpoints, inputs, outputs.
* Makes integration easy for developers.

---

## Configuration, Logs & Monitoring

### Externalized Config

* Store configs outside the app (not in code).
* Easy to update without redeploying.
* Example → Spring Cloud Config.

---

### Centralized Logs

* Containers lose logs after restart → use centralized logging.
* Common setup → Fluentd → Elasticsearch → Kibana/Grafana.
* Gives one place to see all logs and debug easily.

---

### Monitoring & Tracing

* Monitors health, metrics, and errors.
* Tools → Prometheus (metrics), Grafana (dashboards), Jaeger (tracing).

---

### Reporting & Analytics

* Collects and analyzes data from all services.
* Tools → Redshift, Snowflake, ELK Stack.

---

## DNS (Quick Recap)

Client → Root Server → TLD Server (.com) → Domain Server → Returns IP → Browser connects.
Examples → AWS Route 53, Cloudflare.

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

1. One codebase, many deploys.
2. Declare all dependencies clearly.
3. Keep config in environment variables.
4. Treat DBs/queues as replaceable resources.
5. Separate build → release → run stages.
6. Processes should be stateless.
7. App should run on its own port.
8. Scale by adding more processes.
9. Fast startup and graceful shutdown.
10. Keep Dev, Test, and Prod similar.
11. Treat logs as continuous event streams.
12. Run admin/maintenance tasks separately.