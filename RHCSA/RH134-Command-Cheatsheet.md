---
title: RH134 Command Cheatsheet
tags:
  - rh134
  - commands
  - reference
created: 2026-07-29
---

# RH134 Command Cheatsheet
**Red Hat System Administration II (RHEL 10)**

> Quick reference for the most important commands covered in the course. Update this as you learn.

---

## Shell & Bash Scripting

| Command / Syntax                  | Description                              | Example |
|-----------------------------------|------------------------------------------|-------|
| `export VAR=value`                | Set environment variable                 | `export EDITOR=vim` |
| `env`                             | Show all environment variables           | `env \| grep PATH` |
| `source file` / `. file`          | Load variables from script               | `. ~/.bashrc` |
| `#!/bin/bash`                     | Shebang                                  | - |
| `$1`, `$@`, `$#`                  | Positional parameters                    | `echo $1` |
| `read var`                        | Read user input                          | `read name` |
| `if [ condition ]; then ... fi`   | Conditional                              | `if [ $x -gt 10 ]; then` |
| `for i in {1..5}; do ... done`    | For loop                                 | - |
| `while [ condition ]; do ... done`| While loop                               | - |
| `case $var in ... esac`           | Case statement                           | - |
| `[[ ]]` vs `[ ]`                  | Modern vs POSIX test                     | - |

---

## Regular Expressions

| Tool     | Common Flags                  | Example |
|----------|-------------------------------|-------|
| `grep`   | `-E`, `-i`, `-v`, `-o`        | `grep -E '^[0-9]+' file` |
| `egrep`  | (same as grep -E)             | - |
| `sed`    | `-i`, `-E`, `s///`            | `sed -i 's/old/new/g' file` |
| `awk`    | `-F`, `'{print $1}'`          | `awk -F: '{print $1}' /etc/passwd` |

**Useful regex patterns:**
- `^` start, `$` end
- `[0-9]`, `[a-z]`, `[^ ]`
- `.*`, `+`, `?`, `{2,4}`
- `\b` word boundary

---

## Scheduling

### `at`
```bash
at 14:30
at now + 2 hours
atq
atrm <job>
```

### User `cron`
```bash
crontab -e
crontab -l
crontab -r
```

**Cron format:**
```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── day of week (0-6)
│ │ │ └───── month (1-12)
│ │ └─────── day of month (1-31)
│ └───────── hour (0-23)
└─────────── minute (0-59)
```

### System-wide cron
- `/etc/crontab`
- `/etc/cron.d/`
- `/etc/cron.hourly/`, `.daily`, `.weekly`, `.monthly`

### systemd Timers
```bash
systemctl list-timers
systemctl status myjob.timer
journalctl -u myjob.service
```

---

## Logging

```bash
journalctl -xe                    # Recent + errors
journalctl -u sshd                # Specific unit
journalctl -p err -b              # Errors since last boot
journalctl --since "1 hour ago"
journalctl -f                     # Follow (tail -f)
journalctl --list-boots
```

**Persistent journal:**
```bash
mkdir /var/log/journal
systemd-tmpfiles --create --prefix /var/log/journal
systemctl restart systemd-journald
```

**Time sync:**
```bash
chronyc sources
chronyc tracking
timedatectl
```

---

## SELinux

```bash
getenforce
setenforce 0          # Permissive (temporary)
setenforce 1          # Enforcing
sestatus

ls -Z file
ls -Zd /var/www/html

chcon -t httpd_sys_content_t file
restorecon -Rv /var/www/html

semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"
restorecon -Rv /srv/web

getsebool -a | grep httpd
setsebool -P httpd_enable_homedirs on

ausearch -m avc -ts recent
sealert -a /var/log/audit/audit.log
```

---

## Archiving & Transfer

```bash
# Tar + compression
tar -czvf archive.tar.gz /path
tar -xzvf archive.tar.gz
tar -tf archive.tar.gz

tar -cJf archive.tar.xz dir     # xz (best compression)
tar -c --zstd -f archive.tar.zst dir

# Transfer
scp file user@host:/path
sftp user@host
rsync -avz --progress /src/ user@host:/dest/
rsync -avz --delete /src/ /dest/   # Mirror
```

---

## Storage & LVM

```bash
lsblk
fdisk /dev/sdb
parted /dev/sdb

mkfs.xfs /dev/sdb1
mount /dev/sdb1 /mnt/data
blkid
/etc/fstab

# Swap
mkswap /dev/sdb2
swapon /dev/sdb2
swapon -s
free -h

# LVM
pvcreate /dev/sdb1
vgcreate vg_data /dev/sdb1
lvcreate -L 10G -n lv_data vg_data
mkfs.xfs /dev/vg_data/lv_data

lvextend -L +5G /dev/vg_data/lv_data
xfs_growfs /mnt/data          # or resize2fs for ext4
```

---

## Boot & Recovery

```bash
grubby --info=ALL
grubby --update-kernel=ALL --args="console=ttyS0"

systemctl get-default
systemctl set-default multi-user.target

# Emergency / rescue
systemctl rescue
systemctl emergency

# Boot repair
mount -o remount,rw /
passwd root
touch /.autorelabel
reboot
```

---

## Network & Firewall

```bash
firewall-cmd --state
firewall-cmd --list-all
firewall-cmd --add-service=http --permanent
firewall-cmd --reload

firewall-cmd --add-port=8080/tcp --permanent

# SELinux port labeling
semanage port -a -t http_port_t -p tcp 8080
```

---

## NFS & Automount

```bash
showmount -e server
mount -t nfs server:/share /mnt/nfs

# /etc/fstab
server:/share   /mnt/nfs   nfs   defaults,_netdev  0 0

# Automount
systemctl enable --now autofs
cat /etc/auto.master
cat /etc/auto.nfs
```

---

## Podman & Containers

```bash
podman --version
podman info

podman run -d --name web -p 8080:80 nginx
podman ps
podman logs web
podman exec -it web /bin/bash
podman stop web
podman rm web

podman images
podman pull registry.access.redhat.com/ubi9/ubi
podman build -t myapp .

podman volume ls
podman network ls

# Systemd integration
podman generate systemd --name web > ~/.config/systemd/user/container-web.service
systemctl --user daemon-reload
systemctl --user enable --now container-web
```

---

## Performance Tuning

```bash
tuned-adm list
tuned-adm active
tuned-adm profile throughput-performance

ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head
top / htop
nice -n 10 command
renice -n -5 PID
```

---

## Useful One-liners & Tips

```bash
# Find large files
find / -type f -size +100M 2>/dev/null | head -20

# Check disk usage
du -sh /* | sort -h

# Last 50 log lines with errors
journalctl -p 3..4 -n 50

# List all services
systemctl list-units --type=service

# Check SELinux denials quickly
ausearch -m avc -ts recent | tail -5
```

---

## Quick Reference Links (add your own)

- Red Hat Documentation: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/10
- `man` pages + `tldr` tool (highly recommended)

---

**Tip:** Use Obsidian's `[[links]]` or embed this file in chapter notes. Add new commands under the relevant section as you discover them.