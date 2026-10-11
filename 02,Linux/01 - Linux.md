---
tags: [rhel, rhcsa, rh124, linux, rhel10, intro]
book: RH124 - Red Hat System Administration I (RHEL 10.0)
---

# 00 Introduction: Linux Basics + About My Book

> [!info] How this note is built
> **(book)** = in my RH124 PDF. **(my notes)** = my own Linux notes, corrected. **(extra)** = added from outside knowledge. Test **(extra)** items on my VM.

---

# Part A: About my book (RH124)

## A1. Book details (book)
| Item | Value |
|---|---|
| Title | **Red Hat System Administration I (RH124)** |
| Version | Red Hat Enterprise Linux **10.0**, Student Workbook, **Edition 3** |
| Publication date | **2025-08-21** (code `RH124-RHEL10.0-en-3-20250821`) |
| Length | **714 pages**, 20 chapters |
| Next course | **RH134** (Red Hat System Administration II) |

- **Made for:** IT professionals with **no previous Linux admin experience**. It teaches "survival skills" for core admin tasks.
- **Course objective:** operate, manage and maintain an existing RHEL system **from the command line**.
- **Audience:** sysadmins, platform engineers, developers who must do essential tasks on a **single server**: Bash, users and files, software, local storage.
- **Prerequisite:** basic computer use. Linux experience helps but is not required.

> [!tip] How it fits RHCSA
> RH124 + RH134 together map to the RHCSA exam (EX200). RH124 is the foundation, so I do not skip it.

## A2. What is in my book (chapter map and my progress)

| Ch | Topic | Sections | My notes |
|---|---|---|---|
| 1 | Introduction to RHEL | Getting started, Quiz, Summary | ✅ done |
| 2 | Accessing the Command Line | Text interface, Quiz, Desktop environment, Guided exercise, Bash shell, Quiz, Lab, Summary | ✅ done |
| 3 | Getting Help from Local Documentation | Manual pages, Guided exercise, Lab, Summary | 🟡 partial (PDF stopped inside the lab) |
| 4 | Registering Systems for Red Hat Support | Registering, Quiz, Summary | ⬜ |
| 5 | AI-assisted Help: RHEL **Lightspeed** | Command-line assistant questions, Guided exercise, Summary | ⬜ |
| 6 | Navigating the File-system Hierarchy | Hierarchy, Quiz, Specifying files by name, Quiz, Summary | ✅ done |
| 7 | Managing Files from the Command Line | CLI tools, Guided exercise, **Links**, Guided exercise, **Shell expansions**, Quiz, Lab, Summary | ⬜ |
| 8 | Editing Text Files | Editing from the CLI, Guided exercise, Summary | ⬜ |
| 9 | Redirecting Shell Input and Output | Redirect to files and programs, Guided exercise, Quiz, Summary | ⬜ |
| 10 | Managing Local Users and Groups | Users and groups, Quiz, **Superuser access**, User accounts, Group accounts, **Passwords**, Guided exercises, Lab, Summary | ⬜ |
| 11 | Controlling Access to Files | **Permissions**, Quiz, Managing permissions, **Default and special permissions**, Guided exercises, Lab, Summary | ⬜ |
| 12 | Installing and Updating Software with RPM | RPM packages, **DNF**, **Enabling repositories**, Guided exercises, Lab, Summary | ⬜ |
| 13 | Applications by Using **Flatpak** | Configuring Flatpak, Managing applications, Guided exercises, Quiz, Summary | ⬜ |
| 14 | Accessing Removable Media | File systems and block devices, Quiz, **Mounting/unmounting**, Locating files, Guided exercises, Lab, Summary | ⬜ |
| 15 | Monitoring and Managing Linux Processes | Process lifecycle, Quiz, **Job control**, **Signals**, **Monitoring**, Guided exercises, Lab, Summary | ⬜ |
| 16 | Controlling Services and Daemons | Auto-start processes, **Controlling services**, Guided exercises, Lab, Summary | ⬜ |
| 17 | Introduction to Networking | Concepts, Quiz, Validating configuration, Guided exercise, Summary | ⬜ |
| 18 | Managing Network Configuration | CLI networking, **Config files**, **Hostnames and name resolution**, Guided exercises, Lab, Summary | ⬜ |
| 19 | Configuring and Securing SSH | **Host keys**, **Key-based authentication**, Guided exercises, Lab, Summary | ⬜ |
| 20 | Comprehensive Review | Learning objectives, 5 labs (files; users/permissions/processes; server; networks; mount and find files), Summary | ⬜ |

> [!note] Why I started with Chapter 6 early
> Paths and the directory tree are used in every later chapter.

> [!warning] Page 19 of my upload
> Pages 1 to 18 were readable. Page 19 did not come through in what I sent. Re-send it if it has content.

