---
tags: [rhel, rhcsa, rh124, linux, rhel10, chapter2]
book: RH124 - Red Hat System Administration I (RHEL 10)
chapter: 2 - Accessing the Command Line
---

# Chapter 2: Accessing the Command Line (RHEL 10)

> [!info] Goal of this chapter
> Log in to a Linux system and run simple commands using the shell.
> Install steps are in a separate note: [[RHEL 10 Install Notes]].

> [!tip] Tags used in this note
> **(book)** = taught in RH124. **(extra)** = added from my own knowledge. Test the **(extra)** items on your VM.

---

## 2.1 Terminology (this is in the quiz)

| Term | Meaning |
|---|---|
| **Command line** | Text interface where you type instructions |
| **Shell** | Program that **interprets** the commands you type (Bash) |
| **Terminal** | Interface that gives a display and keyboard to a shell session |
| **Console** | Direct interface to the machine |
| **Physical console** | Real keyboard and monitor attached to the machine |
| **Virtual console** | Logical console. Each one supports its own independent login |
| **Prompt** | Text shown when the shell is waiting for a command |

> [!note] Easy way to remember
> Terminal = the **window**. Shell = the **brain** inside it. Console = the **direct** connection to the machine.

---

## 2.2 Types of terminals and consoles

| Type | Notes |
|---|---|
| **Virtual console (TTY)** | Text login. Switch with `Ctrl+Alt+F1` to `F6`. Names: `/dev/tty1` to `/dev/tty6` |
| **Terminal emulator (GUI)** | A window inside the desktop. **RHEL 10 uses Ptyxis** (it replaced GNOME Terminal) |
| **Pseudo-terminal (PTY / PTS)** | Used by GUI terminals, SSH and tmux. Names: `/dev/pts/0`, `/dev/pts/1` |
| **Serial console** | Over a serial port (`ttyS0`). For headless servers, routers, embedded devices |
| **System / physical console** | Direct keyboard and monitor. Shows boot and kernel messages |

- **TTY** originally meant **Teletypewriter**.
- Check your current terminal with: `tty`
  - GUI terminal or SSH shows `/dev/pts/N`
  - Text console shows `/dev/ttyN`

> [!warning] RHEL 10 update (older notes say tty7)
> The **graphical login screen runs on tty1**.
> Your GUI session usually starts on **tty2**. If tty2 already has an active text login, the next free console is used.
> After you log out of the GUI, the screen returns to the login screen on tty1.

- **GUI mode**: unlimited terminal windows.
- **CLI mode**: 6 text consoles (tty1 to tty6).
- **Headless server**: no permanent keyboard or display. Admins use SSH or a serial console.
- Many admins run servers **without a GUI** to save resources.

---

## 2.3 Shell prompt

```
[user@hostname ~]$
```

| Part | Meaning |
|---|---|
| `user` | Current username |
| `@` | Separator |
| `hostname` | System name |
| `~` | Current directory (`~` = home directory) |
| `$` | **Regular user** |
| `#` | **Root user** (superuser) |

> [!warning] Watch the `#`
> A `#` prompt means every command runs with full power and can damage the whole system.

---

## 2.4 Regular user vs root user

| | Regular user | Root user |
|---|---|---|
| Prompt | `$` | `#` |
| Power | Limited | Full control |
| Home directory | `/home/username` | `/root` |
| How it exists | Created by `useradd` | Exists by default |

- Users you create are **regular users**.
- A regular user can get admin power through **sudo** by joining the **wheel** group **(extra)**:
```bash
  usermod -aG wheel username
```
  > [!warning] Do not forget `-a`
  > `-aG` means **append** to the group. Using only `-G` replaces all the user's other groups.
- Sudo rules live in `/etc/sudoers`. Edit only with `visudo` **(extra)**.
- Ticking "Make this user administrator" during install puts the user in `wheel`.
- Use `sudo command` for one admin task. Use `su -` to become root.

> [!note] RHEL 10 root account (extra, verify on your VM)
> If you lock the root account during installation, you cannot log in as root at all. Use your administrator user with `sudo` instead.

---

## 2.5 What is a shell?

- A program that **interprets** the commands you type and passes the request to the kernel.
- **Flow:**
```
  User → Command/Program → Shell → System call → Kernel → Hardware
```

### Interpreter vs compiler (extra)
| | Interpreter | Compiler |
|---|---|---|
| How it works | Reads and runs **line by line**, live | Converts the **whole program** to machine code first |
| Output | No separate file | Creates an executable file |
| Example | Bash | `gcc` (C compiler) |

