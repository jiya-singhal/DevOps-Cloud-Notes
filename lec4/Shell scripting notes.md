# LINUX COMMANDS & SHELL SCRIPTING BASICS

---

## BASIC LINUX COMMANDS

| Command       | Purpose                                    | Example                  |
| ------------- | ------------------------------------------ | ------------------------ |
| pwd           | Show current working directory             | pwd                      |
| cd            | Change directory                           | cd /home/user            |
| mkdir         | Create directory                           | mkdir projects           |
| rmdir         | Remove empty directory                     | rmdir old_folder         |
| rm -rf        | Forcefully delete directory with all files | rm -rf temp/             |
| chmod         | Change file permissions                    | chmod 755 file.sh        |
| find / locate | Search for files or directories            | find /home -name “*.txt” |
| ps            | Show running processes                     | ps -ef                   |

---

## FILE PERMISSIONS (chmod)

* Linux uses three types of permissions:
  **r → read**, **w → write**, **x → execute**

* Three user groups:
  **Owner (u)**, **Group (g)**, **Others (o)**

* Example:
  `chmod 755 file.sh`
  → Owner: rwx | Group: r-x | Others: r-x

* `chmod u+x script.sh` → add execute permission for owner.

---

# INTRODUCTION TO SHELL SCRIPTING

Shell scripting helps **automate repetitive Linux tasks**.
Scripts are written in plain text and executed by the shell (usually bash).

---

## 1. “HELLO WORLD” SCRIPT

File: `hello.sh`

```bash
echo "Hello, World!"
```

**Explanation:**

* `#!/bin/bash` → called **shebang**, tells system to use bash shell.
* `echo` → prints text on the terminal.

**To run:**
`chmod +x hello.sh` → make it executable
`./hello.sh`

---

## 2. SHEBANG OPTIONS

| Shebang             | Interpreter        | Use                      |
| ------------------- | ------------------ | ------------------------ |
| #!/bin/bash         | Bash shell         | Most common              |
| #!/bin/sh           | Basic shell        | Faster, limited features |
| #!/usr/bin/env bash | Portable bash path | Works on all systems     |
| #!/bin/zsh          | Z shell            | Used if zsh preferred    |
| #!/usr/bin/python3  | Python interpreter | For Python scripts       |

---

## 3. SIMPLE SHELL SCRIPT EXAMPLES

**a. Show current date and time**

```bash
echo "Current date and time: $(date)"
```

**b. Check disk usage**

```bash
df -h
```

**c. List files**

```bash
ls -l
```

**d. Check if file exists**

```bash
read -p "Enter filename: " file
if [ -f "$file" ]; then
  echo "File exists."
else
  echo "File not found."
fi
```

**e. Add two numbers**

```bash
read -p "Enter first number: " a
read -p "Enter second number: " b
echo "Sum: $((a+b))"
```

**f. System uptime**

```bash
uptime -p
```

**g. Check if user exists**

```bash
read -p "Enter username: " user
if id "$user" &>/dev/null; then
  echo "User exists."
else
  echo "User not found."
fi
```

**h. Backup a directory**

```bash
read -p "Enter source dir: " src
read -p "Enter backup dir: " dest
tar -czf "$dest/backup_$(date +%F).tar.gz" "$src"
echo "Backup completed!"
```

**i. Check internet connectivity**

```bash
if ping -c 1 8.8.8.8 &>/dev/null; then
  echo "Internet is working."
else
  echo "No internet."
fi
```

---

# STRING MANIPULATION IN BASH

**Find length of string:**

```bash
read -p "Enter string: " str
echo "Length: ${#str}"
```

**Uppercase & Lowercase:**

```bash
echo "Uppercase: ${str^^}"
echo "Lowercase: ${str,,}"
```

**Reverse a string:**

```bash
rev_str=$(echo "$str" | rev)
echo "Reversed: $rev_str"
```

**Compare strings:**

```bash
if [[ "$str1" == "$str2" ]]; then
  echo "Equal"
else
  echo "Not Equal"
fi
```

**Extract substring:**

```bash
echo "Substring: ${str:$pos:$len}"
```

---

## /bin/sh vs /bin/bash

| Feature     | /bin/sh                 | /bin/bash                       |
| ----------- | ----------------------- | ------------------------------- |
| Type        | Basic POSIX shell       | Bourne Again Shell              |
| Speed       | Slightly faster         | Slightly slower                 |
| Portability | Exists everywhere       | Default in Linux                |
| Features    | Simple commands only    | Supports arrays, strings, regex |
| Use case    | Light, portable scripts | Feature-rich automation         |

