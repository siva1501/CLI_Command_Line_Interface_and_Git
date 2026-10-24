# Linux CLI Learning Notes

Beginner-friendly notes for learning Linux/Ubuntu terminal commands in a logical order. Each section explains what the command does and gives examples to practise.

## Table of Contents

1. [What are Linux, CLI, Terminal, and Shell?](#1-what-are-linux-cli-terminal-and-shell)
2. [Command Structure and Help](#2-command-structure-and-help)
3. [Paths and Special Symbols](#3-paths-and-special-symbols)
4. [Navigation and Listing Files](#4-navigation-and-listing-files)
5. [Creating, Copying, Moving, and Removing Files](#5-creating-copying-moving-and-removing-files)
6. [Reading and Editing Text Files](#6-reading-and-editing-text-files)
7. [Wildcards](#7-wildcards)
8. [Finding Files and Searching Text](#8-finding-files-and-searching-text)
9. [Pipes and Command Combinations](#9-pipes-and-command-combinations)
10. [Input, Output, and Redirection](#10-input-output-and-redirection)
11. [Text Processing](#11-text-processing)
12. [Permissions and Ownership](#12-permissions-and-ownership)
13. [Processes, PID, PPID, and Signals](#13-processes-pid-ppid-and-signals)
14. [Users and Groups](#14-users-and-groups)
15. [System Information, Disk Space, and Memory](#15-system-information-disk-space-and-memory)
16. [Installing Software with APT](#16-installing-software-with-apt)
17. [SSH and rsync](#17-ssh-and-rsync)
18. [Terminal Shortcuts and Command History](#18-terminal-shortcuts-and-command-history)
19. [Safe Practice Exercises](#19-safe-practice-exercises)
20. [Quick Revision Sheet](#20-quick-revision-sheet)

---

## 1. What are Linux, CLI, Terminal, and Shell?

- **Linux** is an operating-system kernel used by Linux distributions such as Ubuntu.
- **CLI** means Command-Line Interface. You type commands instead of selecting buttons.
- **Terminal** is the application where you type commands.
- **Shell** reads your commands and runs them. Bash is a common Linux shell.
- **GUI** means Graphical User Interface, where you use windows, icons, menus, and a mouse.

Example:

```bash
pwd
```

This prints the path of the directory you are currently in.

## 2. Command Structure and Help

A common command pattern is:

```text
command [options] [arguments]
```

Example:

```bash
ls -lh ~/Downloads
```

- `ls` is the command.
- `-l` requests a detailed listing.
- `-h` makes file sizes human-readable.
- `~/Downloads` is the target directory.

Get help:

```bash
ls --help
man ls
```

- `--help` usually displays a short usage guide.
- `man` opens the manual page. Press `q` to exit the manual viewer.

## 3. Paths and Special Symbols

A **path** tells Linux where a file or directory is located.

| Symbol | Meaning | Example |
|---|---|---|
| `/` | Root of the filesystem | `/home/siva` |
| `~` | Current user's home directory | `cd ~` |
| `.` | Current directory | `find . -type f` |
| `..` | Parent directory, one level up | `cd ..` |
| `*` | Matches zero or more characters in a filename pattern | `ls *.txt` |
| `?` | Matches one character in a filename pattern | `ls file?.txt` |

Examples:

```bash
cd /
cd ~
cd ..
pwd
```

`cd ..` moves up one level. `cd ~` moves directly to your home directory.

## 4. Navigation and Listing Files

| Command | What it does |
|---|---|
| `pwd` | Prints the current working directory |
| `ls` | Lists files and directories |
| `ls -l` | Lists details, including permissions and ownership |
| `ls -a` | Includes hidden entries |
| `ls -la` | Includes hidden entries in long format |
| `ls -lh` | Shows file sizes in readable units |
| `ls -lt` | Sorts by modification time, newest first |
| `ls -ltr` | Sorts by modification time, oldest first |
| `cd directory` | Moves into a directory |
| `cd ..` | Moves to the parent directory |
| `cd ~` | Moves to the home directory |
| `clear` | Clears the terminal display |

To list everything in your home directory, including hidden entries, with readable sizes and oldest entries first:

```bash
ls -alhtr ~
```

A filename beginning with `.` is hidden from ordinary `ls` output. `ls -a` displays hidden entries too.

## 5. Creating, Copying, Moving, and Removing Files

| Command | What it does | Example |
|---|---|---|
| `mkdir` | Creates a directory | `mkdir practice` |
| `mkdir -p` | Creates parent directories as needed | `mkdir -p work/linux/notes` |
| `touch` | Creates an empty file if it does not exist | `touch notes.txt` |
| `cp` | Copies a file | `cp notes.txt backup.txt` |
| `cp -r` | Copies a directory and its contents | `cp -r project project-backup` |
| `mv` | Moves or renames a file | `mv old.txt new.txt` |
| `rm` | Removes a file | `rm old.txt` |
| `rmdir` | Removes an empty directory | `rmdir empty-folder` |
| `rm -r` | Removes a directory and its contents | `rm -r practice-copy` |

**Caution:** `rm` may permanently delete files without moving them to the Trash. Check the path and filename before running removal commands. Avoid using `rm -rf` unless you fully understand the consequences.

## 6. Reading and Editing Text Files

| Command | What it does | Example |
|---|---|---|
| `cat` | Prints a file's contents | `cat notes.txt` |
| `less` | Views a long file one screen at a time | `less notes.txt` |
| `head` | Prints the first 10 lines by default | `head notes.txt` |
| `head -n 5` | Prints the first 5 lines | `head -n 5 notes.txt` |
| `tail` | Prints the last 10 lines by default | `tail notes.txt` |
| `tail -n 20` | Prints the last 20 lines | `tail -n 20 notes.txt` |
| `nano` | Opens a simple terminal text editor | `nano notes.txt` |

In `less`, use the arrow keys or Page Up/Down to move and press `q` to exit. In Nano, use `Ctrl+O` to save, Enter to confirm the filename, and `Ctrl+X` to exit.

Write text from the terminal:

```bash
echo "Hello Linux" > hello.txt
echo "Another line" >> hello.txt
```

The first command creates or overwrites `hello.txt`. The second appends a line to the end.

You can also type multiple lines into a file:

```bash
cat > story.txt
First line
Second line
Third line
```

Press `Ctrl+D` on a new line to finish. This overwrites an existing `story.txt`, so use it carefully.

## 7. Wildcards

Wildcards match filename patterns.

| Pattern | Meaning | Example |
|---|---|---|
| `*` | Zero or more characters | `ls *.txt` |
| `?` | Exactly one character | `ls file?.txt` |
| `[abc]` | One character from the listed set | `ls file[123].txt` |

Examples:

```bash
ls *.txt
ls report?.csv
find . -type f -name "*.py"
```

- `ls *.txt` lists matching `.txt` files in the current directory.
- `?` matches one character, so `file1.txt` and `fileA.txt` match `file?.txt`.
- In `find`, quotes around `"*.py"` prevent the shell from expanding the wildcard before `find` receives it.

## 8. Finding Files and Searching Text

### Find files by name or type

```bash
find ~ -name "notes.txt"
find ~ -type f -name "*.txt"
find . -type d
```

- `find` searches directory trees.
- `~` means your home directory.
- `-type f` selects regular files.
- `-type d` selects directories.
- `-name "*.txt"` matches names ending in `.txt`.

### Search within files with `grep`

```bash
grep "error" app.log
grep -i "error" app.log
grep -n "error" app.log
grep -R "TODO" .
```

- `grep "error" app.log` prints lines containing `error`.
- `-i` ignores uppercase/lowercase differences.
- `-n` includes matching line numbers.
- `-R` searches recursively in a directory.

Example with a pipe:

```bash
cat app.log | grep error
```

This works, although for a single file you can usually write the simpler `grep error app.log`.

## 9. Pipes and Command Combinations

A **pipe** (`|`) sends the standard output of one command to another command's standard input.

Example:

```bash
ps -ef | grep firefox
```

1. `ps -ef` lists processes.
2. `|` sends the output to the next command.
3. `grep firefox` shows matching lines.

More examples:

```bash
find . -type f | sort
history | grep "cd"
ls -lh ~ | less
```

Pipes let you combine small commands to perform a larger task. The output of the command on the left becomes input to the command on the right.

## 10. Input, Output, and Redirection

| Symbol | Meaning | Example |
|---|---|---|
| `>` | Sends output to a file, replacing its previous contents | `date > dates.txt` |
| `>>` | Appends output to a file | `date >> dates.txt` |
| `<` | Takes input from a file | `sort < names.txt` |
| `2>` | Sends error output to a file | `command 2> errors.txt` |
| `2>&1` | Sends error output to the same destination as standard output | `command > output.txt 2>&1` |
| `|` | Sends output to another command | `cat app.log \| grep error` |

Remember:

- `>` overwrites.
- `>>` appends and preserves existing contents.
- `|` connects commands; it does not append to a file.

## 11. Text Processing

### Count lines, words, and bytes with `wc`

```bash
wc notes.txt
wc -l notes.txt
wc -w notes.txt
wc -c notes.txt
```

- `wc -l` counts lines.
- `wc -w` counts words.
- `wc -c` counts bytes.
- `wc` without an option reports lines, words, and bytes.

### Sort lines with `sort`

```bash
sort names.txt
sort -r names.txt
```

`sort` prints lines in sorted order. `-r` reverses the order.

### Remove adjacent duplicate lines with `uniq`

```bash
sort names.txt | uniq
sort names.txt | uniq -c
```

`uniq` normally combines only adjacent duplicate lines. Sorting first groups identical lines together. `uniq -c` shows the count for each group.

### Select fields with `awk`

```bash
ps aux | awk '{print $1, $2, $11}'
```

`awk` processes text by fields. In this example it prints selected whitespace-separated fields from process output.

### Count matching output lines

```bash
grep -R "TODO" . | wc -l
```

This counts the matching output lines, not necessarily the number of files containing the word.

### Use `xargs`

`xargs` turns input items into arguments for another command. For example:

```bash
find . -type f -name "*.txt" -print0 | xargs -0 ls -l
```

`-print0` and `-0` safely handle filenames containing spaces and many special characters.

## 12. Permissions and Ownership

Run:

```bash
ls -l
```

Example permission string:

```text
-rwxr-xr--
```

The first character describes the file type. The next three characters are the owner's permissions, the next three are the group's permissions, and the final three are permissions for others.

### Permission values

| Permission | Letter | Numeric value |
|---|---|---:|
| Read | `r` | 4 |
| Write | `w` | 2 |
| Execute | `x` | 1 |
| No permission | `-` | 0 |

Add the values in each group:

- `7 = 4 + 2 + 1 = rwx`
- `6 = 4 + 2 = rw-`
- `5 = 4 + 1 = r-x`
- `4 = r--`

### Common `chmod` examples

```bash
chmod 644 notes.txt
chmod 755 script.sh
chmod u+x script.sh
```

- `chmod 644 notes.txt`: owner can read/write; group and others can read.
- `chmod 755 script.sh`: owner can read/write/execute; group and others can read/execute.
- `chmod u+x script.sh`: adds execute permission for the owner.

### Change ownership with `chown`

```bash
sudo chown siva notes.txt
sudo chown siva:developers notes.txt
```

`chown` changes file ownership. Changing ownership often requires `sudo` and the appropriate authorization.

## 13. Processes, PID, PPID, and Signals

A **process** is a running program.

- **PID**: Process ID, a number identifying a process.
- **PPID**: Parent Process ID, the ID of the process that started it.

### View processes

```bash
ps
ps -ef
ps aux
top
pgrep firefox
ps -C firefox -o pid,ppid,cmd
```

- `ps -ef` and `ps aux` display process information.
- `top` shows live process activity; press `q` to exit.
- `pgrep firefox` searches for process IDs by name.
- `ps -C firefox -o pid,ppid,cmd` displays the PID, PPID, and command for processes whose command name is `firefox` (if the process is running under that name).

### Stop a process

```bash
kill 2450
kill -9 2450
```

- `kill 2450` normally sends `SIGTERM`, asking the process to stop cleanly.
- `kill -9 2450` sends `SIGKILL`, which forces the process to stop.

Use `kill -9` only when a normal termination does not work. Replace `2450` with the actual PID.

Other useful process controls:

- `Ctrl+C` usually interrupts the foreground command.
- `Ctrl+Z` suspends the foreground job.
- `command &` starts a command in the background, for example `firefox &` where supported.

## 14. Users and Groups

```bash
whoami
id
groups
```

- `whoami` prints the current username.
- `id` shows the user's ID and group IDs.
- `groups` lists the groups to which the current user belongs.

## 15. System Information, Disk Space, and Memory

```bash
uname -a
cat /etc/os-release
df -h
du -sh ~/Downloads
free -h
lspci
```

- `uname -a` prints kernel and system information.
- `cat /etc/os-release` shows Linux distribution details.
- `df -h` shows available and used space on mounted filesystems.
- `du -sh ~/Downloads` shows the total space used by the Downloads directory.
- `free -h` reports memory and swap usage.
- `lspci` lists PCI devices, such as some hardware components.

### Check Linux version

```bash
cat /etc/os-release
uname -r
```

The first command shows distribution information; the second shows the running kernel version.

## 16. Installing Software with APT

On Ubuntu and other Debian-based distributions, `apt` manages software packages.

```bash
sudo apt update
sudo apt install package-name
sudo apt remove package-name
```

- `sudo` runs a command with elevated privileges when authorized.
- `apt update` refreshes package lists.
- `apt install` installs a package.
- `apt remove` removes an installed package.

Check the package name and understand a command before approving changes.

## 17. SSH and rsync

### SSH

SSH provides an encrypted remote terminal connection to a computer you are authorized to access.

```bash
ssh username@hostname
```

Replace `username` and `hostname` with the correct remote account and host. You need network access and a configured SSH server.

### rsync

`rsync` copies and synchronizes files or directories.

```bash
rsync -av source-folder/ destination-folder/
```

- `-a` enables archive mode and preserves common file attributes.
- `-v` prints more details.
- A trailing slash on `source-folder/` means to copy its contents.

For remote copying:

```bash
rsync -av ./project/ username@hostname:~/project/
```

Check source and destination carefully. Options such as `--delete` can remove destination files.

## 18. Terminal Shortcuts and Command History

| Shortcut | Action |
|---|---|
| `Ctrl+C` | Interrupt the foreground command |
| `Ctrl+Z` | Suspend the foreground job |
| `Ctrl+D` | Send end-of-input; may exit an idle shell |
| `Ctrl+L` | Clear the terminal display |
| `Ctrl+A` | Move to the start of the command line in Bash |
| `Ctrl+E` | Move to the end of the command line in Bash |
| `Ctrl+R` | Search backward through command history |
| `Tab` | Complete a command or path |
| Up/Down arrows | Browse previous commands |

Other useful commands:

```bash
history
history | grep "find"
```

`history` lists previous commands. `Ctrl+R` lets you search interactively; press Enter to run the shown command, or use the arrow keys to edit it first.

## 19. Safe Practice Exercises

Practise inside a separate directory to avoid changing important files.

### Exercise 1: Navigation

1. Display your current directory.
2. Move to your home directory.
3. List all entries, including hidden ones.
4. Move into a child directory and then up one level.

Commands: `pwd`, `cd`, `ls -la`, `cd ..`.

### Exercise 2: Files

1. Create a directory named `cli-practice`.
2. Create `notes.txt` inside it.
3. Add a line of text.
4. Display the contents.
5. Make a backup copy.

Commands: `mkdir`, `touch`, `echo`, `cat`, `cp`.

### Exercise 3: Search and text processing

1. Find `.txt` files under `cli-practice`.
2. Search for a word inside a text file.
3. Count the file's lines.
4. Sort a file containing names.

Commands: `find`, `grep`, `wc -l`, `sort`.

### Exercise 4: Pipes and processes

1. List processes.
2. Filter the output for a program name.
3. Show PID, PPID, and command columns for Firefox.
4. Read the `ps` manual.

Commands: `ps`, `grep`, `|`, `ps -C firefox -o pid,ppid,cmd`, `man ps`.

### Exercise 5: Permissions

1. Check a file's permissions.
2. Set `notes.txt` to mode `644`.
3. Check its permissions again.
4. Explain what each digit in `755` means.

Commands: `ls -l`, `chmod`.

### Exercise 6: System checks

Check your Linux distribution, disk space, and memory.

Commands: `cat /etc/os-release`, `df -h`, `free -h`.

## 20. Quick Revision Sheet

```bash
pwd                              # Current directory
ls -la                           # List all entries, including hidden ones
ls -alhtr ~                      # Home entries, readable sizes, oldest first
cd ..                            # Move to parent directory
cd ~                             # Move to home directory
mkdir practice                   # Create a directory
touch notes.txt                  # Create an empty file
cp notes.txt backup.txt          # Copy a file
mv old.txt new.txt               # Move or rename a file
cat notes.txt                    # Display a file
nano notes.txt                   # Edit a file
echo "Hello" > notes.txt         # Write/overwrite a file
echo "Next line" >> notes.txt    # Append to a file
find ~ -type f -name "*.txt"     # Find text files
grep -i "error" app.log          # Search without case sensitivity
ps -ef | grep firefox            # Filter process list
ps -C firefox -o pid,ppid,cmd    # Show Firefox PID, PPID, and command
wc -l notes.txt                  # Count lines
chmod 644 notes.txt              # Owner read/write; others read
chmod 755 script.sh              # Owner rwx; group/others r-x
kill 2450                        # Ask process to terminate
kill -9 2450                     # Force process termination
df -h                            # Filesystem disk space
du -sh ~/Downloads               # Directory disk usage
free -h                          # Memory and swap
cat /etc/os-release              # Linux distribution details
man chmod                        # Open a command manual
```

## Final Learning Advice

Learn the concepts in order, practise each command in a safe test directory, and understand what the command does before memorising it. Use `man command` or `command --help` whenever you need a reminder.
