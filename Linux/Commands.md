| Category    | Command    | Use                           | Example                          |                             |
| ----------- | ---------- | ----------------------------- | -------------------------------- | --------------------------- |
| Navigation  | `pwd`      | Show current directory path   | `pwd`                            |                             |
| Navigation  | `ls`       | List files/folders            | `ls -la`                         |                             |
| Navigation  | `cd`       | Change directory              | `cd /var/log`                    |                             |
| Navigation  | `mkdir`    | Create folder                 | `mkdir -p recon/nmap/scans`      |                             |
| Files       | `touch`    | Create empty file             | `touch notes.txt`                |                             |
| Files       | `cat`      | View file content             | `cat notes.txt`                  |                             |
| Files       | `less`     | View large file (scroll)      | `less /etc/passwd`               |                             |
| Files       | `head`     | Show first lines              | `head -n 20 scan.txt`            |                             |
| Files       | `tail`     | Show last lines               | `tail -n 20 scan.txt`            |                             |
| Files       | `tail -f`  | Live log monitoring           | `tail -f /var/log/auth.log`      |                             |
| Search      | `grep`     | Search inside files           | `grep -Ri "password" .`          |                             |
| Search      | `find`     | Find files by name            | `find /var/www -name "*.php"`    |                             |
| Search      | `which`    | Locate tool path              | `which nmap`                     |                             |
| Search      | `locate`   | Fast file search (DB based)   | `locate id_rsa`                  |                             |
| Copy/Move   | `cp`       | Copy files/folders            | `cp scan.txt backup.txt`         |                             |
| Copy/Move   | `cp -r`    | Copy folders                  | `cp -r loot loot_backup`         |                             |
| Copy/Move   | `mv`       | Move/rename                   | `mv report.txt final_report.txt` |                             |
| Delete      | `rm`       | Delete file                   | `rm temp.txt`                    |                             |
| Delete      | `rm -r`    | Delete folder                 | `rm -r testfolder`               |                             |
| Permissions | `whoami`   | Show current user             | `whoami`                         |                             |
| Permissions | `id`       | Show user + groups            | `id`                             |                             |
| Permissions | `sudo -i`  | Switch to root                | `sudo -i`                        |                             |
| Permissions | `chmod`    | Change permissions            | `chmod +x exploit.sh`            |                             |
| Permissions | `chown`    | Change owner                  | `sudo chown ak:ak report.txt`    |                             |
| Output      | `>`        | Overwrite output to file      | `nmap 10.0.0.1 > nmap.txt`       |                             |
| Output      | `>>`       | Append output to file         | `echo "done" >> notes.txt`       |                             |
| Output      | `          | `                             | Pipe output to another command   | `cat nmap.txt \| grep open` |
| Networking  | `ip a`     | Show IP address               | `ip a`                           |                             |
| Networking  | `ping`     | Check connectivity            | `ping -c 4 8.8.8.8`              |                             |
| Networking  | `curl`     | Send web request              | `curl -I https://example.com`    |                             |
| Networking  | `wget`     | Download file                 | `wget http://site.com/file.zip`  |                             |
| Networking  | `ssh`      | Remote login                  | `ssh user@192.168.1.10`          |                             |
| Processes   | `ps aux`   | Show running processes        | `ps aux`                         |                             |
| Processes   | `top`      | Live system usage             | `top`                            |                             |
| Processes   | `kill`     | Kill process                  | `kill -9 1234`                   |                             |
| System      | `uname -a` | Kernel/system info            | `uname -a`                       |                             |
| System      | `df -h`    | Disk usage                    | `df -h`                          |                             |
| System      | `du -sh`   | Folder size                   | `du -sh loot/`                   |                             |
| Archives    | `tar`      | Extract tar files             | `tar -xvzf tools.tar.gz`         |                             |
| Archives    | `unzip`    | Extract zip                   | `unzip file.zip`                 |                             |
| History     | `history`  | Show command history          | `history`                        |                             |
| History     | `!!`       | Repeat last command           | `!!`                             |                             |
| History     | `!number`  | Run history command by number | `!55`                            |                             |


## Navigation & Basic Commands

