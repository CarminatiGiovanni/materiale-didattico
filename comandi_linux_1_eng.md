# 🐧 Linux Labs — TPS
### 2 Labs • 1 Hour • Ubuntu/Debian • From Scratch

---

> **Before you start** — open the terminal on your Ubuntu VM.  
> You can find it by searching **Terminal** in the applications menu, or with the shortcut `Ctrl + Alt + T`.
>
> Check that you're ready:
> ```bash
> whoami
> ```
> The terminal replies with your username. You're good to go.

---

## 📋 Schedule

| Lab | Title | Duration | Topics |
|-----|-------|----------|--------|
| 1 | Exploring and managing files | ~25 min | Linux filesystem, navigation, creating and managing files/folders |
| 2 | Users, permissions and processes | ~30 min | UNIX permissions, users, running processes |

> ⏱ Leave 5 minutes at the end to compare results with your classmates.

---

---

# Lab 1 — Exploring and Managing Files

**Duration:** ~25 minutes  
**Objective:** Navigate the Linux filesystem and manage files and folders from the terminal.

---

## 💡 Theory in 2 minutes

In Linux **everything is a file**: documents, devices, processes. The filesystem starts from a single root called `/`, with no drive letters like in Windows.

```
/                   ← system root
├── home/           ← user folders (like "Users" in Windows)
│   └── student/    ← your home directory (~)
├── etc/            ← system configuration files
├── var/            ← logs and variable data
└── tmp/            ← temporary files
```

The terminal always shows **where you are** in the prompt. The `~` symbol means you are in your home directory.

---

## Part A — Navigating the filesystem (8 min)

**1.** Find out where you are:

```bash
pwd
```

> `pwd` = *Print Working Directory*. Shows the absolute path of the current folder.

**2.** List the contents of the current folder:

```bash
ls
```

**3.** Detailed view with permissions, size and date:

```bash
ls -l
```

**4.** Show hidden files too (those starting with `.`):

```bash
ls -la
```

**5.** Move to the `/etc` folder and look inside:

```bash
cd /etc
ls
```

**6.** Jump back to your home directory in one step:

```bash
cd ~
pwd
```

**7.** Go up one level (parent folder):

```bash
cd ..
pwd
```

**8.** Go back to home:

```bash
cd ~
```

---

## Part B — Creating and managing files and folders (17 min)

**9.** Create a working folder for this lab:

```bash
mkdir lab-linux
cd lab-linux
```

**10.** Create an empty text file:

```bash
touch notes.txt
ls
```

**11.** Write some text into the file:

```bash
echo "Operating System: Ubuntu" > notes.txt
echo "Kernel: Linux" >> notes.txt
echo "Shell: bash" >> notes.txt
```

> `>` overwrites the file. `>>` appends to the end without deleting existing content.

**12.** Display the file contents:

```bash
cat notes.txt
```

**13.** Create a subfolder and copy a file into it:

```bash
mkdir memos
cp notes.txt memos/notes-copy.txt
ls memos/
```

**14.** Verify the original still exists:

```bash
ls
```

**15.** Move (rename) the original file:

```bash
mv notes.txt notes-lab1.txt
ls
```

**16.** Create two more files and then delete them:

```bash
touch file-to-delete.txt another-file.txt
ls
rm file-to-delete.txt
rm another-file.txt
ls
```

**17.** Delete the subfolder with all its contents:

```bash
rm -r memos/
ls
```

> ⚠️ Linux has no recycle bin in the terminal: `rm` is **permanent**. Always use it carefully.

---

## ✅ Lab 1 Checklist

- [ ] I can navigate the filesystem with `cd`, `ls`, `pwd`
- [ ] I can create files with `touch` and `echo`
- [ ] I can copy (`cp`), move (`mv`) and delete (`rm`) files and folders
- [ ] I understand the difference between `>` and `>>`

---

## ❓ Questions — Lab 1

**Q1.** The Linux filesystem starts from `/` while Windows uses drive letters like `C:\`. Which approach seems more logical to you and why?


**Q2.** What is the difference between `>` and `>>`? Write a concrete example where using `>>` is necessary while `>` would cause a problem.


**Q3.** The `mv` command is used both to move and to rename a file. How can a single command do two things? What do the two operations have in common?


**Q4.** `rm` doesn't use a recycle bin and deletes permanently. Why do you think Linux works this way instead of using a bin like desktop systems? In what situations is this behaviour an advantage? (Think about what Linux was originally built for.)


---

## 📌 Lab 1 Commands

| Command | What it does |
|---------|--------------|
| `pwd` | Shows the current folder |
| `ls` | Lists the contents |
| `ls -la` | Detailed list including hidden files |
| `cd <path>` | Changes folder |
| `cd ~` | Goes back to home |
| `cd ..` | Goes up to the parent folder |
| `mkdir <name>` | Creates a folder |
| `touch <file>` | Creates an empty file |
| `echo "text" > file` | Writes text to a file (overwrites) |
| `echo "text" >> file` | Appends text to the end of a file |
| `cat <file>` | Displays the contents of a file |
| `cp <src> <dst>` | Copies a file |
| `mv <src> <dst>` | Moves or renames a file |
| `rm <file>` | Deletes a file (permanent!) |
| `rm -r <folder>` | Deletes a folder and its contents |

---

---

# 🔑 Deep Dive — `sudo` and `apt`: installing and managing the system

> Read this section before starting Lab 2. It has no mandatory steps, but contains optional hands-on exercises marked with 🔧.

---

## Who is `root` and what is `sudo`?

In Linux there is a special user called **root** — the system super-administrator. Root can do anything: read every file, change configuration, install software, or delete the entire operating system.

For safety, your normal user **is not root** and cannot perform risky operations. The `sudo` command (*Super User DO*) lets you run a single command with root privileges, after entering your password.

```
normal user  →  sudo command  →  system asks for password  →  command runs as root
```

**Concrete example:** updating the list of available packages requires root because it modifies system files:

```bash
# Without sudo → error: Permission denied
apt update