---

## USER CREATION SCRIPT

### Non-standard (basic)

```bash
USERNAME="$1"
GROUPNAME="$2"

groupadd "$GROUPNAME"
useradd -m -s /bin/bash -g "$GROUPNAME" "$USERNAME"

echo "echo \"Welcome, $USERNAME!!\"" >> /home/$USERNAME/.bashrc
```

---

### Standard (professional)

```bash
#!/bin/bash
# Script Name: create_user.sh
# Description: Create new Linux user & group.
# Usage: sudo ./create_user.sh <username> <groupname>

set -e   # Exit on error

if [[ $EUID -ne 0 ]]; then
  echo "Run as root (use sudo)"
  exit 1
fi

if [[ $# -ne 2 ]]; then
  echo "Usage: $0 <username> <groupname>"
  exit 1
fi

USERNAME="$1"
GROUPNAME="$2"

# Create group
if getent group "$GROUPNAME" >/dev/null; then
  echo "Group exists."
else
  groupadd "$GROUPNAME"
fi

# Create user
if id "$USERNAME" &>/dev/null; then
  echo "User exists."
else
  useradd -m -s /bin/bash -g "$GROUPNAME" "$USERNAME"
fi

id "$USERNAME"
read -p "Set password for $USERNAME? (y/n): " choice
if [[ "$choice" == "y" ]]; then
  passwd "$USERNAME"
fi

echo "User setup complete."
```

---

# BEST PRACTICES FOR SHELL SCRIPTING

1. Add clear **comments and documentation**
2. Include **error handling** (`set -e`, if-else checks)
3. Validate inputs before running commands
4. Follow **naming conventions** (lowercase, descriptive names)
5. Use consistent indentation
6. Always make scripts executable → `chmod +x file.sh`

---

# ASSIGNMENT CONCEPT

**Goal:**
Automate project setup by creating timestamped directories.

### utils.sh

```bash
create_timestamped_dir() {
  local project_name="$1"
  local timestamp=$(date +%Y%m%d-%H%M%S)
  local dir_path="/tmp/${project_name}-${timestamp}"
  mkdir -p "$dir_path"
  echo "Directory created: $dir_path"
}
```

### setup_project.sh

```bash
#!/bin/bash
source "$(dirname "$0")/utils.sh"
create_timestamped_dir "my-new-app"
```

**Outcome:**

* Creates unique directory in `/tmp`
* Demonstrates use of functions, sourcing, and timestamps

---

# ACL (ACCESS CONTROL LISTS)

ACLs provide **fine-grained file permissions** beyond basic owner/group/others.

**Why:**
Basic permissions = only owner, one group, others.
ACLs = give specific users/groups custom access.

---

## CHECK IF ACL ENABLED

```bash
mount | grep acl
sudo mount -o remount,acl /home   # enable temporarily
```

Or add in `/etc/fstab` for persistence.

---

## VIEW ACLs

```bash
getfacl file.txt
```

Shows extra entries like:

```
user:john:r--
group:dev:r--
mask::r--
```

---

## SET ACLs

```bash
setfacl -m u:john:r-- file.txt     # give john read
setfacl -m g:devs:rw- file.txt     # give devs read/write
```

**Remove ACLs:**

```bash
setfacl -x u:john file.txt
setfacl -b file.txt     # remove all
```

**Set Recursive ACLs:**

```bash
setfacl -R -m u:john:rwX /var/www/
```

**Default ACLs (new files inherit):**

```bash
setfacl -d -m u:john:rwX /var/www/
```

---

## ACL MASK

Defines the **maximum permission limit** for all non-owner users/groups.
Example:

```bash
setfacl -m m::r-- file.txt
```

→ even if user has `rw`, mask restricts to `r--`.

---

## SCRIPT EXAMPLE – GIVE TEAM ACCESS

```bash
SHARE_DIR="/data/team"
USER_LIST="alice bob charlie"

for user in $USER_LIST; do
  setfacl -m u:$user:rwx $SHARE_DIR
done
```

---

**Summary:**

* Shell scripting automates Linux tasks.
* Use proper structure, error handling, and comments.
* ACLs provide more flexible file permissions for multiple users.