| Command | What It Does | Example |
|---------|--------------|---------|
| `pwd` | Shows which folder you are in right now | `pwd` → Output: `/home/kali` |
| `cd folder` | Go inside a folder | `cd /var/log` → Goes to log folder |
| `cd ..` | Go back one folder (parent folder) | `cd ..` → If in /home/kali, goes to /home |
| `cd ~` | Go to your home folder directly | `cd ~` → Goes to /home/kali |
| `cd -` | Go to the folder you were in before | `cd -` → Switches back to previous folder |
| `ls` | List files in current folder | `ls` → Shows files and folders |
| `ls -la` | List ALL files with details (including hidden) | `ls -la` → Shows permissions, owner, size, hidden files |
| `clear` | Clear the terminal screen | `clear` → Empty screen |

---

## File Operations

| Command | What It Does | Example |
|---------|--------------|---------|
| `cat file` | Display entire file content on screen | `cat /etc/passwd` → Shows all users |
| `head file` | Show first 10 lines of file | `head /etc/passwd` → First 10 users |
| `head -n 20 file` | Show first 20 lines of file | `head -n 20 log.txt` → First 20 lines |
| `tail file` | Show last 10 lines of file | `tail /var/log/auth.log` → Recent logins |
| `tail -f file` | Watch file updates live (real-time) | `tail -f /var/log/syslog` → See new logs appear |
| `less file` | View large file page by page (q to quit) | `less bigfile.txt` → Navigate with arrows |
| `touch file` | Create empty file | `touch notes.txt` → Creates notes.txt |
| `mkdir folder` | Create new folder | `mkdir tools` → Creates tools folder |
| `cp source dest` | Copy file to new location | `cp file.txt /tmp/` → Copies to /tmp |
| `cp -r folder dest` | Copy entire folder | `cp -r tools/ /tmp/` → Copies folder |
| `mv old new` | Move or rename file/folder | `mv old.txt new.txt` → Renames file |
| `rm file` | Delete file (cannot undo!) | `rm unwanted.txt` → Deletes file |
| `rm -r folder` | Delete folder and everything inside | `rm -r oldfolder/` → Deletes folder |

---

## Searching & Finding

| Command | What It Does | Example |
|---------|--------------|---------|
| `grep "word" file` | Find lines containing a word in file | `grep "error" log.txt` → Shows lines with "error" |
| `grep -i "word" file` | Find word (ignore uppercase/lowercase) | `grep -i "password" file.txt` → Finds Password, PASSWORD, etc. |
| `grep -r "word" folder` | Search for word in all files inside folder | `grep -r "password" /etc/` → Search entire /etc folder |
| `find / -name "file"` | Find file by name anywhere on system | `find / -name "passwd"` → Finds all files named passwd |
| `find / -name "*.txt"` | Find all files ending with .txt | `find /home -name "*.txt"` → All txt files in /home |
| `find / -perm -4000` | Find SUID files (privilege escalation!) | `find / -perm -4000 2>/dev/null` → SUID binaries |
| `which command` | Find where a command is located | `which python` → /usr/bin/python |
| `locate filename` | Quick search (uses database) | `locate password.txt` → Fast search |

---

## User & Permission Info

| Command | What It Does | Example |
|---------|--------------|---------|
| `whoami` | Shows your current username | `whoami` → Output: kali |
| `id` | Shows your user ID, group ID, and all groups | `id` → uid=1000(kali) gid=1000(kali) groups=... |
| `sudo command` | Run command as root (admin) | `sudo cat /etc/shadow` → View password hashes |
| `sudo -l` | List what commands you can run as sudo | `sudo -l` → Shows your sudo powers |
| `su -` | Switch to root user (need root password) | `su -` → Become root |
| `chmod +x file` | Make file executable (can run as program) | `chmod +x script.sh` → Now can run ./script.sh |
| `chmod 755 file` | Set permission: owner full, others read+run | `chmod 755 script.sh` → Common for scripts |
| `chmod 600 file` | Set permission: only owner can read/write | `chmod 600 id_rsa` → Protect private key |
| `chown user file` | Change file owner | `sudo chown kali file.txt` → kali now owns it |

---

## System Information

