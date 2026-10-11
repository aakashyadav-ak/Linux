


# 🐧 Linux Beginner Roadmap

---

## Phase 1: Linux Basics (Week 1-2)

### 1.1 What is Linux
- [x] What is Linux, Kernel, Shell
- [x] What is a Distribution (RHEL, Ubuntu, CentOS, Rocky)
- [x] Linux vs Windows differences
- [x] Install Rocky Linux / Ubuntu on VirtualBox
- [x] Connect via SSH using terminal

### 1.2 Linux Directory Structure
- [x] `/` — Root
- [x] `/home` — User home directories
- [x] `/etc` — Configuration files
- [x] `/var` — Logs, variable data
- [x] `/tmp` — Temporary files
- [x] `/boot` — Boot files
- [x] `/opt` — Optional software
- [x] `/usr` — User programs
- [x] `/dev` — Device files
- [x] `/proc` — Process information

### 1.3 Basic Navigation Commands
```bash
pwd           # Where am I
ls            # List files
ls -la        # List with details + hidden
cd            # Change directory
cd ..         # Go back
cd ~          # Go home
clear         # Clear screen
history       # Command history
```

### 1.4 File & Directory Operations
```bash
touch file.txt          # Create empty file
mkdir folder            # Create directory
mkdir -p a/b/c          # Create nested directories
cp file.txt /tmp/       # Copy file
cp -r folder/ /tmp/     # Copy directory
mv file.txt newname.txt # Rename/Move
rm file.txt             # Delete file
rm -rf folder/          # Delete directory
```

### 1.5 File Viewing Commands
```bash
cat file.txt            # View full file
less file.txt           # View with scroll
head file.txt           # First 10 lines
head -20 file.txt       # First 20 lines
tail file.txt           # Last 10 lines
tail -f /var/log/messages  # Live log watching
wc -l file.txt          # Count lines
```

### 1.6 Vi/Vim Editor (Basic)
- [ ] Open file: `vim file.txt`
- [ ] Press `i` — Insert mode (type text)
- [ ] Press `Esc` — Back to command mode
- [ ] `:wq` — Save and quit
- [ ] `:q!` — Quit without saving
- [ ] `dd` — Delete line
- [ ] `yy` — Copy line
- [ ] `p` — Paste
- [ ] `/word` — Search

---

## Phase 2: Users, Groups & Permissions (Week 3-4)

### 2.1 User Management
```bash
whoami                      # Current user
id                          # User ID details
useradd john                # Create user
passwd john                 # Set password
usermod -aG wheel john      # Add to sudo group
userdel -r john             # Delete user + home
```

### 2.2 Important User Files
- [ ] `/etc/passwd` — All users list
- [ ] `/etc/shadow` — Encrypted passwords
- [ ] `/etc/group` — All groups

### 2.3 Group Management
```bash
groupadd devops             # Create group
usermod -aG devops john     # Add user to group
groups john                 # Check user groups
groupdel devops             # Delete group
```

### 2.4 File Permissions
```
Permission Format: rwx rwx rwx
                   Owner Group Others

r = read (4)
w = write (2)
x = execute (1)
```

```bash
chmod 755 file.txt          # rwxr-xr-x
chmod 644 file.txt          # rw-r--r--
chmod u+x script.sh         # Add execute for owner
chown john file.txt         # Change owner
chown john:devops file.txt  # Change owner + group
```

### 2.5 Sudo Access
- [ ] `su -` — Switch to root
- [ ] `sudo command` — Run as root
- [ ] `visudo` — Edit sudoers file safely
- [ ] `/etc/sudoers.d/` — Custom sudo rules

---

## Phase 3: Package Management (Week 4-5)

### 3.1 RHEL/CentOS/Rocky (dnf/yum)
```bash
dnf install httpd           # Install package
dnf remove httpd            # Remove package
dnf update                  # Update all packages
dnf search nginx            # Search package
dnf list installed          # List installed
dnf info httpd              # Package details
```

### 3.2 Ubuntu/Debian (apt)
```bash
apt update                  # Update repo list
apt install nginx           # Install package
apt remove nginx            # Remove package
apt upgrade                 # Upgrade all
apt search nginx            # Search
```

### 3.3 RPM (Low Level)
```bash
rpm -ivh package.rpm        # Install
rpm -qa                     # List all installed
rpm -qi httpd               # Info about package
rpm -e httpd                # Remove
```

---

## Phase 4: Service & Process Management (Week 5-6)

### 4.1 systemctl (Service Management)
```bash
systemctl start httpd       # Start service
systemctl stop httpd        # Stop service
systemctl restart httpd     # Restart
systemctl enable httpd      # Start on boot
systemctl disable httpd     # Don't start on boot
systemctl status httpd      # Check status
systemctl list-units --type=service  # All services
```