## A3. How the book is written: conventions (book)
The book uses four boxes. I map them to Obsidian callouts:

| Book box | Meaning | Obsidian callout |
|---|---|---|
| **References** | Where to find outside documentation | `> [!quote]` or `> [!info]` |
| **Note** | Tips and shortcuts. Skipping one has no bad effect | `> [!note]` |
| **Important** | Easy-to-miss details (current-session-only changes, service restarts). Skipping causes frustration | `> [!important]` |
| **Warning** | Skipping it will most likely cause **data loss** | `> [!warning]` |

- The course has content that was **generated or assisted by AI tools**, with human authors and expert review.
- Red Hat is **reviewing its language** to remove potentially offensive terms.

## A4. The classroom environment (book)
The main machine is **workstation**. Three more machines: **servera**, **serverb**, **serverc**. All are in the DNS domain **`lab.example.com`**.

| Machine | IP address | Role |
|---|---|---|
| classroom / content / materials `.example.com` | 172.25.252.254, 172.25.254.254 | Hosts the classroom materials |
| utility.lab.example.com | 172.25.250.8 | Course support services |
| **workstation**.lab.example.com | 172.25.250.9 | Graphical machine students use |
| **servera**.lab.example.com | 172.25.250.10 | Managed server A |
| **serverb**.lab.example.com | 172.25.250.11 | Managed server B |
| **serverc**.lab.example.com | 172.25.250.12 | Managed server C |

- Student network: **172.25.250.0/24**.
- Normal workflow: log in to **workstation** only, then use **`ssh`** to reach the servers. This is practice for Chapter 19.

## A5. Two ways to run the book's labs (book)

### Instructor-led classroom
- You get a physical machine `foundationX.ilt.example.com`. Auto-login user: **`kiosk`**, password **`redhat`**.
- The classroom machines are virtual machines on that host.
- Run these as `kiosk` on the foundation machine:
```bash
  rht-vmctl start server      # start a VM
  rht-vmview view server      # open its console
  rht-vmctl reset server      # reset to initial course state and restart
```
- "Reset your servera" in a lab means `rht-vmctl reset servera`.

### Self-paced online (ROLE: Red Hat Online Learning)
- Website: **rol.redhat.com**, logged in with a **Red Hat Customer Portal** account.
- Controls are on the **Lab Environment** tab.

| VM state | Meaning |
|---|---|
| **building** | Being created |
| **active** | Running (services may still be starting) |
| **stopped** | Shut down, **disk state is kept** |

| Classroom button | Action |
|---|---|
| CREATE / CREATING | Make and start all VMs (takes minutes) |
| START / STARTING | Start all VMs |
| STOP | Stop all VMs |
| **DELETE** | Remove the whole classroom. **All saved work is lost** |

| Machine action | Action |
|---|---|
| **OPEN CONSOLE** | Opens the VM console in a browser tab |
| **Start** | Power on |
| **Shutdown** | Graceful shutdown, disk kept |
| **Power Off** | Forced off (like pulling the plug), disk kept |
| **Reset** | Forced off and **disk reset to start. All saved work lost** |

> [!warning] DELETE cannot be undone
> It removes all work in the classroom.

#### Timers (book)
- ROLE gives a limited time allowance, so it uses timers.
- **Auto-stop:** each **+** adds **1 hour**, maximum **11 hours**.
- **Auto-destroy:** each **+** adds **1 day**, maximum **14 days**.
- Keep the timers set while working, but do not set them needlessly high.

## A6. The `lab` command (book)
```bash
student@workstation:~$ lab action exercise
```
| Action | What it does |
|---|---|
| `install` | Installs the exercise files for a course or lesson |
| `select` | Picks a course or lesson when several are installed |
| `list` | Lists installed packages in the lab environment |
| **`start`** | Prepares the exercise (settings, resources, prerequisites) |
| **`grade`** | Checks my work. Shows PASS or FAIL per item. Fix and re-run. **Only end-of-chapter and comprehensive review labs** |
| **`finish`** | Cleans up after the exercise. I can repeat an exercise any time |

- Example: `lab start cli-desktop` then `lab finish cli-desktop`.
- Tab completion works: `lab start` then press `Tab` twice to list the exercises.
- Three activity types:
  - **Guided exercise**: step-by-step practice after a lesson.
  - **Quiz**: knowledge check.
  - **End-of-chapter lab**: gradable, high-level steps with a solution.

## A7. Practicing in my own VMware lab (extra)
My VMware setup has no `lab` command and no `rht-vmctl`. I build the same layout myself.

| Book machine | My VMware version |
|---|---|
| **workstation** | Main RHEL 10 VM, Server with GUI |
| **servera**, **serverb** | Clones of it (Server or Minimal), used from Chapter 15 onward for SSH and networking |

