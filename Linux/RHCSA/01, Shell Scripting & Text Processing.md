## 1.1 Changing the Shell Environment

The shell environment consists of variables, aliases, and functions that control how your shell behaves and how commands execute.

#### 1. Environment Variables
- Variables that store configuration information
- Available to the shell and programs it runs
- Written in UPPERCASE by convention

##### Key Environment Variables

| Variable | Purpose | Example |
|----------|----------|---------|
| `PATH` | Directories searched for executable commands | `/usr/local/bin:/usr/bin:/bin` |
| `HOME` | Current user's home directory | `/home/username` |
| `USER` | Current logged-in username | `student` |
| `SHELL` | Default login shell | `/bin/bash` |
| `PWD` | Present working directory | `/home/student/documents` |
| `LANG` | System language and locale settings | `en_US.UTF-8` |
| `PS1` | Primary shell prompt format | `[\u@\h \W]\$` |
| `HISTSIZE` | Number of commands stored in shell history | `1000` |
| `EDITOR` | Default command-line text editor | `vim` or `nano` |

---

#### 2. Viewing Environment Variables

**Commands to Display Variables:**

```bash
# Display all environment variables
env

# Alternative command
printenv

# Display all variables (environment + shell + functions)
set

# Display specific variable
echo $PATH
echo $HOME

# Display variable with printenv
printenv PATH
printenv HOME

# Display all variables starting with 'HIS'
env | grep HIS
```

```bash
[student@server ~]$ echo $PATH
/home/student/.local/bin:/home/student/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin

[student@server ~]$ echo $HOME
/home/student

[student@server ~]$ printenv USER
student
```

> **Note:** `env` and `printenv` display only environment variables, while `set` displays shell variables, environment variables, and shell functions.

##### PATH
Determines where the shell searches for executable commands.

```bash
echo $PATH
```

Example output:

```text
/usr/local/bin:/usr/bin:/bin
```

> Directories are searched from **left to right**. The first matching executable is executed.

##### HOME
Displays the current user's home directory.

```bash
echo $HOME
```

Output:

```text
/home/student
```

##### USER
Shows the currently logged-in username.

```bash
echo $USER
```

##### SHELL
Displays the default shell being used.

```bash
echo $SHELL
```

Output:

```text
/bin/bash
```

##### PWD
Shows the current working directory.

```bash
echo $PWD
```

##### LANG
Displays the system language and locale.

```bash
echo $LANG
```

##### PS1
Displays the shell prompt format.

```bash
echo $PS1
```

##### HISTSIZE
Shows how many commands are stored in history.

```bash
echo $HISTSIZE
```

##### EDITOR
Displays the default text editor.

```bash
echo $EDITOR
```

| Command | Description |
|---------|-------------|
| `export VAR=value` | Create or modify an environment variable |
| `unset VAR` | Remove an environment variable |
| `set` | Display shell variables, environment variables, and functions |

> **Tip:** Environment variables are inherited by child processes and are widely used to configure Linux applications and shell behavior.

---

#### 3. Setting Variables

##### Local Variables (Current Shell Only)

```bash
# Create a local variable
MYVAR="Hello World"

# Display it
echo $MYVAR

# This variable will NOT be available to child processes
```

##### Environment Variables (Exported to Child Processes)

```bash
# Method 1: Create then export
MYVAR="Hello World"
export MYVAR

# Method 2: Create and export in one line
export MYVAR="Hello World"

# Verify it's exported
env | grep MYVAR
```

**Difference Example:**

```bash
# Local variable
LOCALVAR="I am local"

# Start a new bash shell (child process)
bash

# Try to access the variable
echo $LOCALVAR
# Output: (empty - variable not available)

exit

# Now with exported variable
export EXPORTVAR="I am exported"
bash
echo $EXPORTVAR
# Output: I am exported

exit
```

---

#### 4. Modifying PATH Variable

The `PATH` variable determines where the shell looks for commands.

View current PATH:

```bash
echo $PATH
```

##### Add directory to PATH (temporary - current session only):

```bash
# Add to beginning
export PATH="$HOME/bin:$PATH"

# Add to end
export PATH="$PATH:$HOME/scripts"

# Verify
echo $PATH
```

**Example Scenario:**