### 4.2 Process Management
```bash
ps aux                      # All running processes
ps aux | grep httpd         # Find specific process
top                         # Live process monitor
htop                        # Better process monitor (install first)
kill PID                    # Kill by process ID
kill -9 PID                 # Force kill
killall httpd               # Kill by name
```

### 4.3 Background Jobs
```bash
command &                   # Run in background
jobs                        # List background jobs
fg                          # Bring to foreground
nohup command &             # Keep running after logout
```

---

## Phase 5: Networking Basics (Week 6-7)

### 5.1 Network Commands
```bash
ip addr                     # Show IP addresses
ip route                    # Show routing table
hostname                    # Show hostname
hostnamectl set-hostname server1  # Set hostname
ping google.com             # Test connectivity
traceroute google.com       # Trace path
ss -tulnp                   # Show listening ports
curl http://example.com     # Test HTTP
wget http://example.com/file  # Download file
```

### 5.2 Network Configuration Files
- [ ] `/etc/hostname` — Hostname
- [ ] `/etc/hosts` — Local DNS mapping
- [ ] `/etc/resolv.conf` — DNS servers
- [ ] `nmcli` — Network Manager CLI
- [ ] `nmtui` — Network Manager TUI (easy GUI in terminal)

### 5.3 SSH (Secure Shell)
```bash
ssh user@192.168.1.10       # Connect to remote server
ssh -p 2222 user@server     # Connect on custom port
ssh-keygen                  # Generate SSH key pair
ssh-copy-id user@server     # Copy key to server
scp file.txt user@server:/tmp/  # Copy file to remote
```

### 5.4 SSH Hardening (in `/etc/ssh/sshd_config`)
- [ ] Change default port from 22
- [ ] `PermitRootLogin no`
- [ ] `PasswordAuthentication no` (use keys)
- [ ] Restart: `systemctl restart sshd`

---

## Phase 6: Storage & Disk Management (Week 7-8)

### 6.1 Disk Commands
```bash
lsblk                      # List all disks/partitions
df -h                       # Disk space usage
du -sh /var/                # Directory size
fdisk /dev/sdb              # Partition a disk
blkid                       # Show UUIDs
```

### 6.2 Mount & Unmount
```bash
mount /dev/sdb1 /mnt/data   # Mount partition
umount /mnt/data             # Unmount
```

### 6.3 Permanent Mounting (`/etc/fstab`)
```
/dev/sdb1  /mnt/data  xfs  defaults  0  0
```

### 6.4 LVM (Logical Volume Manager)
```bash
# Create
pvcreate /dev/sdb
vgcreate myvg /dev/sdb
lvcreate -L 5G -n mylv myvg
mkfs.xfs /dev/myvg/mylv
mount /dev/myvg/mylv /mnt/data

# Extend
lvextend -L +2G /dev/myvg/mylv
xfs_growfs /mnt/data
```

---

## Phase 7: Firewall & Basic Security (Week 8-9)

### 7.1 firewalld (RHEL/Rocky)
```bash
systemctl start firewalld
systemctl enable firewalld
firewall-cmd --state
firewall-cmd --list-all
firewall-cmd --add-service=http --permanent
firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd --remove-service=http --permanent
firewall-cmd --reload
```

### 7.2 UFW (Ubuntu)
```bash
ufw enable
ufw status
ufw allow 22
ufw allow 80/tcp
ufw deny 3306
ufw delete allow 80/tcp
```

### 7.3 SELinux (RHEL/Rocky)
```bash
getenforce                  # Check status
sestatus                    # Detailed status
setenforce 0                # Set permissive (temporary)
setenforce 1                # Set enforcing (temporary)
# Permanent: edit /etc/selinux/config
```

### 7.4 Fail2ban
```bash
dnf install fail2ban        # Install
systemctl enable fail2ban
systemctl start fail2ban
# Config: /etc/fail2ban/jail.local
fail2ban-client status      # Check status
fail2ban-client status sshd # SSH jail status
```

---

## Phase 8: Logs & Troubleshooting (Week 9-10)

### 8.1 Important Log Files
- [ ] `/var/log/messages` — System log (RHEL)
- [ ] `/var/log/syslog` — System log (Ubuntu)
- [ ] `/var/log/secure` — Auth log (RHEL)
- [ ] `/var/log/auth.log` — Auth log (Ubuntu)
- [ ] `/var/log/httpd/` — Apache logs
- [ ] `/var/log/nginx/` — Nginx logs
- [ ] `/var/log/boot.log` — Boot log

### 8.2 Log Commands
```bash
tail -f /var/log/messages           # Watch live
journalctl                          # All systemd logs
journalctl -u sshd                  # Specific service
journalctl -xe                      # Recent errors
journalctl --since "1 hour ago"     # Time based
```

