# 🐧 Linux Commands Cheatsheet for DevOps Engineers
### Prepared for: Abhishek Thakur | Packaged App Development Associate
### Date: May 3, 2026

---

## 📑 Table of Contents

1. [File & Directory Management](#1-file--directory-management)
2. [File Content & Text Processing](#2-file-content--text-processing)
3. [User & Permission Management](#3-user--permission-management)
4. [Process Management](#4-process-management)
5. [Disk & Storage](#5-disk--storage)
6. [Networking](#6-networking)
7. [Package Management (RHEL/CentOS)](#7-package-management-rhelcentos)
8. [Archive & Compression](#8-archive--compression)
9. [SELinux](#9-selinux)
10. [Systemd & Services](#10-systemd--services)
11. [Docker / Podman (Containers)](#11-docker--podman-containers)
12. [Kubernetes / OpenShift (oc / kubectl)](#12-kubernetes--openshift-oc--kubectl)
13. [SSH & Remote Access](#13-ssh--remote-access)
14. [Shell Scripting Essentials](#14-shell-scripting-essentials)
15. [Log Management](#15-log-management)
16. [Ansible Basics](#16-ansible-basics)
17. [Git](#17-git)
18. [Performance & Monitoring](#18-performance--monitoring)
19. [Handy One-Liners & Tips](#19-handy-one-liners--tips)

---

## 1. File & Directory Management

| Command | Syntax | Description |
|---|---|---|
| `ls` | `ls -la /path` | List files with details (`-l`) and hidden files (`-a`) |
| `ls -Z` | `ls -Z /path` | List files with SELinux security context |
| `cd` | `cd /path` | Change directory |
| `pwd` | `pwd` | Print current working directory |
| `mkdir` | `mkdir -p /dir1/dir2` | Create directories recursively |
| `mkdir -Z` | `mkdir -Z context /dir` | Create directory with SELinux context |
| `touch` | `touch file.txt` | Create empty file or update timestamp |
| `cp` | `cp -r source/ dest/` | Copy files/directories (`-r` for recursive) |
| `mv` | `mv old new` | Move or rename files/directories |
| `rm` | `rm -rf /path` | Remove files/directories (`-r` recursive, `-f` force) |
| `find` | `find /path -name "*.log"` | Search files by name, type, size, etc. |
| `find` | `find /path -type f -mtime +30 -delete` | Find and delete files older than 30 days |
| `locate` | `locate filename` | Fast file search (uses database, run `updatedb` first) |
| `ln` | `ln -s /target /link` | Create symbolic link |
| `tree` | `tree -L 2 /path` | Display directory tree (2 levels deep) |
| `stat` | `stat file.txt` | Show detailed file info (size, permissions, timestamps) |
| `chmod` | `chmod 755 file` | Change file permissions (rwx = 7, rw- = 6, r-x = 5, r-- = 4) |
| `chmod` | `chmod u+x script.sh` | Add execute permission for owner |
| `chown` | `chown user:group file` | Change file owner and group |
| `chgrp` | `chgrp groupname file` | Change file group |
| `basename` | `basename /path/to/file.txt` | Extract filename from path → `file.txt` |
| `dirname` | `dirname /path/to/file.txt` | Extract directory from path → `/path/to` |

### 🔢 Permission Reference:

```
r = 4    w = 2    x = 1

 Owner  Group  Others
  rwx    rwx    rwx
  7      5      5     = 755 (common for scripts)
  6      4      4     = 644 (common for files)
  7      0      0     = 700 (private)
```

---

## 2. File Content & Text Processing

| Command | Syntax | Description |
|---|---|---|
| `cat` | `cat file.txt` | Display file content |
| `less` | `less file.txt` | View file with scroll (use `q` to quit) |
| `more` | `more file.txt` | View file page by page |
| `head` | `head -n 20 file.txt` | Show first 20 lines |
| `tail` | `tail -n 20 file.txt` | Show last 20 lines |
| `tail -f` | `tail -f /var/log/messages` | Follow log in real-time (live tail) |
| `grep` | `grep -i "error" file.txt` | Search text (case-insensitive) |
| `grep -r` | `grep -rn "TODO" /src/` | Recursive search with line numbers |
| `grep -v` | `grep -v "^#" config.conf` | Exclude lines starting with `#` (comments) |
| `grep -E` | `grep -E "error|warn|fail" log.txt` | Search multiple patterns (extended regex) |
| `awk` | `awk '{print $1, $3}' file.txt` | Print 1st and 3rd columns |
| `awk` | `awk -F: '{print $1}' /etc/passwd` | Use `:` as field separator |
| `sed` | `sed 's/old/new/g' file.txt` | Find and replace text |
| `sed` | `sed -i 's/old/new/g' file.txt` | In-place find and replace |
| `sed` | `sed -n '5,10p' file.txt` | Print lines 5 to 10 |
| `cut` | `cut -d: -f1 /etc/passwd` | Cut columns by delimiter |
| `sort` | `sort -n file.txt` | Sort numerically |
| `sort` | `sort -k2 -t, file.csv` | Sort by 2nd column, comma-delimited |
| `uniq` | `sort file \| uniq -c` | Count unique occurrences (requires sorted input) |
| `wc` | `wc -l file.txt` | Count lines (`-w` words, `-c` bytes) |
| `diff` | `diff file1 file2` | Compare two files |
| `tee` | `cmd \| tee output.log` | Show output AND save to file |
| `tr` | `echo "HELLO" \| tr 'A-Z' 'a-z'` | Translate characters (uppercase → lowercase) |
| `xargs` | `find . -name "*.tmp" \| xargs rm` | Pass output of one command as arguments to another |

---

## 3. User & Permission Management

| Command | Syntax | Description |
|---|---|---|
| `whoami` | `whoami` | Show current username |
| `id` | `id username` | Show user UID, GID, and groups |
| `useradd` | `useradd -m -s /bin/bash username` | Create user with home dir and bash shell |
| `usermod` | `usermod -aG docker username` | Add user to a supplementary group |
| `userdel` | `userdel -r username` | Delete user and their home directory |
| `passwd` | `passwd username` | Set/change password |
| `groupadd` | `groupadd devops` | Create a new group |
| `groups` | `groups username` | List groups a user belongs to |
| `su` | `su - username` | Switch user |
| `sudo` | `sudo command` | Run command as root/superuser |
| `visudo` | `visudo` | Safely edit sudoers file |
| `chage` | `chage -l username` | View password expiry info |
| `last` | `last` | Show last logged-in users |
| `w` | `w` | Show who is logged in and what they're doing |

### 🔐 Sudoers Quick Reference:

```bash
# Allow user to run all commands without password
username ALL=(ALL) NOPASSWD: ALL

# Allow group to run specific commands
%devops ALL=(ALL) /usr/bin/systemctl, /usr/bin/docker
```

---

## 4. Process Management

| Command | Syntax | Description |
|---|---|---|
| `ps` | `ps aux` | Show all running processes |
| `ps -ef` | `ps -ef \| grep nginx` | Find a specific process |
| `ps -eZ` | `ps -eZ \| grep httpd` | Show processes with SELinux contexts |
| `top` | `top` | Real-time process monitor (press `q` to quit) |
| `htop` | `htop` | Interactive process monitor (better than top) |
| `kill` | `kill -9 PID` | Force kill a process by PID |
| `kill` | `kill -15 PID` | Gracefully terminate a process |
| `killall` | `killall nginx` | Kill all processes by name |
| `pkill` | `pkill -f "pattern"` | Kill process by pattern match |
| `bg` | `bg %1` | Resume suspended job in background |
| `fg` | `fg %1` | Bring background job to foreground |
| `jobs` | `jobs` | List background jobs |
| `nohup` | `nohup ./script.sh &` | Run command that persists after logout |
| `nice` | `nice -n 10 command` | Start process with lower priority |
| `renice` | `renice -n 5 -p PID` | Change priority of running process |
| `pgrep` | `pgrep -a nginx` | Find PIDs by process name |
| `lsof` | `lsof -i :8080` | Show what is using port 8080 |

### 📊 Kill Signals:

```
Signal 15 (SIGTERM) → Graceful shutdown (default)
Signal  9 (SIGKILL) → Force kill (use as last resort)
Signal  1 (SIGHUP)  → Reload configuration
Signal  2 (SIGINT)  → Same as Ctrl+C
```

---

## 5. Disk & Storage

| Command | Syntax | Description |
|---|---|---|
| `df` | `df -hT` | Show disk space usage (human-readable, with filesystem type) |
| `du` | `du -sh /path` | Show directory size |
| `du` | `du -h --max-depth=1 /path` | Show sizes of subdirectories (1 level) |
| `lsblk` | `lsblk` | List block devices (disks, partitions) |
| `fdisk` | `sudo fdisk -l` | List all disks and partitions |
| `mount` | `mount /dev/sdb1 /mnt` | Mount a filesystem |
| `umount` | `umount /mnt` | Unmount a filesystem |
| `mkfs` | `mkfs.ext4 /dev/sdb1` | Format partition as ext4 |
| `blkid` | `blkid` | Show block device UUIDs |
| `free` | `free -h` | Show memory usage (RAM + swap) |
| `swapon` | `swapon --show` | Show swap devices |
| `lvm` | `pvs / vgs / lvs` | List physical volumes / volume groups / logical volumes |
| `lvextend` | `lvextend -L +5G /dev/vg/lv` | Extend a logical volume by 5GB |
| `resize2fs` | `resize2fs /dev/vg/lv` | Resize ext filesystem after lvextend |
| `xfs_growfs` | `xfs_growfs /mountpoint` | Resize XFS filesystem after lvextend |

---

## 6. Networking

| Command | Syntax | Description |
|---|---|---|
| `ip a` | `ip a` | Show all IP addresses |
| `ip r` | `ip r` | Show routing table |
| `ip link` | `ip link set eth0 up/down` | Enable/disable network interface |
| `ping` | `ping -c 4 google.com` | Test connectivity (4 packets) |
| `traceroute` | `traceroute google.com` | Trace packet route to destination |
| `ss` | `ss -tlnp` | Show listening TCP ports with process info |
| `ss` | `ss -ulnp` | Show listening UDP ports |
| `netstat` | `netstat -tlnp` | (Legacy) Show listening ports |
| `curl` | `curl -I https://example.com` | Fetch HTTP headers |
| `curl` | `curl -X POST -d '{"key":"val"}' -H "Content-Type: application/json" URL` | POST request with JSON |
| `curl` | `curl -o file.zip URL` | Download file |
| `wget` | `wget -q URL` | Download file quietly |
| `nslookup` | `nslookup domain.com` | DNS lookup |
| `dig` | `dig domain.com +short` | Detailed DNS lookup |
| `hostname` | `hostnamectl` | Show/set hostname |
| `firewall-cmd` | `firewall-cmd --list-all` | Show firewall rules (RHEL/CentOS) |
| `firewall-cmd` | `firewall-cmd --add-port=8080/tcp --permanent` | Open a port permanently |
| `firewall-cmd` | `firewall-cmd --reload` | Reload firewall rules |
| `iptables` | `iptables -L -n` | List iptables rules |
| `nc` | `nc -zv host 22` | Test if a specific port is open |
| `nmcli` | `nmcli con show` | NetworkManager CLI — show connections |

### 🔍 Quick Port Check:

```bash
# Is port 443 open on a remote host?
nc -zv remote-host 443

# What's using port 8080 on my machine?
ss -tlnp | grep 8080
# OR
lsof -i :8080
```

---

## 7. Package Management (RHEL/CentOS)

| Command | Syntax | Description |
|---|---|---|
| `dnf install` | `sudo dnf install -y nginx` | Install a package |
| `dnf remove` | `sudo dnf remove nginx` | Remove a package |
| `dnf update` | `sudo dnf update -y` | Update all packages |
| `dnf search` | `dnf search keyword` | Search for packages |
| `dnf info` | `dnf info nginx` | Show package details |
| `dnf list installed` | `dnf list installed` | List all installed packages |
| `dnf provides` | `dnf provides */netstat` | Find which package provides a command |
| `dnf history` | `dnf history` | Show package transaction history |
| `dnf clean` | `dnf clean all` | Clear package cache |
| `rpm -qa` | `rpm -qa \| grep nginx` | List installed RPMs |
| `rpm -qi` | `rpm -qi nginx` | Show RPM package info |
| `rpm -ql` | `rpm -ql nginx` | List files installed by an RPM |
| `rpm -qf` | `rpm -qf /usr/sbin/nginx` | Find which RPM owns a file |

> 💡 `yum` still works on RHEL 8/9 but is a symlink to `dnf`.

---

## 8. Archive & Compression

| Command | Syntax | Description |
|---|---|---|
| `tar -czf` | `tar -czf archive.tar.gz /path/` | Create gzip-compressed tar archive |
| `tar -xzf` | `tar -xzf archive.tar.gz` | Extract gzip tar archive |
| `tar -cjf` | `tar -cjf archive.tar.bz2 /path/` | Create bzip2 tar archive |
| `tar -xjf` | `tar -xjf archive.tar.bz2` | Extract bzip2 tar archive |
| `tar -tf` | `tar -tf archive.tar.gz` | List contents without extracting |
| `gzip` | `gzip file.txt` | Compress file (replaces original) |
| `gunzip` | `gunzip file.txt.gz` | Decompress gzip file |
| `zip` | `zip -r archive.zip /path/` | Create zip archive |
| `unzip` | `unzip archive.zip -d /dest/` | Extract zip to destination |

### 🧠 Tar Flags Memory Trick:

```
c = Create        x = eXtract
z = gZip          j = bzip2 (J for xz)
f = File          v = Verbose
t = lisT contents
```

---

## 9. SELinux

| Command | Syntax | Description |
|---|---|---|
| `getenforce` | `getenforce` | Show current SELinux mode (Enforcing/Permissive/Disabled) |
| `sestatus` | `sestatus` | Show detailed SELinux status |
| `setenforce` | `setenforce 0` | Temporarily set to Permissive (0) or Enforcing (1) |
| `ls -Z` | `ls -Z /path` | Show file SELinux labels |
| `ps -eZ` | `ps -eZ \| grep httpd` | Show process SELinux labels |
| `chcon` | `chcon -t httpd_sys_content_t /path` | Change SELinux type (temporary) |
| `restorecon` | `restorecon -R -v /path` | Restore default SELinux labels |
| `semanage fcontext` | `semanage fcontext --add --type httpd_sys_content_t '/path(/.*)?'` | Add permanent label rule |
| `semanage fcontext` | `semanage fcontext --list \| grep /path` | List label rules |
| `getsebool` | `getsebool -a` | List all SELinux booleans |
| `setsebool` | `setsebool -P httpd_can_network_connect on` | Set boolean permanently |
| `audit2why` | `audit2why < /var/log/audit/audit.log` | Explain why SELinux denied something |
| `audit2allow` | `audit2allow -a` | Generate policy to allow denied actions |

### 🚨 SELinux Troubleshooting Workflow:

```bash
# Step 1: Check if SELinux is the problem
getenforce                              # Is it Enforcing?
grep "denied" /var/log/audit/audit.log  # Any recent denials?

# Step 2: Understand the denial
audit2why < /var/log/audit/audit.log

# Step 3: Fix it (choose one)
# Option A: Fix the label
restorecon -R -v /path

# Option B: Set a boolean
setsebool -P boolean_name on

# Option C: Add a custom label rule
semanage fcontext --add --type correct_type_t '/path(/.*)?'
restorecon -R -v /path
```

---

## 10. Systemd & Services

| Command | Syntax | Description |
|---|---|---|
| `systemctl start` | `systemctl start nginx` | Start a service |
| `systemctl stop` | `systemctl stop nginx` | Stop a service |
| `systemctl restart` | `systemctl restart nginx` | Restart a service |
| `systemctl reload` | `systemctl reload nginx` | Reload config without restarting |
| `systemctl enable` | `systemctl enable nginx` | Enable service at boot |
| `systemctl disable` | `systemctl disable nginx` | Disable service at boot |
| `systemctl status` | `systemctl status nginx` | Check service status |
| `systemctl is-active` | `systemctl is-active nginx` | Check if service is running |
| `systemctl is-enabled` | `systemctl is-enabled nginx` | Check if service starts at boot |
| `systemctl list-units` | `systemctl list-units --type=service --state=running` | List running services |
| `systemctl list-unit-files` | `systemctl list-unit-files --type=service` | List all service unit files |
| `systemctl daemon-reload` | `systemctl daemon-reload` | Reload systemd after editing unit files |
| `systemctl mask` | `systemctl mask nginx` | Prevent service from starting entirely |
| `systemctl unmask` | `systemctl unmask nginx` | Remove mask |

### 📋 Journalctl (Log Viewer for Systemd):

```bash
journalctl -u nginx                    # Logs for nginx service
journalctl -u nginx --since "1 hour ago"  # Last hour's logs
journalctl -u nginx -f                  # Follow logs in real-time
journalctl -p err                       # Show only errors
journalctl --disk-usage                 # Check journal disk usage
journalctl --vacuum-size=500M           # Trim journal to 500MB
```

---

## 11. Docker / Podman (Containers)

> 💡 Podman is the preferred container tool on RHEL (rootless, daemonless).
> Most Docker commands work with Podman — just replace `docker` with `podman`.

| Command | Syntax | Description |
|---|---|---|
| `run` | `podman run -d -p 8080:80 --name web nginx` | Run container in background |
| `run` | `podman run -it --rm ubuntu bash` | Run interactive container (remove on exit) |
| `run -v` | `podman run -v /host:/container:Z nginx` | Mount volume (`:Z` for SELinux relabel) |
| `ps` | `podman ps -a` | List all containers (including stopped) |
| `logs` | `podman logs -f container_name` | Follow container logs |
| `exec` | `podman exec -it container_name bash` | Open shell inside running container |
| `stop` | `podman stop container_name` | Stop a container |
| `rm` | `podman rm container_name` | Remove a stopped container |
| `rmi` | `podman rmi image_name` | Remove an image |
| `images` | `podman images` | List downloaded images |
| `build` | `podman build -t myapp:v1 .` | Build image from Dockerfile |
| `inspect` | `podman inspect container_name` | Show container details (JSON) |
| `network` | `podman network ls` | List container networks |
| `volume` | `podman volume ls` | List volumes |
| `cp` | `podman cp file.txt container:/path/` | Copy files into/from container |
| `system prune` | `podman system prune -a` | Remove all unused containers, images, volumes |

### 🐳 Dockerfile Quick Reference:

```dockerfile
FROM node:18-alpine          # Base image
WORKDIR /app                 # Set working directory
COPY package*.json ./        # Copy dependency files
RUN npm install              # Install dependencies
COPY . .                     # Copy app source
EXPOSE 3000                  # Document port
CMD ["node", "server.js"]    # Default command
```

---

## 12. Kubernetes / OpenShift (oc / kubectl)

> 💡 `oc` includes all `kubectl` commands + OpenShift-specific features.

### Core Commands:

| Command | Syntax | Description |
|---|---|---|
| `login` | `oc login https://api.cluster:6443 -u user -p pass` | Login to OpenShift cluster |
| `project` | `oc project my-namespace` | Switch to a project/namespace |
| `new-project` | `oc new-project dev-team` | Create new project |
| `get` | `oc get pods -o wide` | List pods with node info |
| `get` | `oc get all -n namespace` | List all resources in namespace |
| `get` | `oc get nodes` | List cluster nodes |
| `describe` | `oc describe pod pod-name` | Show detailed info about a pod |
| `logs` | `oc logs pod-name -c container` | View pod logs (specific container) |
| `logs` | `oc logs -f pod-name` | Follow pod logs in real-time |
| `exec` | `oc exec -it pod-name -- bash` | Open shell in a pod |
| `apply` | `oc apply -f manifest.yaml` | Apply a YAML manifest |
| `delete` | `oc delete pod pod-name` | Delete a resource |
| `scale` | `oc scale deployment app --replicas=3` | Scale replicas |
| `rollout` | `oc rollout status deployment/app` | Check rollout status |
| `rollout` | `oc rollout undo deployment/app` | Rollback to previous version |
| `port-forward` | `oc port-forward pod-name 8080:80` | Forward local port to pod |
| `top` | `oc top pods` | Show pod CPU/memory usage |
| `edit` | `oc edit deployment/app` | Edit resource in-place |
| `cp` | `oc cp pod-name:/path local-file` | Copy files from pod |
| `adm` | `oc adm top nodes` | Show node resource usage |

### 🔍 Debugging Pods:

```bash
# Why is my pod stuck?
oc describe pod pod-name          # Events section shows errors
oc logs pod-name --previous       # Logs from crashed container
oc get events --sort-by=.lastTimestamp  # Recent cluster events

# Quick debug pod
oc run debug --rm -it --image=busybox -- sh

# Check resource quotas
oc describe quota -n namespace
oc describe limitrange -n namespace
```

### 📋 Common Resource Types:

```
pod (po)          service (svc)        deployment (deploy)
replicaset (rs)   configmap (cm)       secret
namespace (ns)    node (no)            ingress (ing)
persistentvolumeclaim (pvc)            serviceaccount (sa)
daemonset (ds)    statefulset (sts)    job / cronjob
route (OpenShift-specific)
```

---

## 13. SSH & Remote Access

| Command | Syntax | Description |
|---|---|---|
| `ssh` | `ssh user@host` | Connect to remote host |
| `ssh` | `ssh -p 2222 user@host` | Connect on custom port |
| `ssh` | `ssh -i ~/.ssh/key.pem user@host` | Connect with specific key |
| `ssh-keygen` | `ssh-keygen -t ed25519 -C "email"` | Generate SSH key pair |
| `ssh-copy-id` | `ssh-copy-id user@host` | Copy public key to remote host |
| `scp` | `scp file.txt user@host:/path/` | Copy file to remote host |
| `scp` | `scp -r user@host:/path/ local/` | Copy directory from remote |
| `rsync` | `rsync -avz /src/ user@host:/dest/` | Sync files (faster than scp for large transfers) |
| `rsync` | `rsync -avz --delete /src/ /dest/` | Sync and delete extra files at destination |

### 🔧 SSH Config File (`~/.ssh/config`):

```
Host jumpserver
    HostName 10.0.0.5
    User abhishek
    IdentityFile ~/.ssh/id_ed25519
    Port 22

Host aro-node
    HostName 10.1.0.10
    User core
    ProxyJump jumpserver          # Jump through jumpserver first
    IdentityFile ~/.ssh/aro_key
```

```bash
# Now just type:
ssh jumpserver
ssh aro-node       # Automatically jumps through jumpserver
```

---

## 14. Shell Scripting Essentials

### Variables:

```bash
NAME="Abhishek"
echo "Hello, $NAME"
echo "Date: $(date)"         # Command substitution
echo "Home: ${HOME}"         # Variable with braces
readonly PI=3.14             # Constant (can't be changed)
```

### If / Else:

```bash
if [ -f "/path/file" ]; then
    echo "File exists"
elif [ -d "/path/dir" ]; then
    echo "Directory exists"
else
    echo "Not found"
fi

# One-liner
[ -f "/path/file" ] && echo "Exists" || echo "Not found"
```

### Test Conditions:

```
-f file    → file exists and is a regular file
-d dir     → directory exists
-s file    → file exists and is not empty
-z "$var"  → variable is empty
-n "$var"  → variable is not empty
-eq        → equal (numbers)
-ne        → not equal
-gt / -lt  → greater than / less than
==         → equal (strings)
!=         → not equal (strings)
```

### For Loop:

```bash
# Loop through list
for server in web1 web2 web3; do
    echo "Pinging $server"
    ping -c 1 $server
done

# Loop through files
for file in /var/log/*.log; do
    echo "Processing: $file"
done

# C-style loop
for ((i=1; i<=5; i++)); do
    echo "Iteration $i"
done
```

### While Loop:

```bash
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Read file line by line
while IFS= read -r line; do
    echo "$line"
done < input.txt
```

### Functions:

```bash
deploy_app() {
    local app_name=$1
    local version=$2
    echo "Deploying $app_name v$version..."
    return 0
}

deploy_app "myapp" "2.1"
```

### Exit Codes:

```bash
command
if [ $? -eq 0 ]; then
    echo "Success"
else
    echo "Failed with exit code: $?"
fi

# Common exit codes:
# 0   = Success
# 1   = General error
# 2   = Misuse of command
# 126 = Permission denied
# 127 = Command not found
# 130 = Ctrl+C
```

### Redirections:

```bash
command > file.txt          # Redirect stdout to file (overwrite)
command >> file.txt         # Append stdout to file
command 2> error.log        # Redirect stderr to file
command &> all.log          # Redirect both stdout + stderr
command 2>&1                # Redirect stderr to stdout
command < input.txt         # Use file as stdin
command1 | command2         # Pipe stdout of cmd1 to cmd2
```

### Cron Jobs (`crontab -e`):

```
# ┌───── minute (0-59)
# │ ┌───── hour (0-23)
# │ │ ┌───── day of month (1-31)
# │ │ │ ┌───── month (1-12)
# │ │ │ │ ┌───── day of week (0-7, 0 & 7 = Sunday)
# │ │ │ │ │
  * * * * * command

# Examples:
0 2 * * *   /opt/backup.sh          # Daily at 2 AM
*/5 * * * * /opt/health-check.sh    # Every 5 minutes
0 0 * * 0   /opt/weekly-cleanup.sh  # Every Sunday midnight
0 9 * * 1-5 /opt/report.sh          # Weekdays at 9 AM
```

---

## 15. Log Management

### Important Log Files:

| Log File | What It Contains |
|---|---|
| `/var/log/messages` | General system messages (RHEL/CentOS) |
| `/var/log/syslog` | General system messages (Ubuntu/Debian) |
| `/var/log/secure` | Authentication and sudo logs (RHEL) |
| `/var/log/audit/audit.log` | SELinux denials, security events |
| `/var/log/boot.log` | Boot process messages |
| `/var/log/cron` | Cron job execution logs |
| `/var/log/httpd/` | Apache web server logs |
| `/var/log/nginx/` | NGINX logs |
| `/var/log/containers/` | Container logs (Podman/OpenShift) |
| `/var/log/pods/` | Kubernetes/OpenShift pod logs |

### Log Commands:

```bash
# Follow a log in real-time
tail -f /var/log/messages

# Follow multiple logs
tail -f /var/log/messages /var/log/secure

# Search logs for errors
grep -i "error\|fail\|critical" /var/log/messages

# Logs since last boot
journalctl -b

# Logs for a specific time range
journalctl --since "2026-05-03 08:00" --until "2026-05-03 12:00"

# Check log rotation config
cat /etc/logrotate.conf
ls /etc/logrotate.d/
```

---

## 16. Ansible Basics

### Commands:

| Command | Syntax | Description |
|---|---|---|
| `ansible` | `ansible all -m ping -i inventory` | Ad-hoc: ping all hosts |
| `ansible` | `ansible web -m shell -a "uptime" -i inventory` | Ad-hoc: run command on group |
| `ansible-playbook` | `ansible-playbook playbook.yml -i inventory` | Run a playbook |
| `ansible-playbook` | `ansible-playbook playbook.yml --check` | Dry run (don't make changes) |
| `ansible-playbook` | `ansible-playbook playbook.yml --limit web1` | Run only on specific host |
| `ansible-playbook` | `ansible-playbook playbook.yml -e "env=prod"` | Pass extra variables |
| `ansible-vault` | `ansible-vault encrypt secrets.yml` | Encrypt a file |
| `ansible-vault` | `ansible-vault decrypt secrets.yml` | Decrypt a file |
| `ansible-vault` | `ansible-vault view secrets.yml` | View encrypted file |
| `ansible-galaxy` | `ansible-galaxy init myrole` | Create role structure |
| `ansible-galaxy` | `ansible-galaxy install geerlingguy.docker` | Install role from Galaxy |

### Inventory File Example (`inventory.ini`):

```ini
[webservers]
web1 ansible_host=10.0.0.1
web2 ansible_host=10.0.0.2

[dbservers]
db1 ansible_host=10.0.1.1

[all:vars]
ansible_user=abhishek
ansible_ssh_private_key_file=~/.ssh/id_ed25519
```

### Playbook Example:

```yaml
---
- name: Configure web servers
  hosts: webservers
  become: yes

  tasks:
    - name: Install nginx
      dnf:
        name: nginx
        state: present

    - name: Copy config file
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify: Restart nginx

    - name: Ensure nginx is running
      service:
        name: nginx
        state: started
        enabled: true

  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
```

### Common Modules:

| Module | Usage |
|---|---|
| `dnf` / `yum` | Install/remove packages |
| `service` / `systemd` | Manage services |
| `copy` | Copy files to remote |
| `template` | Copy Jinja2 templates |
| `file` | Manage files, directories, permissions |
| `lineinfile` | Ensure a line exists in a file |
| `command` | Run a command (no shell features) |
| `shell` | Run a command (with shell features like pipes) |
| `user` | Manage user accounts |
| `group` | Manage groups |
| `cron` | Manage cron jobs |
| `debug` | Print debug messages |
| `uri` | Make HTTP requests |

---

## 17. Git

| Command | Syntax | Description |
|---|---|---|
| `init` | `git init` | Initialize a new Git repo |
| `clone` | `git clone URL` | Clone a remote repo |
| `status` | `git status` | Check working tree status |
| `add` | `git add .` | Stage all changes |
| `add` | `git add file.txt` | Stage specific file |
| `commit` | `git commit -m "message"` | Commit staged changes |
| `commit` | `git commit --amend` | Edit last commit message |
| `push` | `git push origin main` | Push to remote branch |
| `pull` | `git pull origin main` | Pull latest changes |
| `fetch` | `git fetch --all` | Fetch all remote changes (without merging) |
| `branch` | `git branch` | List branches |
| `branch` | `git branch feature-x` | Create new branch |
| `branch` | `git branch -d feature-x` | Delete branch |
| `checkout` | `git checkout feature-x` | Switch branch |
| `checkout` | `git checkout -b feature-x` | Create and switch to new branch |
| `switch` | `git switch feature-x` | Switch branch (modern syntax) |
| `merge` | `git merge feature-x` | Merge branch into current branch |
| `rebase` | `git rebase main` | Rebase current branch onto main |
| `stash` | `git stash` | Save uncommitted changes temporarily |
| `stash` | `git stash pop` | Restore stashed changes |
| `log` | `git log --oneline --graph` | Compact log with branch graph |
| `diff` | `git diff` | Show unstaged changes |
| `diff` | `git diff --staged` | Show staged changes |
| `reset` | `git reset --hard HEAD~1` | Undo last commit (DESTRUCTIVE) |
| `reset` | `git reset --soft HEAD~1` | Undo last commit (keep changes staged) |
| `tag` | `git tag -a v1.0 -m "Release 1.0"` | Create annotated tag |
| `cherry-pick` | `git cherry-pick commit-hash` | Apply specific commit to current branch |

### 🔁 Git Workflow Diagram:

```
Working Dir  →  Staging Area  →  Local Repo  →  Remote Repo
   (edit)     git add          git commit      git push
              ←────────────────────────────── git pull
```

---

## 18. Performance & Monitoring

| Command | Syntax | Description |
|---|---|---|
| `top` | `top` | Real-time system monitor |
| `htop` | `htop` | Better interactive system monitor |
| `uptime` | `uptime` | System uptime and load averages |
| `vmstat` | `vmstat 2 5` | Virtual memory stats (every 2 sec, 5 times) |
| `iostat` | `iostat -x 2` | Disk I/O statistics |
| `sar` | `sar -u 2 5` | CPU usage history |
| `sar` | `sar -r 2 5` | Memory usage history |
| `dmesg` | `dmesg -T \| tail -20` | Kernel messages with timestamps |
| `lsof` | `lsof -i :80` | Show processes using port 80 |
| `lsof` | `lsof -u username` | Files opened by a user |
| `strace` | `strace -p PID` | Trace system calls of a process |
| `tcpdump` | `tcpdump -i eth0 port 443` | Capture network traffic |
| `nmon` | `nmon` | All-in-one performance monitor |
| `mpstat` | `mpstat -P ALL 2` | Per-CPU usage |

### 📊 Load Average Guide:

```
uptime output: load average: 1.05, 2.10, 3.15
                              ↑      ↑      ↑
                            1 min  5 min  15 min

Rule of thumb: Load should be ≤ number of CPU cores
  - 4 cores → load ≤ 4.0 is OK
  - Load > cores → system is overloaded
  
Check core count: nproc
```

### 🧠 Quick Health Check Script:

```bash
#!/bin/bash
echo "===== SYSTEM HEALTH CHECK ====="
echo ""
echo "--- Hostname & Uptime ---"
hostname; uptime
echo ""
echo "--- CPU Load ---"
top -bn1 | head -5
echo ""
echo "--- Memory ---"
free -h
echo ""
echo "--- Disk ---"
df -hT | grep -v tmpfs
echo ""
echo "--- Top 5 CPU Processes ---"
ps aux --sort=-%cpu | head -6
echo ""
echo "--- Top 5 Memory Processes ---"
ps aux --sort=-%mem | head -6
echo ""
echo "--- SELinux Status ---"
getenforce 2>/dev/null || echo "SELinux not available"
echo ""
echo "===== END ====="
```

---

## 19. Handy One-Liners & Tips

### History Tricks:

```bash
!!                  # Repeat last command
sudo !!             # Re-run last command with sudo
!$                  # Last argument of previous command
!grep               # Run last command starting with 'grep'
history | grep ssh  # Search command history
Ctrl + R            # Reverse search in history
```

### Useful Aliases (add to `~/.bashrc`):

```bash
alias ll='ls -la'
alias lz='ls -laZ'
alias gs='git status'
alias gp='git pull'
alias k='kubectl'
alias oc='oc'
alias dc='docker-compose'
alias tf='terraform'
alias ap='ansible-playbook'
alias ports='ss -tlnp'
alias myip='curl -s ifconfig.me'
alias reload='source ~/.bashrc'
```

### Environment Variables:

```bash
export PATH=$PATH:/usr/local/bin     # Add to PATH
export KUBECONFIG=~/.kube/config     # Set kubeconfig
env                                   # Show all env variables
printenv VARIABLE                     # Show specific variable
echo $VARIABLE                        # Print variable value
```

### Power One-Liners:

```bash
# Find large files (>100MB)
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# Count files by extension in current dir
find . -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn

# Kill all zombie processes
ps aux | awk '{if ($8=="Z") print $2}' | xargs kill -9

# Check which ports are listening
ss -tlnp | awk 'NR>1 {print $4}' | sort -u

# Watch a command every 2 seconds
watch -n 2 'kubectl get pods'

# Replace text in all files recursively
find . -type f -name "*.yml" -exec sed -i 's/old/new/g' {} +

# Disk usage sorted by size
du -sh /* 2>/dev/null | sort -rh | head -10

# Find files modified in last 24 hours
find /path -type f -mtime -1

# Check SSL certificate expiry
echo | openssl s_client -connect host:443 2>/dev/null | openssl x509 -noout -dates

# Generate random password
openssl rand -base64 16

# Quick HTTP server (python)
python3 -m http.server 8080

# Compare two directories
diff <(ls dir1) <(ls dir2)
```

### Keyboard Shortcuts:

```
Ctrl + C     → Kill current command
Ctrl + Z     → Suspend current command (resume with fg/bg)
Ctrl + D     → Logout / Exit
Ctrl + L     → Clear screen
Ctrl + A     → Move cursor to beginning of line
Ctrl + E     → Move cursor to end of line
Ctrl + W     → Delete word before cursor
Ctrl + U     → Delete everything before cursor
Ctrl + K     → Delete everything after cursor
Ctrl + R     → Reverse search history
Tab          → Auto-complete
Tab Tab      → Show all completions
```

---

## 📌 Quick Reference Card

| Task | Command |
|---|---|
| Check disk space | `df -hT` |
| Check memory | `free -h` |
| Check CPU load | `uptime` or `top` |
| Who's logged in | `w` |
| Check open ports | `ss -tlnp` |
| Check logs | `journalctl -u service -f` |
| Check SELinux | `getenforce` |
| Find a file | `find / -name "filename"` |
| Check DNS | `dig domain +short` |
| Test connectivity | `ping -c 4 host` |
| Check service status | `systemctl status service` |
| Tail multiple logs | `tail -f /var/log/{messages,secure}` |

---

> 💡 **Pro Tip:** Bookmark this file and keep it on your jump server as a quick reference!
>
> 📚 **Want to learn more?** Check out these courses available on Workday:
> - CompTIA Linux+: Managing SELinux
> - RHEL 8: Managing Security
> - RHEL 9 System Hardening and Security

---

*Generated by Microsoft Copilot — May 3, 2026*
