# INTRODUCTION TO DEVOPS

→ DevOps = **Development + Operations**
→ It’s a modern way of developing and delivering software **faster and more reliably**.
→ Goal: **Zero Downtime Upgrades (ZDU)** → update apps without stopping them.
→ Breaks barriers between developers and operations teams.

---

## WHAT IS DEVOPS?

→ A set of **practices + culture + tools** that help build, test, and deploy software quickly.
→ Focuses on **collaboration**, **automation**, and **continuous feedback**.

Without DevOps:

* Dev team writes code → sends to Ops → deployment fails → long bug cycles.

With DevOps:

* Code is pushed → automated CI/CD pipeline tests and deploys → faster, reliable release.

---

## WHY DO WE NEED DEVOPS?

1. **Faster Delivery:**
   → Reduces release time from months to days or hours.
   → Example: Weekly feature updates instead of quarterly.

2. **Better Collaboration:**
   → Dev and Ops share work and tools.
   → Example: Both use “Infrastructure as Code”.

3. **More Reliability:**
   → Automated testing + monitoring = fewer production errors.
   → Example: CI pipeline catches bugs before deployment.

4. **Automation:**
   → Builds, testing, and deployments are automated.
   → Tools: Jenkins, GitHub Actions.

5. **Scalability & Flexibility:**
   → Cloud-native apps scale automatically.
   → Example: Kubernetes scales containers as needed.

6. **Continuous Improvement:**
   → Monitoring + feedback help improve performance, security, and user experience.

---

## TRADITIONAL SOFTWARE LIFECYCLE (WATERFALL)

1. Requirement Analysis
2. Design Phase

   * HLD (High-Level Design) – 60%
   * LLD (Low-Level Design)
3. Coding Phase – 40%

   * Includes Unit Testing
4. Deployment

**Artifacts Produced:**
→ .zip, .exe, .jar, .war, etc.

**Build Tools:**
→ package.json, pom.xml, Dockerfile

---

## DEVOPS LIFECYCLE (CONTINUOUS)

DevOps is **not linear** – it’s a **continuous loop** with feedback from each stage.

### Main Phases:

1. **Plan:**
   → Gather requirements, plan tasks.
   → Tools: Jira, Trello, Confluence

2. **Code:**
   → Write code + unit tests.
   → Tools: Git, GitHub, GitLab

3. **Build:**
   → Compile and create deployable artifacts.
   → Tools: Maven, Gradle, npm, Dockerfile

4. **Test:**
   → Automated testing for quality.
   → Tools: Selenium, JUnit, pytest

5. **Release:**
   → Package and prepare for release.
   → Tools: Jenkins, GitLab CI, CircleCI

6. **Deploy:**
   → Deploy to staging or production.
   → Tools: Kubernetes, Docker, Ansible, Helm

7. **Operate:**
   → Manage infrastructure and ensure apps run smoothly.
   → Tools: AWS CloudWatch, Prometheus, ELK

8. **Monitor:**
   → Track performance, logs, and feedback.
   → Tools: Grafana, Nagios, Datadog

---

## SHIFT LEFT APPROACH

→ Idea: **Find and fix bugs early** in development (during design/testing).
→ Fixing later costs more time and money.
→ Involves early **performance**, **security**, and **testing**.

**Earlier Tools:**

* Performance → JMeter
* Security → SonarQube, BugSnills

---

## SECURITY TESTING TYPES

1. **SCA (Software Composition Analysis):**
   → Checks open-source libraries for known vulnerabilities.

2. **SAST (Static Application Security Testing):**
   → Scans source code for weaknesses before running.

3. **DAST (Dynamic Application Security Testing):**
   → Tests running applications for security issues.

**Example:**
→ In Java, using normal Statement can cause SQL Injection.
→ Use PreparedStatement to prevent it.

---

## AWS (CLOUD & INFRASTRUCTURE BASICS)

→ Virtual Machines = **EC2 Instances**
→ Run inside **VPC (Virtual Private Cloud)** with subnets.
→ **IGW (Internet Gateway):** connects VPC to the internet.
→ Security is applied in **multiple layers** (network, app, etc.)

**Architecture Example:**
DNS → Load Balancer (Public Subnet) → Auth Service → MS/MI Service
→ Message Broker (Kafka - async) → API/Auth

**Communication:** Can be synchronous or asynchronous.

---

## CI/CD – Continuous Integration / Continuous Delivery / Deployment

### 1. Continuous Integration (CI)

→ Integrate code changes frequently.
→ Automate build + test process.
→ Tools: GitHub, Nexus, JFrog Artifactory

**Steps:**

* Download code
* Install dependencies
* Lint & build
* Unit test
* Run SAST/SCA (security checks)
* Build Docker image
* Validate build

---

### 2. Continuous Delivery (CD)

→ Automatically deliver tested builds to **staging or pre-production** environments.

---

### 3. Continuous Deployment

→ Automatically deploy builds to **production** after testing.
→ Strategies:

* **Blue-Green Deployment:** two environments (one live, one standby).
* **Canary Release:** release to small user group first, then all users.

---

## ADDITIONAL NOTES

**High Cyclomatic Complexity:**
→ Means complex code → hard to test and maintain.
→ Measured by SonarQube.

**Defect Categories:**

* Critical Issues
* Blockers
* Code Smells

---

## DEVOPS PRINCIPLES (SUMMARY)

1. Collaboration
2. Automation
3. CI/CD
4. Measurement (track performance & errors)
5. Knowledge Sharing
6. Infrastructure as Code (Terraform, Ansible)
7. Monitoring & Feedback

---

## TOOLS IN DEVOPS (BY STAGE)

| Stage            | Tools                             | Purpose                      |
| ---------------- | --------------------------------- | ---------------------------- |
| Version Control  | Git, GitHub, GitLab               | Track code changes           |
| CI/CD            | Jenkins, GitHub Actions, CircleCI | Automate build & deploy      |
| Config Mgmt      | Ansible, Chef, Puppet             | Server setup automation      |
| Containerization | Docker                            | Package apps into containers |
| Orchestration    | Kubernetes                        | Manage containers at scale   |
| IaC              | Terraform, CloudFormation         | Provision cloud resources    |
| Monitoring       | Prometheus, Grafana, ELK          | System health tracking       |
| Collaboration    | Slack, MS Teams                   | Team alerts, communication   |
| Security         | SonarQube, Trivy, Snyk            | Integrate security checks    |

---

## CONCLUSION

→ DevOps = **Mindset + Methodology + Toolchain**
→ Combines people, process, and tools for:

* Faster delivery
* Reliable software
* Better collaboration
* Continuous improvement
  → Key idea: **Automate everything** and keep **continuous feedback** flowing.