- Create the book's user: `sudo useradd student && sudo passwd student`, and add it to `wheel` for sudo.
- Set hostnames with `hostnamectl set-hostname servera.lab.example.com`.
- Use **NAT** or **Host-only** so the VMs can talk to each other. Add names to `/etc/hosts` until Chapter 18 (name resolution).
- **Take snapshots** before risky chapters (users, storage, networking). A snapshot is my own "Reset".
- Without `lab grade`, I check my work by running the commands and verifying the result.

---

# Part B: Linux basics (my notes, corrected)

## B1. What is Linux?
- Linux is a **free and open-source**, **Unix-like** operating system **kernel**, started by **Linus Torvalds in 1991**.
- Licence: **GPL v2** (extra).
- **Strictly:** "Linux" is the **kernel**. A complete OS = kernel + tools = a **distribution**.
- **GNU** project (1983, Richard Stallman) supplies many core tools, so people say **GNU/Linux** (extra).

> [!warning] Fix to my old note: "based on Unix"
> Linux was **written from scratch** and **copies Unix's design and behavior**. It is **Unix-like**, not made from Unix code.

### Why people use Linux
| My note | Corrected, simple version |
|---|---|
| FREE | The **kernel and most software are free**. **RHEL is a paid subscription** for support and updates (a free developer option exists) |
| OPEN SOURCE | Anyone can **see, change and share** the code |
| SECURE | Strong **multi-user permissions** and fast patching. **Not unhackable.** Security depends on how it is configured |
| STABLE | Servers can run for **very long periods** without a reboot (kernel updates still need one) |
| FAST | Light on resources, runs well on older hardware |

## B2. UNIX
- UNIX began in **1969** at **Bell Labs** (Ken Thompson, Dennis Ritchie) (extra).
- A powerful **multi-user, multitasking** operating system.

> [!warning] Fix to my old note: "Unix is closed source and paid"
> - **UNIX** is a **family and a trademark**. Many versions are **proprietary** (IBM AIX, Oracle Solaris, HP-UX).
> - Some Unix-family systems are **free**, such as **FreeBSD**, and **macOS** is certified UNIX.
> - Linux is **not** a UNIX product. It is **Unix-like**.

## B3. Linux architecture (layers)
```
Layer 4  User applications   Firefox, VS Code, Nmap, Docker, PostgreSQL
Layer 3  Shell / CLI         bash, zsh; commands like ls, cd, grep, ssh
Layer 2  System libraries    glibc, OpenSSL, libpcap
Layer 1  Linux kernel        processes, memory, drivers, filesystems, networking
Layer 0  Hardware            CPU, RAM, disk, NIC
```
- Requests go **down**: command → shell → **system call** → kernel → hardware. Results come back **up**.
- The **shell** is itself a user program. The layered view is a simplification.

### Kernel (heart of the OS)
- Talks **directly to hardware**.
- Manages: **processes** (scheduling), **memory** (RAM, virtual memory), **device drivers**, **system calls**, **security** (permissions), **networking**, **file systems**.
```bash
uname -r      # kernel version
uname -a      # all system info
```
> [!warning] Fix to my old note: "ext4, btrfs"
> RHEL's default is **XFS**, with **ext4** also supported. **Btrfs is not supported in RHEL.**

### Shell
- A program that is the **interface between me and the kernel**. It is a **command interpreter**: it reads `ls`, asks the OS to list files, and prints the result.
- Gives: command line, command execution, **scripting and automation**, history, tab completion.
- **Bash** (Bourne Again Shell) is the **default in RHEL**.

| Shell | Path | Description | Security note |
|---|---|---|---|
| **Bash** | `/bin/bash` | RHEL default | Most scripts and one-liners use it |
| **sh** | `/bin/sh` | POSIX shell. On RHEL it **points to bash** (extra) | Minimal environments often only have `sh` |
| **Zsh** | `/bin/zsh` | Enhanced, macOS default | More features |
| **Dash** | `/bin/dash` | **Debian/Ubuntu** minimal `/bin/sh`, **not** RHEL | Very small and fast |

```bash
cat /etc/shells      # shells allowed on the system
echo $SHELL          # my login shell
echo $0              # the shell running right now
ls -l /bin/sh        # (extra) see what sh points to
```

### File system
- How Linux **stores and organizes data** on SSD/HDD/USB.
- Controls: **files and folders**, **permissions** (read, write, execute), where system files live, how devices are **mounted**.
- Details: Chapter 6 note, [[Chapter 6 Notes]]. RHEL default type: **XFS**.

### Userspace
- Everything that runs **outside the kernel**: system tools, libraries and user programs.
- Kernel space = privileged. Userspace = limited. They talk through **system calls**.

