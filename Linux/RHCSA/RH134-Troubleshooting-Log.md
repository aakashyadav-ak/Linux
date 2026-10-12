---
title: RH134 Troubleshooting Log
tags:
  - rh134
  - troubleshooting
  - errors
---

# RH134 Troubleshooting Log

> Log every error, stuck point, or unexpected behavior you encounter. This is one of the most valuable learning tools.

**Format:**
- Date
- Chapter / Topic
- Error / Symptom
- What you tried
- Root cause / Solution
- Command(s) that helped
- Notes / Prevention

---

## Entry Template

**Date:**  
**Chapter:**  
**Topic:**  

**Error / Symptom:**  
```
Paste exact error here
```

**What I tried:**  
- 

**Solution / Root Cause:**  
- 

**Commands that worked:**  
```bash

```

**Prevention / Lesson Learned:**  
- 

---

## Log Entries

### 2026-07-29 - Example Entry

**Chapter:** 6 - SELinux  
**Topic:** File context not applying  

**Error / Symptom:**  
```
httpd: Could not open configuration file /etc/httpd/conf/httpd.conf: Permission denied
```

**What I tried:**  
- `chcon -t httpd_sys_content_t /var/www/html`
- `setenforce 0` (temporary)

**Solution / Root Cause:**  
Used `semanage fcontext` + `restorecon`. The context was not persistent because the policy mapping was missing.

**Commands that worked:**  
```bash
semanage fcontext -a -t httpd_sys_content_t "/var/www/html(/.*)?"
restorecon -Rv /var/www/html
```

**Prevention / Lesson Learned:**  
Always use `semanage` for permanent changes. Check with `ls -Z` and `sealert`.

---

## Common Patterns (fill as you discover)

| Error Type             | Typical Cause                  | Quick Fix Command                          |
|------------------------|--------------------------------|--------------------------------------------|
| Permission denied      | Wrong SELinux context          | `restorecon -Rv` + `semanage fcontext`     |
| Service won't start    | Port not allowed               | `firewall-cmd` + `semanage port`           |
| LVM resize not visible | Forgot to grow filesystem      | `xfs_growfs` or `resize2fs`                |
| Boot fails             | Wrong root password or fstab   | `systemctl rescue` + `touch /.autorelabel` |
| Container can't bind   | Port conflict / SELinux        | Check `podman ps -a` + port labeling       |

---

**Tip:** Search this file using `Ctrl+Shift+F` in Obsidian when you hit a similar problem later.