### Types of shell
| Shell | Platform | Note |
|---|---|---|
| `cmd.exe` | Windows | Old command interpreter |
| PowerShell | Windows | Powerful scripting |
| `sh` | UNIX | Original Bourne shell |
| **`bash`** | **Linux, RHEL default** | **B**ourne **A**gain **SH**ell, improved `sh`, has scripting |
| `zsh` | macOS default (since Catalina) | Also available in RHEL |

> [!note] IOS is not Linux
> **IOS** = Internetwork Operating System. It is **Cisco's** router and switch OS with its own CLI. It is not part of RHEL.

### Interactive vs non-interactive shell (extra)
- **Interactive**: you type commands and read the output.
- **Non-interactive**: runs on its own, like a **script** or cron job.
- **Login shell** vs non-login: `su - user` starts a **login shell**. Plain `su user` does not.

### Check and change your shell (extra)
```bash
echo $SHELL           # your login shell
cat /etc/shells       # all allowed shells
chsh -s /bin/zsh      # change your login shell
```
> [!warning] Correction
> A shell change takes effect after you **log out and log in again**. A full system restart is not needed.

### Where commands live (extra)
- Programs are stored in `/usr/bin` (normal commands) and `/usr/sbin` (admin commands).
- On modern RHEL, `/bin` and `/sbin` are links to them.
- The shell finds them using the **PATH** variable: `echo $PATH`
- Find a command: `which ls` or `whereis passwd`

---

## 2.6 Parts of a command (book)

```
command [options] [arguments]
```

| Part | Meaning | Example |
|---|---|---|
| **Command** | Program to run | `usermod` |
| **Option** | Adjusts behavior. Starts with `-` or `--` | `-L` |
| **Argument** | Target the command works on | `user01` |

```bash
ls              # command only
ls -l           # command + option
ls /etc         # command + argument
ls -la /etc     # command + options + argument
usermod -L user01    # command=usermod, option=-L, argument=user01 (locks the password)
```
- Words are separated by **spaces**. Press **Enter** to run.

### mkdir examples
```bash
mkdir d1
mkdir --help
mkdir -p d1/d2/d3       # -p creates parent directories as needed
mkdir -pv d1/d2/d3      # -v shows what is created
```

---

## 2.7 Power off and reboot (extra)

| Action | Commands |
|---|---|
| Shut down | `poweroff`, `systemctl poweroff`, `init 0` |
| Reboot | `reboot`, `systemctl reboot`, `init 6` |

GUI: system menu > Power icon > Power Off or Restart. If you do not choose, it acts after **60 seconds**.

---

## 2.8 GUI vs CLI login mode (extra, systemd targets)

| Mode | Target |
|---|---|
| **GUI** | `graphical.target` |
| **CLI** | `multi-user.target` |

```bash
systemctl get-default                     # show default target
systemctl isolate multi-user.target       # switch to CLI now (temporary)
systemctl isolate graphical.target        # switch back to GUI
systemctl set-default multi-user.target   # make CLI permanent
systemctl set-default graphical.target    # make GUI permanent
setfont sun12x22                          # bigger font (text console only)
```
> [!warning] Save your work first
> `isolate multi-user.target` closes the GUI session and everything open in it.

---

## 2.9 Logging in to a remote system with SSH

### What is SSH? (book)
- **Secure Shell**. Gives you a shell on another machine over an **encrypted** connection.
- Client program: `ssh`. Server service: `sshd`.

### 3 checks before connecting (your notes)
1. **Connectivity**: you can reach the server. Find its IP with `ip a`.
2. **Service**: `systemctl status sshd` shows **active (running)**.
3. **Credentials**: a password or a key.

### Commands
```bash
ssh remoteuser@remotehost                  # password login
ssh -i mylab.pem remoteuser@remotehost     # key login
exit                                        # log out (or Ctrl+D)
```

### Key-based login (book)
- **Private key** = secret, stays with you (like a password).
- **Public key** = placed in the user's account on the server. It does not need to be secret.
- Get the `.pem` file onto your machine by GUI, `scp`, or **WinSCP** (a Windows tool that copies files over SSH).
- The private key file must not be readable by others:
```bash
  chmod 600 mylab.pem     # the book uses 600. 400 also works
```
  If the permissions are wrong, OpenSSH refuses to use the key.

### First login warning (book)
```
The authenticity of host 'remotehost (192.0.2.42)' can't be established.
Are you sure you want to continue connecting (yes/no)?
```
- `yes` saves the server's **host key**, so you are not asked again.
- `no` closes the connection.
- If a saved host key later **changes**, SSH blocks the connection. It can mean someone is pretending to be that server (**interceptor / man-in-the-middle attack**).
- Saved host keys are in `~/.ssh/known_hosts` (extra).

