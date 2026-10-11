# Linux for SOC Analyst -- Interview Notes (Fresher)

------------------------------------------------------------------------

# 1. Linux Basics

## Linux Directory Structure (Security Perspective)

  -----------------------------------------------------------------------
  Directory                        
  -------------------------------- --------------------------------------
  /etc                             Configuration files (users, services,
                                   sudo, network configs). Critical
                                   during investigation.

  /var                             Logs stored in /var/log/ (auth logs,
                                   syslog, secure logs).

  /home                            User directories. Check for suspicious
                                   files, SSH keys.

  /root                            Root user's home. High-value target.

  /opt                             Optional software (3rd-party apps).

  /tmp                             World-writable. Malware often drops
                                   payloads here. Sticky bit enabled.
  -----------------------------------------------------------------------

## Root vs Normal User

-   Root: UID 0, full privileges.
-   Normal user: Limited permissions.
-   SOC relevance:
    -   Privilege escalation detection.
    -   Check unauthorized root access.
    -   Monitor sudo usage.

## File Permissions (rwx)

-   r = read
-   w = write
-   x = execute

Example: -rwxr-xr--

SOC Use: - Identify overly permissive files (e.g., 777). - Detect
writable config files.

## Ownership Concept

-   User (owner)
-   Group
-   Others

Important for: - Detecting misconfigurations. - Privilege abuse
detection.

## Hidden Files

-   Start with .
-   Example: .bash_history, .ssh

SOC relevance: - Attackers hide backdoors. - Check hidden persistence
files.

------------------------------------------------------------------------

# 2. File & Directory Commands (SOC Usage)

ls -la → View permissions, hidden files\
cd → Navigate investigation path\
pwd → Confirm location\
mkdir → Create test dirs\
touch → Create files\
cp → Backup suspicious file\
mv → Rename suspicious file\
rm -rf → Can destroy evidence (use carefully)\
cat → View file content\
less → Analyze logs safely\
head/tail → Check start/end of logs\
find → Search files, SUID, writable files\
locate → Quick search\
which → Find command path\
whereis → Locate binary, source, manual

------------------------------------------------------------------------

# 3. Text Processing (Very Important for SOC)

## grep

grep "Failed password" /var/log/auth.log

Used for: - Brute force detection - Suspicious IP search

Options: - -i ignore case - -r recursive - -n line number

## cut

Extract columns from logs.

## awk

Used for log parsing: awk '{print \$1}' file

## sed

Modify or filter logs.

## sort & uniq

Detect brute force: grep "Failed password" auth.log \| awk '{print
\$11}' \| sort \| uniq -c \| sort -nr

## wc

Count lines (number of login attempts).

## tr

Replace characters in logs.

## Redirection

> overwrite\
> \> append\
> 2\>/dev/null hide errors

------------------------------------------------------------------------

# 4. Users & Privileges

id → Check UID, GID, groups\
whoami → Current logged-in user\
groups → Check group membership

## sudo Basics

sudo -l

SOC relevance: - Misconfigured sudo rules. - Privilege escalation
detection.

## Important Files

/etc/passwd → username, UID, home directory, shell\
/etc/shadow → password hashes (root only)

SOC relevance: - Detect new backdoor users.

## su vs sudo

su → switches user\
sudo → runs command as root

------------------------------------------------------------------------

# 5. Permissions (Critical for SOC)

## Numeric Permissions

777 → Full access (dangerous)\
755 → Common for directories\
644 → Common for files\
600 → Sensitive files (SSH keys)

chmod → Change permissions\
chown → Change owner\
chgrp → Change group

## Special Bits

SUID: find / -perm -4000 2\>/dev/null

Privilege escalation vector.

SGID → Affects group ownership\
Sticky Bit → Used on /tmp

------------------------------------------------------------------------

# 6. Processes & Services

ps aux → List running processes\
top / htop → Monitor CPU, memory spikes\
kill / kill -9 → Terminate malicious process

systemctl status ssh → Check service status

Look for: - Unknown services enabled at boot. - Suspicious processes.

------------------------------------------------------------------------

# 7. Package Management

Debian: apt update\
apt install\
dpkg -l

RHEL: yum\
dnf\
rpm -qa

SOC relevance: - Detect suspicious installed tools.

------------------------------------------------------------------------

# 8. Networking (Very Important for SOC)

ip a → Check IP address\
ip r → Routing table\
ss -tulnp → Check open ports with process\
ping → Connectivity test\
traceroute → Trace path\
nslookup / dig → DNS investigation\
curl / wget → Test web services\
arp -a → Check ARP cache

------------------------------------------------------------------------

# 9. Firewall Basics

iptables → Packet filtering rules\
ufw → Ubuntu firewall\
firewalld → RHEL firewall

SOC monitors: - Suspicious port opening.

------------------------------------------------------------------------

# 10. Important Files for SOC

/etc/sudoers\
/etc/hosts\
/etc/resolv.conf\
/etc/crontab\
/var/log/auth.log\
/var/log/syslog\
/var/log/secure\
/home/\*/.ssh/authorized_keys\
/root/.ssh/

------------------------------------------------------------------------

# 11. Enumeration Checklist (SOC Investigation Flow)

1.  id\
2.  sudo -l\
3.  ps aux\
4.  ss -tulnp\
5.  crontab -l\
6.  find / -writable 2\>/dev/null\
7.  find / -perm -4000\
8.  getcap -r /\
9.  dpkg -l / rpm -qa\
10. uname -a

------------------------------------------------------------------------

# 12. Logs & Monitoring

journalctl\
dmesg\
tail -f

SOC Use: - Detect brute force. - Detect service crashes. - Detect kernel
exploit attempts.

------------------------------------------------------------------------

# 13. SSH Basics

ssh user@ip

Key-Based Authentication: - Public key → server - Private key → client -
chmod 600 id_rsa

Monitor: - Unauthorized key insertion. - Suspicious SSH login times.

------------------------------------------------------------------------

# 14. Shell Basics

echo \$PATH\
export VAR=value\
history\
alias\
chmod +x script.sh\
./script.sh\
#!/bin/bash

------------------------------------------------------------------------

# 15. Resource Monitoring

df -h\
du -sh\
free -m\
ps\
top

SOC use: - Detect crypto miners. - Detect disk-filling attacks.

------------------------------------------------------------------------

# 16. Bonus Topics

cron → Persistence method\
tar / gzip → Log backups & packaging\
tmux / screen → Maintained sessions\
Basic Bash scripting → Automation

------------------------------------------------------------------------

# Final Interview Tip

Focus on: - Privilege escalation - Persistence - Lateral movement - Log
analysis - Misconfigurations