### 8.3 Text Searching & Filtering
```bash
grep "error" /var/log/messages      # Search word
grep -i "error" file.txt            # Case insensitive
grep -r "error" /var/log/           # Recursive search
grep -c "error" file.txt            # Count matches
cat file | awk '{print $1}'         # Print 1st column
cat file | cut -d: -f1              # Cut by delimiter
cat file | sort | uniq              # Sort & unique
```

---

## Phase 9: Cron Jobs & Automation (Week 10-11)

### 9.1 Cron (Scheduled Tasks)
```bash
crontab -e                  # Edit cron jobs
crontab -l                  # List cron jobs

# Format: MIN HOUR DAY MONTH WEEKDAY COMMAND

# Examples:
0 2 * * * /opt/backup.sh           # Daily 2 AM
*/5 * * * * /opt/health.sh         # Every 5 minutes
0 0 * * 0 /opt/weekly.sh           # Every Sunday midnight
0 6,18 * * * /opt/report.sh        # 6 AM and 6 PM daily
```

### 9.2 Basic Shell Scripting
```bash
#!/bin/bash
# Script: system_info.sh

echo "=== System Info ==="
echo "Hostname: $(hostname)"
echo "IP Address: $(ip addr show | grep inet | head -2)"
echo "Uptime: $(uptime)"
echo "Disk Usage:"
df -h
echo "Memory Usage:"
free -h
echo "Logged in Users:"
who
```

```bash
chmod +x system_info.sh    # Make executable
./system_info.sh            # Run it
```

### 9.3 Scripting Concepts to Learn
- [ ] Variables: `NAME="John"`
- [ ] Read input: `read -p "Enter name: " NAME`
- [ ] If-else: `if [ condition ]; then ... fi`
- [ ] For loop: `for i in 1 2 3; do echo $i; done`
- [ ] While loop: `while [ condition ]; do ... done`
- [ ] Functions: `function greet() { echo "Hello"; }`
- [ ] Exit codes: `$?` (0=success, non-zero=fail)

---

## Phase 10: Beginner Projects 🏗️

### Project 1: LAMP Stack Setup
- [ ] Install Apache (`httpd`)
- [ ] Install MariaDB (`mariadb-server`)
- [ ] Install PHP (`php`)
- [ ] Deploy a sample webpage
- [ ] Open firewall port 80
- [ ] Enable services on boot
- [ ] Test from browser

### Project 2: SSH Hardened Server
- [ ] Create a new user with sudo access
- [ ] Generate SSH keys
- [ ] Disable root login
- [ ] Disable password authentication
- [ ] Change SSH port
- [ ] Configure firewall for new port
- [ ] Install & configure Fail2ban
- [ ] Document everything

### Project 3: Automated Backup System
- [ ] Write a bash script to backup `/etc` and `/home`
- [ ] Compress with tar + gzip
- [ ] Name backup with date: `backup-2024-01-15.tar.gz`
- [ ] Store in `/opt/backups/`
- [ ] Delete backups older than 7 days
- [ ] Schedule with cron (daily 2 AM)
- [ ] Log output to `/var/log/backup.log`

### Project 4: System Monitoring Script
- [ ] Check CPU usage
- [ ] Check Memory usage
- [ ] Check Disk usage
- [ ] Check if critical services are running (httpd, sshd)
- [ ] If disk > 80% → log a warning
- [ ] If service is down → restart it + log
- [ ] Run every 5 minutes via cron

### Project 5: Multi-User Server Setup
- [ ] Create 5 users (dev1, dev2, qa1, qa2, admin1)
- [ ] Create groups (developers, qa, admins)
- [ ] Assign users to groups
- [ ] Create shared directories per group
- [ ] Set proper permissions (group can read/write, others cannot)
- [ ] Configure sudo for admins group only
- [ ] Test access with each user

### Project 6: Firewall & Security Audit
- [ ] List all running services
- [ ] Disable unnecessary services
- [ ] Configure firewall (allow only SSH, HTTP, HTTPS)
- [ ] Check open ports with `ss -tulnp`
- [ ] Check failed login attempts from logs
- [ ] Check for users with empty passwords
- [ ] Check files with SUID bit set
- [ ] Write a report of findings

---

## 🗺️ Visual Roadmap

```
START HERE
    │
    ▼
┌─────────────────────┐
│  Phase 1: Basics    │  ← Commands, Navigation, Vim
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 2: Users &   │  ← Users, Groups, Permissions
│  Permissions        │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 3: Packages  │  ← dnf, apt, rpm
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 4: Services  │  ← systemctl, processes
│  & Processes        │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 5: Networking│  ← IP, SSH, DNS, ports
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 6: Storage   │  ← Disks, LVM, fstab
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 7: Firewall  │  ← firewalld, SELinux
│  & Security         │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 8: Logs      │  ← /var/log, journalctl, grep
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 9: Cron &    │  ← Cron jobs, Bash scripting
│  Scripting          │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Phase 10: Projects │  ← Build & add to resume
└─────────────────────┘
         │
         ▼
      APPLY FOR
        JOBS
```

____