> [!warning] Root over SSH in RHEL 10
> The default is `PermitRootLogin prohibit-password`. Root **cannot use a password** over SSH, but key-based root login can still be configured.
> Normal practice: log in as a regular user, then use `sudo` or `su -`.

> [!note] Password or key expiry (extra)
> Passwords can be made to expire with password aging (`chage`, covered in the users chapter). Keys can be rotated or removed so old ones stop working.

### Useful commands (extra)
```bash
ip a                        # IP addresses. "inet" = IPv4, "inet6" = IPv6
nmcli con show              # list network connections
nmcli con down ens160       # bring a connection down
nmcli con up ens160         # bring it up (use the name shown by con show)
who -a                      # logged-in users, detailed
w                           # logged-in users and what they run
last                        # login history
su - root                   # switch to root with a login shell
```
> [!warning] Do not run `nmcli con down` over SSH
> You will cut off your own session. Do it from the VM console.

---

## 2.10 GNOME desktop in RHEL 10 (book)

- Default desktop: **GNOME 47**. Graphics system: **Wayland** (the older one is **X Window System / X11**).
- GNOME is part of the **GNU** project (gnu.org).
- Default look is standard GNOME. You can choose **GNOME Classic** from the Settings icon on the login screen (after picking the user, before the password).
- First login as a new user starts the **Tour** program. You can close it.

### Parts of GNOME Shell
| Part | What it does |
|---|---|
| **Top bar** | Always visible. Activities, clock, system menu |
| **Activities Overview** | Start apps, organize windows. Open with the **Super key** or the Red Hat logo |
| **Dash (dock)** | Favorite and running apps, Show Applications icon |
| **Windows overview** | Thumbnails of open windows |
| **Workspace selector** | Switch workspaces |
| **Message tray** | Notifications and calendar. Open with `Super+M` or the clock |
| **System menu** | Top right: network, settings, lock, log out, power |

### Starting a terminal
- Activities Overview > Terminal in the dash, or search "terminal".
- `Alt+F2`, type `ptyxis`, press Enter.

### Shortcuts
| Shortcut | Action |
|---|---|
| `Super` | Open Activities Overview |
| `Super+M` | Message tray |
| `Super+L` | Lock screen |
| `Ctrl+Alt+Left/Right` | Switch workspace |
| `Ctrl+Alt+Del` | Power off dialog |
| `Ctrl+Shift+N` | New terminal window (extra) |
| `Ctrl+Shift+T` | New terminal tab (extra) |
| `Ctrl+Shift++` / `Ctrl+-` | Zoom in / out (extra) |

### Workspaces
- Separate desktops to group windows by task.
- **Dynamic** (default): empty ones are removed. **Fixed**: a set number always shows.
- Change in System menu > Settings > Multitasking.

### Lock, log out, power
- **Lock**: system menu Lock icon, or `Super+L`. Unlock with Enter or Space, then your password.
- **Log out**: system menu > Power icon > Log Out.
- **Power off or restart**: Power icon, or `Ctrl+Alt+Del`.
- You may see "Install pending software updates" on shutdown. You can untick it.

> [!tip] Lab tip
> In the classroom browser, Super and Ctrl+Alt keys may not reach the VM. Use the on-screen keyboard (click a modifier key once to hold it) or the mouse.

---

## 2.11 Executing commands with Bash

### Running more than one command
| Symbol | Meaning | Example |
|---|---|---|
| `;` | Run one after another, **always** | `cd /tmp; ls; pwd` |
| `&&` | Run next **only if the previous succeeds** | `cd /etc && cat passwd` |
| `\|\|` | Run next **only if the previous fails** | `cd /nodir \|\| echo "Not found"` |
| `\|` (pipe) | Send output of one command to the next | `cat /etc/passwd \| grep root` |

```bash
ls | head -5               # first 5 items
history | grep ssh         # find ssh commands
cat /etc/passwd | wc -l    # count lines
```

### Simple commands
```bash
whoami
mkdir l1; cd l1; touch testing     # three commands on one line
date                               # date and time
date +%R                           # 24-hour time, e.g. 17:58
date +%r                           # 12-hour time with AM/PM
date +%x                           # date, e.g. 04/29/25
passwd                             # change your password
```
- Normal user: changes only their own password and must enter the old one.
- Root: `passwd username` sets anyone's password.
- Passwords must be strong (mixed characters, not a dictionary word).
- Linux does not need file extensions. Use `file` to see the real type.