| Command | What It Does | Example |
|---------|--------------|---------|
| `uname -a` | Shows kernel version and system info | `uname -a` → Linux kali 5.10.0 x86_64 |
| `cat /etc/os-release` | Shows which Linux distribution | `cat /etc/os-release` → Kali, Ubuntu, etc. |
| `hostname` | Shows computer name | `hostname` → kali |
| `uptime` | Shows how long system has been running | `uptime` → up 2 days, 5:30 |
| `df -h` | Shows disk space usage (human readable) | `df -h` → Shows GB/MB used and free |
| `free -h` | Shows RAM usage | `free -h` → Total, used, free memory |
| `ps aux` | Shows all running processes | `ps aux` → List of all programs running |
| `ps aux \| grep name` | Find specific process | `ps aux \| grep apache` → Find apache process |
| `top` | Live view of processes and CPU usage | `top` → Press q to quit |
| `kill PID` | Stop a process by its ID number | `kill 1234` → Stops process 1234 |
| `kill -9 PID` | Force stop a process (when normal kill fails) | `kill -9 1234` → Force kills process |

---

## Network Commands

| Command | What It Does | Example |
|---------|--------------|---------|
| `ip a` | Shows all network interfaces and IP addresses | `ip a` → Your IP, MAC address, etc. |
| `ip route` | Shows default gateway (router IP) | `ip route` → default via 192.168.1.1 |
| `ping -c 4 host` | Test if you can reach another computer | `ping -c 4 google.com` → 4 ping packets |
| `ss -tulnp` | Shows all open ports and what's listening | `ss -tulnp` → See port 22 SSH, port 80 HTTP, etc. |
| `netstat -tulnp` | Same as above (older command) | `netstat -tulnp` → Open ports |
| `curl URL` | Get webpage content or make HTTP request | `curl http://example.com` → Downloads page |
| `curl -I URL` | Get only HTTP headers | `curl -I http://example.com` → Server info |
| `wget URL` | Download file from internet | `wget http://site.com/file.zip` → Downloads file |
| `nslookup domain` | Find IP address of domain name | `nslookup google.com` → Shows IP |
| `dig domain` | DNS lookup (more detailed) | `dig google.com` → DNS records |

---

## SSH & Remote Access

| Command | What It Does | Example |
|---------|--------------|---------|
| `ssh user@host` | Connect to remote computer | `ssh root@192.168.1.100` → Remote login |
| `ssh -p port user@host` | Connect on different port | `ssh -p 2222 admin@host` → Port 2222 |
| `ssh -i keyfile user@host` | Connect using private key file | `ssh -i id_rsa john@server` → Key-based login |
| `scp file user@host:/path` | Copy file TO remote computer | `scp tool.sh root@192.168.1.100:/tmp/` |
| `scp user@host:/file ./` | Copy file FROM remote computer | `scp root@host:/etc/passwd ./` → Download file |
| `scp -r folder user@host:/path` | Copy entire folder to remote | `scp -r tools/ user@host:/home/` |

---

## Archive & Compression

| Command | What It Does | Example |
|---------|--------------|---------|
| `tar -czvf name.tar.gz folder` | Create compressed archive (zip-like) | `tar -czvf backup.tar.gz /home/` → Creates backup |
| `tar -xzvf file.tar.gz` | Extract compressed archive | `tar -xzvf backup.tar.gz` → Extracts files |
| `tar -tvf file.tar.gz` | List contents without extracting | `tar -tvf backup.tar.gz` → See what's inside |
| `zip -r name.zip folder` | Create zip archive | `zip -r files.zip documents/` |
| `unzip file.zip` | Extract zip archive | `unzip files.zip` |

---

## Important Files to Check (VAPT)

| Command | What It Shows | Why Important |
|---------|---------------|---------------|
| `cat /etc/passwd` | All user accounts on system | Find users to target |
| `cat /etc/shadow` | Password hashes (need root) | Crack passwords offline |
| `cat /etc/group` | All groups and members | Find privileged groups |
| `cat /etc/sudoers` | Who can use sudo and how | Find privilege escalation paths |
| `cat /etc/crontab` | Scheduled tasks | Find writable scripts that run as root |
| `cat /etc/hosts` | Local DNS mappings | Find internal hostnames |
| `cat /etc/ssh/sshd_config` | SSH server settings | Find misconfigurations |
| `cat ~/.bash_history` | Command history of user | Find passwords typed by mistake |
| `cat ~/.ssh/id_rsa` | Private SSH key | Use for lateral movement |
| `ls -la /home/` | All user home directories | Find interesting user files |

---

## VAPT One-Liners (Most Important!)