```bash
# Create a custom script directory
mkdir ~/bin
echo 'echo "Hello from my script!"' > ~/bin/myscript
chmod +x ~/bin/myscript

# Add to PATH
export PATH="$PATH:$HOME/bin"

# Now you can run it from anywhere
myscript
# Output: Hello from my script!
```

---

#### 5. Shell Aliases

Aliases are shortcuts for commands.

**Creating Aliases:**

```bash
# Basic alias
alias ll='ls -l'

# Alias with multiple commands
alias update='sudo dnf update -y'

# Alias with options
alias rm='rm -i'

# View all aliases
alias

# View specific alias
alias ll
```

**Common Useful Aliases:**

```bash
alias ls='ls --color=auto'
alias ll='ls -lh'
alias la='ls -lah'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias df='df -h'
alias free='free -h'
alias ports='ss -tulanp'
```

**Remove an Alias:**

```bash
unalias ll
```

---

#### 6. Making Changes Persistent

Changes made in the command line are temporary (lost when you log out).

##### Configuration Files

| File | Scope | Purpose |
|------|-------|---------|
| `~/.bashrc` | Per-user, non-login shells | Aliases, functions, prompt customization, and user-specific shell settings |
| `~/.bash_profile` | Per-user, login shells | Environment variables, startup programs, and typically loads `~/.bashrc` |
| `/etc/bashrc` | System-wide, non-login shells | Global aliases, functions, and shell settings |
| `/etc/profile` | System-wide, login shells | Global environment variables and startup settings |

##### Login vs Non-Login Shells

- **Login shell:** Console login, SSH login, or `bash --login` (reads `/etc/profile` and `~/.bash_profile`)
- **Non-login shell:** Opening a new terminal or running `bash` (reads `~/.bashrc`)

**Best Practice**

Put this in `~/.bash_profile`:

```bash
# Source .bashrc if it exists
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

This ensures `.bashrc` is loaded for both login and non-login shells.

---

#### 7. Editing Configuration Files

##### Add Permanent Environment Variables

Edit `~/.bash_profile`:

```bash
vim ~/.bash_profile
```

Add at the end:

```bash
# Custom environment variables
export EDITOR=vim
export HISTSIZE=10000
export PATH="$PATH:$HOME/bin"
```

Apply changes immediately:

```bash
source ~/.bash_profile
# or
. ~/.bash_profile
```

##### Add Permanent Aliases

Edit `~/.bashrc`:

```bash
vim ~/.bashrc
```

```bash
# Custom aliases
alias ll='ls -lh'
alias la='ls -lah'
alias update='sudo dnf update'
alias ports='ss -tulanp'
```

Apply changes:

```bash
source ~/.bashrc
```

---

#### 8. Shell Functions

Functions are reusable blocks of code (more powerful than aliases).

Creating Functions:

```bash
# Define a function
mkcd() {
    mkdir -p "$1"
    cd "$1"
}

# Use it
mkcd ~/test/new/directory
```

---

#### 9. Command Substitution

Execute a command and use its output.

```bash
# Using $() (recommended)
TODAY=$(date +%Y-%m-%d)
echo "Today is $TODAY"

# Using backticks (older syntax)
TODAY=`date +%Y-%m-%d`

# Practical examples
CURRENT_DIR=$(pwd)
NUM_FILES=$(ls | wc -l)
KERNEL=$(uname -r)

echo "You have $NUM_FILES files in $CURRENT_DIR"
echo "Running kernel: $KERNEL"
```

---

#### 10. Quotes and Special Characters

##### Double Quotes (`""`)

- Variables are expanded
- Command substitution works

```bash
NAME="John"
echo "Hello $NAME"
```

Output:

```text
Hello John
```

##### Single Quotes (`''`)

- Everything is literal
- No expansion

```bash
NAME="John"
echo 'Hello $NAME'
```

Output:

```text
Hello $NAME
```

##### Escaping with Backslash (`\`)

```bash
echo "The cost is \$100"
```

Output:

```text
The cost is $100
```

---

#### 11. History Management

```bash
# View command history
history

# Execute command from history
!100
!! 
!-2
!string

# Search history
Ctrl+R
history | grep ssh

# Clear history
history -c

# History variables
export HISTSIZE=10000
export HISTFILESIZE=20000
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "
export HISTCONTROL=ignoredups
```