### Viewing files
| Command | Purpose |
|---|---|
| `file /etc/passwd` | Show file type |
| `cat /etc/passwd` | Show the whole file |
| `cat file1 file2` | Show several files together |
| `cat > file1` | Create a file. Type text, then `Ctrl+D` to save. **Overwrites** old content |
| `cat >> file1` | **Appends** to the end of a file |
| `less file` | One page at a time. Arrows to scroll, `Q` to quit |
| `head file` | First 10 lines. `head -n 3 file` = first 3 |
| `tail file` | Last 10 lines. `tail -n 3 file` = last 3 |
| `wc file` | Lines, words, bytes |
| `wc -l` / `-w` / `-c` | Only lines / words / characters |

### Tab completion
- `Tab` once = complete a unique name. `Tab` twice = list all matches.
- Works on commands, file names and options. Try `useradd --` then `Tab Tab`.

### Writing a long command over several lines
- End the line with `\`. The shell shows a `>` continuation prompt.
- Do **not** type the `>` yourself. It would act as redirection.
```bash
head -n 3 \
/usr/share/dict/words \
/usr/share/dict/linux.words
```

### Command history
| Action | How |
|---|---|
| Show history | `history` |
| Repeat last command | `!!` |
| Run command number 12 | `!12` |
| Run last command starting with `ls` | `!ls` |
| Search history | `Ctrl+R`, then type |
| Reuse last argument | `Esc + .` or `Alt + .` (repeat to go further back) |
| Clear current session history | `history -c` |
- Up and Down arrows move through old commands. Left and Right arrows edit them.

> [!warning] History is a security risk
> `~/.bash_history` can hold passwords typed in commands and file paths.
> `history -c` clears only the **current session's memory**. The saved file is updated when the shell exits.

### Command-line editing shortcuts
| Shortcut | Action |
|---|---|
| `Ctrl+A` | Jump to start of line |
| `Ctrl+E` | Jump to end of line |
| `Ctrl+U` | Delete from cursor to start of line |
| `Ctrl+K` | Delete from cursor to end of line |
| `Ctrl+Left/Right` | Move one word left or right |
| `Ctrl+R` | Search history |
| `Ctrl+D` | On an empty prompt, log out |

---

## 2.12 Quiz answers (book)

| Question | Answer |
|---|---|
| Interpreter of typed commands | **Shell** |
| Cue that the shell waits for input | **Prompt** |
| Name of the program to run | **Command** |
| Adjusts behavior | **Option** |
| Target of the command | **Argument** |
| Hardware keyboard and display | **Physical console** |
| Several independent logins | **Virtual console** |
| Display and keyboard for a shell session | **Terminal** |
| Last 5 lines of a file | `tail -n 5 /var/log/messages` |
| Separate commands on one line | `;` |
| Change a password | `passwd` |
| Show file type | `file` |
| Complete names and options | `Tab` |
| Re-run a command from history | `!number` |
| Jump to the start of the line | `Ctrl+A` |
| List previous commands | `history` |
| Copy last argument | `Esc + .` |

---

## 2.13 Guided exercise (book): change a password in the GUI
1. Log out: system menu > Power icon > Log Out.
2. Log in as `student`.
3. Open a terminal, run `passwd`, enter the old and new password.
4. Log out and log in with the new password to test it.
5. Lock the screen, then unlock it.
6. Open Power Off, then **Cancel** it.
7. Run `lab finish cli-desktop` to reset the lab.

## 2.14 Lab (book): practice commands
```bash
date
date +%R
file zcat
wc zcat                # then use Esc + . for the file name
head zcat
tail zcat
!!                     # repeat the last command
tail -n 20 zcat        # Up arrow, Ctrl+A, Ctrl+Right, add -n 20
history                # find the number
!2                     # run that command again
```

---

## Chapter 2 revision checklist
- [ ] Shell vs terminal vs console vs virtual console vs prompt
- [ ] tty1 = login screen, tty2 = GUI session in RHEL 10
- [ ] `$` vs `#`, regular user vs root, wheel group
- [ ] Interpreter vs compiler, shell types, login shell
- [ ] `command [options] [arguments]`
- [ ] `graphical.target` vs `multi-user.target`
- [ ] SSH: connectivity, `sshd` active, credentials, `chmod 600`
- [ ] Host key warning and `known_hosts`
- [ ] GNOME 47, Wayland, Ptyxis, Activities Overview, Super key
- [ ] `;` `&&` `||` `|`
- [ ] `cat >` vs `cat >>`, `head`, `tail`, `wc`
- [ ] History: `!!`, `!12`, `!ls`, `Ctrl+R`, `Esc + .`
- [ ] Editing: `Ctrl+A/E/U/K`