| Command | What It Does |
|---------|--------------|
| `find / -perm -4000 -type f 2>/dev/null` | Find SUID binaries (privilege escalation) |
| `find / -perm -o+w -type d 2>/dev/null` | Find world-writable directories |
| `find / -name "id_rsa" 2>/dev/null` | Find SSH private keys |
| `find / -name "*.conf" 2>/dev/null` | Find configuration files |
| `find / -name "*password*" 2>/dev/null` | Find files with "password" in name |
| `grep -r "password" /etc/ 2>/dev/null` | Search for password strings in /etc |
| `getcap -r / 2>/dev/null` | Find files with capabilities (priv esc) |
| `cat /etc/passwd \| grep bash` | Find users with shell access |
| `awk -F: '$3 == 0 {print $1}' /etc/passwd` | Find users with root privileges (UID 0) |
| `ss -tulnp \| grep LISTEN` | Find all listening services |

---

## Output Redirection

| Symbol | What It Does | Example |
|--------|--------------|---------|
| `>` | Save output to file (overwrites) | `ls > files.txt` → Saves list to file |
| `>>` | Add output to file (appends) | `echo "new line" >> file.txt` |
| `\|` | Send output to another command | `cat file \| grep password` → Filter output |
| `2>/dev/null` | Hide error messages | `find / -name x 2>/dev/null` → No errors shown |
| `&>` | Save both output and errors | `command &> all_output.txt` |

---

## Service Management

| Command | What It Does | Example |
|---------|--------------|---------|
| `systemctl status service` | Check if service is running | `systemctl status ssh` |
| `systemctl start service` | Start a service | `sudo systemctl start apache2` |
| `systemctl stop service` | Stop a service | `sudo systemctl stop apache2` |
| `systemctl restart service` | Restart a service | `sudo systemctl restart ssh` |
| `systemctl enable service` | Make service start at boot | `sudo systemctl enable ssh` |

---

## Package Installation (Kali/Ubuntu)

| Command | What It Does | Example |
|---------|--------------|---------|
| `sudo apt update` | Refresh package list | `sudo apt update` → Gets latest info |
| `sudo apt install package` | Install software | `sudo apt install nmap` |
| `sudo apt remove package` | Uninstall software | `sudo apt remove nmap` |
| `apt search name` | Search for package | `apt search nikto` |
| `dpkg -l` | List all installed packages | `dpkg -l \| grep nmap` → Check if installed |

---

## Quick Tips

| Shortcut | What It Does |
|----------|--------------|
| `Tab` | Auto-complete file/command names |
| `Ctrl + C` | Stop current running command |
| `Ctrl + Z` | Pause current command (use `fg` to resume) |
| `Ctrl + L` | Clear screen (same as `clear`) |
| `Ctrl + R` | Search command history |
| `Up Arrow` | Previous command |
| `!!` | Run last command again |
| `sudo !!` | Run last command with sudo |

---

## Permission Numbers Cheat

| Number | Permission | Meaning |
|--------|------------|---------|
| `7` | rwx | Read + Write + Execute |
| `6` | rw- | Read + Write |
| `5` | r-x | Read + Execute |
| `4` | r-- | Read only |
| `0` | --- | No permission |

**Common combinations:**
- `755` = Owner can do everything, others can read/run
- `644` = Owner can read/write, others can only read  
- `600` = Only owner can read/write (private files)
- `777` = Everyone can do everything (dangerous!)

---

## Top 15 Commands for VAPT Beginners

| # | Command | Why Use It |
|---|---------|------------|
| 1 | `id` | First thing to run - know who you are |
| 2 | `sudo -l` | Check what you can run as root |
| 3 | `find / -perm -4000 2>/dev/null` | Find SUID binaries for priv esc |
| 4 | `cat /etc/passwd` | See all users |
| 5 | `cat /etc/shadow` | Get password hashes (if root) |
| 6 | `ss -tulnp` | Find open ports and services |
| 7 | `ps aux` | See running processes |
| 8 | `uname -a` | Get system info for exploits |
| 9 | `cat /etc/crontab` | Find scheduled tasks |
| 10 | `history` | See what user typed before |
| 11 | `cat ~/.ssh/id_rsa` | Find SSH keys |
| 12 | `getcap -r / 2>/dev/null` | Find capabilities |
| 13 | `ip a` | Get network info |
| 14 | `cat /etc/hosts` | Find internal hostnames |
| 15 | `ls -la /home/` | Explore user directories |