## B4. Distribution (distro)
- **Distro = Linux kernel + GNU tools + package manager + (optional) desktop + pre-configured software + installer.**

### Corrected family tree
```
Red Hat family                      Debian family               Independent
------------------                  --------------              -----------
Fedora   (new features)             Debian                      Arch Linux
   ↓                                   ↓                         Slackware
CentOS Stream (preview of next RHEL)   Ubuntu                    Gentoo
   ↓                                   ↓
RHEL     (stable, supported)           Kali (Debian-based)
   ↓
Rocky Linux, AlmaLinux (free RHEL rebuilds)
```

> [!warning] Fix to my old tree and table
> - **Fedora → CentOS Stream → RHEL** is the correct order. CentOS Stream is **upstream** (a preview) of RHEL, **not based on RHEL**.
> - **Fedora is not below CentOS.** It comes **first**.
> - **RHEL is not "independent."** Its source line is Fedora → CentOS Stream.
> - **Kali is Debian-based**, not Ubuntu-based.

| Distro | Based on | Use |
|---|---|---|
| **RHEL** | Fedora → CentOS Stream | Enterprise servers, **10-year** support |
| **CentOS Stream** | Upstream of RHEL | Development and testing |
| **Rocky Linux / AlmaLinux** | RHEL rebuilds | Free RHEL-compatible |
| **Fedora** | Independent, upstream of RHEL | Latest features |
| **Ubuntu** | Debian | Desktop and server |
| **Kali Linux** | Debian | Penetration testing |

### Package managers
| Family | Package file | Tools |
|---|---|---|
| Debian / Ubuntu | **`.deb`** | `apt`, `dpkg` |
| RHEL / Fedora | **`.rpm`** | **`dnf`** (and `yum`, which is an alias of `dnf`), `rpm` |

> [!warning] Fix to my old note: "file extension .dnf"
> RHEL packages end in **`.rpm`**, not `.dnf`. **DNF** is the **tool** that installs them. Learned in Chapter 12.

## B5. RHEL
- **Red Hat Enterprise Linux**: a **commercial** Linux OS from **Red Hat** for servers, data centers and cloud.
- Gives **stability, security and long-term support (10 years)**, paid support and regular updates.
- Based on the Linux kernel. Source line: **Fedora → CentOS Stream → RHEL**.
- Free ways to learn: **Red Hat Developer Subscription**, or rebuilds like Rocky and AlmaLinux.

---

# Part C: RHEL 10 and RHCSA in 2026

## C1. RHEL 10 key facts
| Fact | Note |
|---|---|
| Released | **2025** (book is built on RHEL 10.0) |
| Source base | **Fedora 40** (book) |
| Desktop | **GNOME 47** on **Wayland**, terminal app **Ptyxis** (book) |
| Command-line AI help | **RHEL Lightspeed** command-line assistant (Chapter 5, book) |
| Flatpak | Application install chapter in my book (Chapter 13) |
| CPU requirement | Needs **x86-64-v3** CPUs (extra, verify) |
| Image mode | Bootable-container based deployment (extra) |
| SSH root | `PermitRootLogin prohibit-password` by default: **no root password login over SSH** |
| Kernel | Check with `uname -r` on my VM (extra) |

## C2. RHCSA exam (EX200) on RHEL 10 (extra, verify on Red Hat's page)
- The current RHCSA exam, **EX200**, is based on **RHEL 10**.
- 2026 study guides list **Flatpak** and **systemd timers** among new objectives.
- Third-party sources **disagree** about **Podman/containers**. Red Hat's own EX200 objectives page is the source of truth.
- Exam rule to remember: **configurations must survive a reboot** without intervention.
- Hands-on practice matters more than reading. This is why I install and rebuild my VM.

> [!tip] Study order after this note
> Chapters 1, 2, 3 (finish), then 6, then 7 to 11 (files, users, permissions), then 12 to 16, then 17 to 19, then the comprehensive review in Chapter 20.

---

# Revision checklist
- [ ] Book: RH124, RHEL 10.0, Edition 3, 714 pages, 20 chapters
- [ ] Next course after RH124: RH134
- [ ] Machines: workstation, servera, serverb, serverc in `lab.example.com`
- [ ] `lab start`, `lab grade`, `lab finish` and what each does
- [ ] ROLE timers: auto-stop max 11 hours, auto-destroy max 14 days
- [ ] Reset and DELETE lose saved work. Shutdown and Power Off keep the disk
- [ ] Linux is a Unix-**like** kernel, not Unix code
- [ ] Architecture layers and the system-call flow
- [ ] Kernel jobs, shell = interpreter, userspace
- [ ] Fedora → CentOS Stream → RHEL
- [ ] `.rpm` and `dnf` (RHEL) vs `.deb` and `apt` (Debian)
- [ ] XFS default, Btrfs not supported