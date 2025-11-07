# CONTINUOUS DELIVERY (CD)

→ CD (Continuous Delivery) begins **where CI (Continuous Integration)** ends.
→ Goal: **Automate deployment** of tested code into multiple environments for faster, reliable releases.

---

## FLOW OF ENVIRONMENTS

SIT → UAT → Pre-Prod → Production

**SIT (System Integration Testing)**

* Checks if all integrated modules work well together.
* Example: Login + Payment + Notification flow.

**UAT (User Acceptance Testing)**

* Final testing by end-users before release.
* Ensures system behaves as expected.

**Pre-Prod (Staging)**

* Exact copy of production environment.
* Final verification stage before going live.

**Production (Prod)**

* Actual live system used by customers.

---

## TESTING IN CD

| Type                    | Purpose                               | Example                 |
| ----------------------- | ------------------------------------- | ----------------------- |
| Functional Testing      | Tests main business logic             | Login, Signup, Checkout |
| Performance Testing     | Checks system load-handling capacity  | JMeter                  |
| Security Testing (DAST) | Tests running apps for security risks | OWASP ZAP, Burp Suite   |

---

## CONTINUOUS DEPLOYMENT

→ Extends Continuous Delivery.
→ After successful testing, deployment happens **automatically** to production (no manual approval).
→ Ensures **Zero Downtime Deployment (ZDD)** using **Blue-Green Deployment**.

---

## TESTING & QUALITY METRICS

**Code Coverage (~80% recommended)**

* Shows how much of your code is tested.
* Types:

  * Line Coverage
  * Function Coverage
  * Branch Coverage

**Mocking:**

* Used to isolate units of code during testing.
* Example: Fake database object during unit tests.

**Cyclomatic Complexity:**

* Measures how complex your code is (no. of paths).
* Higher value → harder to maintain.
* Tool: SonarQube.

**Defect Categories:**

* **Critical:** Breaks major features.
* **Blockers:** Stop the process.
* **Code Smells:** Poor coding style but not an actual bug.

---

# LINUX BASICS

**Created by:** Linus Torvalds (1991)
**Type:** Open-source Operating System
**Used in:** ~90% of production environments
**Distributions:** 200+ (Ubuntu, Red Hat, CentOS, etc.)

---

## LINUX ARCHITECTURE

**Kernel:**

* Core part of OS → bridge between hardware & software.
* Manages CPU, memory, devices, and processes.
* Handles scheduling, networking, and security.
* Microkernel architecture = minimal kernel + external drivers.

**Shell:**

* Command-line interpreter that lets users talk to the kernel.
* Common shells: bash (default), zsh, sh, fish.

**Why Learn Shell Commands?**

* Faster automation
* Basis for DevOps scripting
* Easier debugging & system management

---

## BASIC LINUX COMMANDS

| Command | Purpose                        | Example             |
| ------- | ------------------------------ | ------------------- |
| pwd     | Show current working directory | pwd                 |
| cd      | Change directory               | cd /home/user       |
| mkdir   | Create new directory           | mkdir projects      |
| rmdir   | Remove empty directory         | rmdir old_folder    |
| touch   | Create empty file              | touch file.txt      |
| cat     | Show file contents             | cat notes.txt       |
| echo    | Print or write text            | echo "Hello Linux!" |
| history | Show previous commands         | history             |

**Lab Practice:**

1. Create a directory `starting_point`.
2. Go inside it.
3. Create a file `secret_lair.txt`.
4. Show full path using `pwd`.
5. View command history.

---

## FILE MANAGEMENT COMMANDS

| Command | Purpose                 | Example                       |
| ------- | ----------------------- | ----------------------------- |
| ls      | List files and folders  | ls -l                         |
| cp      | Copy files/folders      | cp file1.txt /backup/         |
| mv      | Move or rename          | mv old.txt new.txt            |
| rm      | Delete files or folders | rm -r temp/                   |
| find    | Search files            | find /home/user -name "*.log" |
| du      | Show disk usage         | du -sh /var/log/              |
| df      | Show free space         | df -h                         |

**Lab Practice:**

* Delete `delete.txt` and `delete_directory`.
* Find all `.txt` files under `/home/user`.
* Display disk usage of `/var/log/`.