# With sudo → works
sudo apt update
```

> 💡 `sudo` does not permanently turn you into root: it only applies to that single command. If you want a full root session (not recommended), you use `sudo -i` — but you won't need it in this course.

### Why not always use root?

The principle is called **least privilege**: every user and process should have only the permissions strictly necessary. Always working as root is dangerous because:

- A typo (`rm -r /` instead of `rm -r ./`) can destroy the system
- A malicious program running as root would have unlimited access
- There is no way to distinguish administrator actions from normal ones in the logs

---

## `apt` — the package manager

On Ubuntu and Debian, programs are installed via **packages**: archives containing the software, its dependencies and installation instructions. The package manager `apt` (*Advanced Package Tool*) handles downloading, installing, updating and removing packages.

Packages are downloaded from **repositories**: official servers maintained by Ubuntu/Debian containing thousands of verified, safe programs.

```
Official Ubuntu repository  →  Internet  →  apt  →  your system
```

### The essential `apt` commands

| Command | What it does |
|---------|--------------|
| `sudo apt update` | Updates the list of available packages (installs nothing) |
| `sudo apt upgrade` | Installs updates for all packages already on the system |
| `sudo apt install <name>` | Installs a new package |
| `sudo apt remove <name>` | Removes a package (leaves configuration files) |
| `sudo apt purge <name>` | Removes a package **and** its configuration files |
| `apt search <word>` | Searches packages by name or description (no sudo needed) |
| `apt show <name>` | Shows detailed information about a package |

> ⚠️ `sudo apt update` and `sudo apt install` are **separate, distinct commands**. `update` installs nothing: it only downloads the updated list of what is available. Running it before installing is good practice because it prevents installing outdated versions.

### The typical installation workflow

```bash
# 1. Update the list of available packages
sudo apt update

# 2. Install the desired program
sudo apt install <package-name>
```

During installation, `apt` shows how much disk space will be used and asks for confirmation. Reply `Y` (or press Enter if `Y` is already the default).

---

## 🔧 Hands-on exercises (optional)

**Ex. 1 — Search and install `tree`** (a command that displays folders as a tree):

```bash
apt search tree          # search for packages containing "tree"
apt show tree            # read the description
sudo apt update
sudo apt install tree
tree ~/lab-linux         # try the new command straight away
```

**Ex. 2 — Install and try `htop`** (an improved version of `top`):

```bash
sudo apt install htop
htop                     # press q to quit
```

**Ex. 3 — Remove a package you just installed:**

```bash
sudo apt remove tree
tree                     # the command no longer exists
```

---

## ❓ Questions — sudo and apt

**Q-S1.** What is the difference between `apt update` and `apt upgrade`? Why is it important to run them in the correct order?


**Q-S2.** Why does `sudo apt update` require `sudo`, while `apt search <word>` does not? What distinguishes the two operations in terms of permissions?


---

---

# Lab 2 — Users, Permissions and Processes

**Duration:** ~30 minutes  
**Prerequisite:** Lab 1 completed  
**Objective:** Understand the UNIX permission system, manage users and inspect running processes.

---

## 💡 Theory in 2 minutes

### UNIX Permissions

Every file in Linux has three groups of permissions:

```
-rwxr-xr--   student   group   file.txt
 ^^^              → owner permissions       (rwx = read, write, execute)
    ^^^           → group permissions       (r-x = read, execute)
       ^^^        → others' permissions     (r-- = read only)