---

## PROCESS MANAGEMENT COMMANDS

**Process = Running Program**

| Command | Purpose                 | Example   |
| ------- | ----------------------- | --------- |
| ps      | Show current processes  | ps -ef    |
| top     | Live process view       | top       |
| htop    | Advanced process viewer | htop      |
| kill    | End process using PID   | kill 1234 |
| jobs    | List background jobs    | jobs      |
| bg      | Move job to background  | bg %1     |
| fg      | Bring job to foreground | fg %1     |

**Lab Practice:**

* List running processes.
* Find PID of `sshd`.
* Run `sleep 60 &` and bring to foreground.
* Kill a process using PID.

---

## PERMISSIONS & OWNERSHIP (EXTRA)

| Command | Purpose            | Example                  |
| ------- | ------------------ | ------------------------ |
| chmod   | Change permissions | chmod 755 file.sh        |
| chown   | Change owner       | chown user:group file.sh |
| chgrp   | Change group       | chgrp dev file.txt       |

**Lab Challenge:**

* Find permission-restricted folder in `secret_lair`.
* Change ownership to `user`.
* Save answers in `/home/user/answer.txt`.

---

# AWS BASICS

**AWS (Amazon Web Services):** Cloud platform offering compute, storage, and networking.

---

## EC2 (Elastic Compute Cloud)

* Used to create **virtual machines** on AWS.
* **AMI (Amazon Machine Image):** Pre-built OS template (Ubuntu, Amazon Linux, Windows).
* You can also make **custom AMIs** with pre-installed software.

---

## VPC (Virtual Private Cloud)

* A **private virtual network** within AWS.
* Divided into **subnets** (Public & Private).
* Connected to internet through **IGW (Internet Gateway)**.
* Allows **layered security** at network and app level.

---

## IP TYPES IN AWS

| Type       | Description                                       |
| ---------- | ------------------------------------------------- |
| Public IP  | Accessible over the internet. Changes on restart. |
| Private IP | Used for internal AWS communication.              |
| Elastic IP | Static, permanent public IP (doesn’t change).     |

---

## AWS REGIONS & AVAILABILITY ZONES (AZ)

**Region:**

* Geographically separate area (like `us-east-1`, `ap-south-1`).
* Each region has multiple **Availability Zones**.

**Availability Zone (AZ):**

* Separate physical data centers within a region.
* Example:

  * Region: ap-south-1
  * AZs: ap-south-1a, ap-south-1b, ap-south-1c

**Best Practice:**
Use multiple AZs for high availability.

---

## SUBNETS IN AWS

**Subnet:** Smaller network inside a VPC.
Each subnet belongs to **one AZ**.
Divides the VPC into **public** and **private** zones.

**Subnet Tips:**

1. Use multiple AZs for redundancy.
2. Create public, private, and isolated tiers.
3. Use **NAT Gateway** for private subnet internet access.
4. Reserve enough IP range (CIDR).
5. Tag clearly (e.g., Env=Prod).
6. Use Route Tables & Security Groups to control access.

---

## EC2 LAUNCH SETTINGS (FIELDS)

1. **Name & Tags:** Label your instance (e.g., Name=WebServer).
2. **AMI:** Choose OS image (Amazon Linux, Ubuntu, Windows).
3. **Instance Type:** Choose hardware size (t2.micro, m5.large).
4. **Key Pair:** SSH login key for security.
5. **Network Settings:** Choose VPC, subnet, and security groups.
6. **Storage:** Choose disk type (EBS volumes like gp2/gp3) and size.
7. **Advanced Options:**

   * IAM Role → grant permissions to instance.
   * Termination protection → avoid accidental deletion.
   * Monitoring → enable CloudWatch.
   * User Data → script to auto-configure instance.

---

**EC2 DEMO STEPS (Summary):**

1. Choose AMI → Choose Instance Type
2. Configure Network & Security Group
3. Add Storage → Add Tags
4. Review → Launch Instance
5. Connect using Key Pair via SSH

---

## SUMMARY

* **CD** automates deployment and testing → reliable, fast delivery.
* **Linux** provides strong base for DevOps and automation.
* **AWS** (EC2 + VPC) provides scalable cloud infrastructure for deployments.