```

| Symbol | Meaning | Numeric value |
|--------|---------|---------------|
| `r` | read | 4 |
| `w` | write | 2 |
| `x` | execute | 1 |
| `-` | permission absent | 0 |

Permissions can also be written in **octal notation**: `rwx` = 4+2+1 = **7**, `r-x` = 4+0+1 = **5**, `r--` = 4+0+0 = **4**.

So `rwxr-xr--` = **754**.

---

## Part A — Reading and modifying permissions (12 min)

**1.** Go to the Lab 1 folder and create a test file:

```bash
cd ~/lab-linux
echo "permissions test file" > test.txt
```

**2.** Display the file's permissions:

```bash
ls -l test.txt
```

> Read the first column: type + owner permissions + group permissions + others' permissions.

**3.** Remove all permissions (nobody can do anything):

```bash
chmod 000 test.txt
ls -l test.txt
```

**4.** Try to read the file — you will get an error:

```bash
cat test.txt
```

**5.** Restore permissions: owner can read and write, others can only read:

```bash
chmod 644 test.txt
ls -l test.txt
cat test.txt
```

**6.** Make the file executable for the owner:

```bash
chmod 744 test.txt
ls -l test.txt
```

> Notice the change in the first column: the `x` appears.

**7.** Create a minimal bash script and make it executable:

```bash
echo '#!/bin/bash' > greet.sh
echo 'echo "Hello from Linux!"' >> greet.sh
chmod +x greet.sh
./greet.sh
```

---

## Part B — Users and groups (8 min)

**8.** Find out who you are and which groups you belong to:

```bash
whoami
id
groups
```

**9.** Display the list of system users:

```bash
cat /etc/passwd
```

> Each line has the format: `username:x:UID:GID:info:home:shell`  
> System users (UID < 1000) manage services — they are not real people.

**10.** Filter only "real" users (UID ≥ 1000):

```bash
awk -F: '$3 >= 1000' /etc/passwd
```

**11.** Display the system groups:

```bash
cat /etc/group
```

**12.** Show information about your user:

```bash
id
```

> `uid` = user ID, `gid` = primary group ID, `groups` = all groups you belong to.

---

## Part C — Processes (10 min)

**13.** Display the current user's active processes:

```bash
ps
```

**14.** Display **all** system processes in extended format:

```bash
ps aux
```

> Main columns: `USER` (owner), `PID` (process ID), `%CPU`, `%MEM`, `COMMAND`.

**15.** Real-time dynamic view (like Task Manager):

```bash
top
```

> Press `q` to quit. Press `k` to kill a process by entering its PID.

**16.** Search for a specific process by name:

```bash
ps aux | grep bash
```

> The `|` (pipe) passes the output of `ps aux` as input to `grep`, which filters lines containing "bash".

**17.** Launch a process in the background and then find it:

```bash
sleep 60 &
ps aux | grep sleep
```

**18.** Kill the process by finding its PID (replace `<PID>` with the number you saw):

```bash
kill <PID>
ps aux | grep sleep
```

---

## ✅ Lab 2 Checklist

- [ ] I can read a file's permissions with `ls -l`
- [ ] I can change permissions with `chmod` using octal notation
- [ ] I know the structure of `/etc/passwd`
- [ ] I can view processes with `ps aux` and `top`
- [ ] I can kill a process with `kill`

---

## ❓ Questions — Lab 2

**Q5.** Convert the following permission sets to octal notation. Show your working:
- `rwxrwxrwx` → ___
- `rw-r--r--` → ___
- `rwx------` → ___

**Q6.** A file has permissions `chmod 000`. Even the owner cannot read it. How can they fix the problem? Could another normal user still read the file? What about root?


**Q7.** What does a process's PID represent? Why does every process have a unique number instead of just using its name?


**Q8.** Explain in detail what this command does, piece by piece:
```bash
ps aux | grep bash
```
What does `ps aux` do? What does `|` do? What does `grep bash` do? What is the final result?


---

## 📌 Lab 2 Commands

| Command | What it does |
|---------|--------------|
| `ls -l` | Shows permissions, owner and size |
| `chmod <octal> <file>` | Changes permissions using octal notation |
| `chmod +x <file>` | Adds execute permission |
| `whoami` | Shows the current user |
| `id` | Shows UID, GID and groups of the user |
| `groups` | Lists the user's groups |
| `cat /etc/passwd` | Lists system users |
| `cat /etc/group` | Lists system groups |
| `ps` | Processes of the current user |
| `ps aux` | All system processes |
| `top` | Real-time process monitor |
| `kill <PID>` | Kills a process by ID |
| `command &` | Launches a process in the background |
| `cmd1 \| cmd2` | Pipe: passes output of cmd1 to cmd2 |

---

---

## 📚 All Commands — Quick Reference

| Command | Category |
|---------|----------|
| `pwd` | Navigation |
| `cd <path>` / `cd ~` / `cd ..` | Navigation |
| `ls` / `ls -l` / `ls -la` | Navigation |
| `mkdir` / `touch` | Creation |
| `echo "..." > file` / `>>` | Writing files |
| `cat <file>` | Reading files |
| `cp` / `mv` / `rm` / `rm -r` | File management |
| `chmod` | Permissions |
| `whoami` / `id` / `groups` | Users |
| `cat /etc/passwd` / `/etc/group` | Users |
| `ps` / `ps aux` / `top` | Processes |
| `kill <PID>` | Processes |
| `command &` | Background processes |
| `cmd1 \| cmd2` | Pipe |

---

*Linux Labs TPS — Year 4 • v1.0 • March 2026 • 2 labs / 1 hour*