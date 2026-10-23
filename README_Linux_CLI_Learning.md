# Linux CLI Learning Notes

Beginner-friendly notes for learning the Linux/Ubuntu Command-Line
Interface (CLI), with explanations and practical examples.

> **Learning status:** These notes cover topics we have discussed and
> useful next steps. Some commands are revision topics; this README does
> not mean every command has been mastered.

## Table of Contents

1.  [What is CLI?](#1-what-is-cli)
2.  [Terminal basics](#2-terminal-basics)
3.  [Files and directories](#3-files-and-directories)
4.  [Reading and writing files](#4-reading-and-writing-files)
5.  [Hidden files and listing
    details](#5-hidden-files-and-listing-details)
6.  [Finding files and searching
    text](#6-finding-files-and-searching-text)
7.  [Pipes and command combinations](#7-pipes-and-command-combinations)
8.  [Redirection](#8-redirection)
9.  [File permissions and ownership](#9-file-permissions-and-ownership)
10. [Processes: PID and PPID](#10-processes-pid-and-ppid)
11. [System information and disk
    space](#11-system-information-and-disk-space)
12. [Installing software with APT](#12-installing-software-with-apt)
13. [SSH and rsync](#13-ssh-and-rsync)
14. [Useful command-line shortcuts](#14-useful-command-line-shortcuts)
15. [Practice exercises](#15-practice-exercises)
16. [Suggested next steps](#16-suggested-next-steps)

------------------------------------------------------------------------

## 1. What is CLI?

**CLI** means **Command-Line Interface**. It lets you interact with your
computer by typing commands into a terminal.

-   **GUI:** Graphical User Interface; you use windows, icons, and a
    mouse.
-   **CLI:** You type commands, and the shell runs them.

On Ubuntu, a common shell is **Bash**.

Example:

``` bash
pwd
```

This prints the path of your current directory.

### Basic command pattern

``` bash
command [options] [arguments]
```

For example:

``` bash
ls -lh ~/Downloads
```

-   `ls` lists directory contents.
-   `-l` displays detailed information.
-   `-h` shows file sizes in a human-readable format.
-   `~/Downloads` is the directory to list.

Be careful with commands that delete files or change permissions. Check
the path before pressing Enter.

## 2. Terminal basics

  Command            Explanation                        Example
  ------------------ ---------------------------------- ------------------
  `pwd`              Print the current directory path   `pwd`
  `ls`               List files and directories         `ls`
  `cd`               Change directory                   `cd ~/Downloads`
  `clear`            Clear the terminal display         `clear`
  `whoami`           Print the current username         `whoami`
  `history`          Show recently entered commands     `history`
  `man`              Open a command's manual            `man ls`
  `command --help`   Show brief help for a command      `ls --help`

### Important path symbols

-   `~` means your home directory, for example `/home/siva`.
-   `.` means the current directory.
-   `..` means the parent directory.
-   `/` is the filesystem root directory.

Examples:

``` bash
cd ~
cd ..
cd /tmp
```

## 3. Files and directories

### Create files and directories

``` bash
touch notes.txt
mkdir cli-practice
mkdir -p projects/python/api
```

-   `touch` creates an empty file if it does not exist; if it exists, it
    updates its timestamps.
-   `mkdir` creates a directory.
-   `mkdir -p` creates parent directories as needed.

### Copy, move, rename, and remove

``` bash
cp notes.txt notes-copy.txt
mv notes-copy.txt backup.txt
mv backup.txt cli-practice/
rm notes.txt
rmdir empty-folder
```

-   `cp` copies a file.
-   `mv` moves or renames a file.
-   `rm` removes files.
-   `rmdir` removes an empty directory.

Copy a directory and its contents:

``` bash
cp -r source-folder destination-folder
```

**Safety:** `rm` can permanently remove files. Review the path with
`pwd` and `ls` first. Do not run unfamiliar commands copied from the
internet without understanding them.

## 4. Reading and writing files

### Read a file

``` bash
cat notes.txt
less notes.txt
head notes.txt
tail notes.txt
head -n 5 notes.txt
tail -n 10 notes.txt
```

-   `cat` prints file contents.
-   `less` lets you scroll through a file; press `q` to quit.
-   `head` displays the first lines (10 by default).
-   `tail` displays the last lines (10 by default).

Count lines, words, and bytes:

``` bash
wc notes.txt
wc -l notes.txt
```

### Write text with Nano

``` bash
nano notes.txt
```

Type your text, then: - `Ctrl + O` --- save (write out); press Enter to
confirm the filename. - `Ctrl + X` --- exit. - `Ctrl + G` --- show help.

If Nano shows a prompt such as **"Save modified buffer?"**, choose the
appropriate option: - `Y` to save, - `N` to discard, - `Ctrl + C` to
cancel that prompt.

### Write text directly from the terminal

Replace the file's contents:

``` bash
echo "Hello Linux" > hello.txt
```

Append to the end of a file:

``` bash
echo "Second line" >> hello.txt
```

The `>` operator overwrites the target file; `>>` appends. Check the
filename before using `>`.

For multiple lines:

``` bash
cat > story.txt
First line
Second line
Third line
```

Press `Ctrl + D` on a new line to finish input. This method overwrites
an existing file, so use it carefully.

## 5. Hidden files and listing details

Linux filenames beginning with `.` are hidden by default in ordinary
`ls` output.

``` bash
ls -a
ls -l
ls -lh
ls -lt
ls -ltr
```

-   `-a`: include hidden entries.
-   `-l`: long format with permissions, owner, size, and date.
-   `-h`: human-readable file sizes.
-   `-t`: sort by modification time, newest first.
-   `-r`: reverse the sort order.

### List everything in your home directory, including hidden files, oldest first, with readable sizes

``` bash
ls -alhtr ~
```

To sort by modification time with newest first:

``` bash
ls -alht ~
```

Note: `.` and `..` also appear when using `-a`.

## 6. Finding files and searching text

### Find files and directories

Search for a file by name in your home directory:

``` bash
find ~ -name "notes.txt"
```

Find Python files:

``` bash
find ~/WORK -type f -name "*.py"
```

Find directories:

``` bash
find . -type d
```

-   `-type f` means regular files.
-   `-type d` means directories.
-   `-name` matches a filename pattern; `*.py` matches names ending in
    `.py`.

### Search inside files with grep

``` bash
grep "Linux" notes.txt
grep -i "linux" notes.txt
grep -n "Linux" notes.txt
grep -R "TODO" .
```

-   `-i`: ignore letter case.
-   `-n`: show matching line numbers.
-   `-R`: search recursively through files under a directory.

## 7. Pipes and command combinations

A **pipe** (`|`) sends one command's standard output to another
command's standard input.

Example:

``` bash
ps aux | grep firefox
```

1.  `ps aux` prints process information.
2.  `|` passes that output onward.
3.  `grep firefox` displays lines matching `firefox`.

Other examples:

``` bash
ls -lh ~ | less
find . -type f | sort
history | grep "cd"
```

### Count matching lines

``` bash
grep -R "TODO" . | wc -l
```

This counts the matching output lines. It is not necessarily the number
of files containing the word.

### Use `sort` and `uniq`

``` bash
sort names.txt
sort names.txt | uniq
sort names.txt | uniq -c
```

`uniq` normally combines only adjacent duplicate lines, so sorting first
helps group identical lines.

### Use `awk` for columns

``` bash
ps aux | awk '{print $1, $2, $11}'
```

`awk` processes text by fields. In this example, it prints selected
whitespace-separated fields from process output. The exact meaning of a
field depends on the input format.

### Use `xargs`

``` bash
find . -name "*.tmp" -print0 | xargs -0 -r ls -l
```

This safely passes filenames (including spaces) to `ls -l` for
inspection. It does **not** delete them.

`-print0` and `-0` handle spaces and many unusual characters in
filenames; `-r` avoids running the command when there is no input on GNU
`xargs`.

## 8. Redirection

Redirection sends input or output somewhere other than the terminal.

  -----------------------------------------------------------------------
  Symbol                              Meaning
  ----------------------------------- -----------------------------------
  `>`                                 Write standard output to a file,
                                      replacing its previous contents

  `>>`                                Append standard output to a file

  `<`                                 Read standard input from a file

  `2>`                                Write standard error to a file

  `2>&1`                              Send standard error to the same
                                      destination as standard output
  -----------------------------------------------------------------------

Examples:

``` bash
ls > files.txt
date >> files.txt
sort < names.txt
find / -name "notes.txt" 2> errors.txt
```

Save both standard output and standard error in one file in Bash:

``` bash
some-command > output.txt 2>&1
```

In Bash, `> output.txt 2>&1` sends both streams to the file. The order
matters.

## 9. File permissions and ownership

Check permissions:

``` bash
ls -l notes.txt
```

Example output:

``` text
-rw-r--r-- 1 siva siva 120 Oct 23 10:00 notes.txt
```

The first character indicates the type (`-` for a regular file, `d` for
a directory). The next nine characters are permissions for owner, group,
and others.

-   `r` = read = 4
-   `w` = write = 2
-   `x` = execute = 1

### Numeric permissions

    Number Permission
  -------- --------------------------------
       `7` read + write + execute (`rwx`)
       `6` read + write (`rw-`)
       `5` read + execute (`r-x`)
       `4` read only (`r--`)

For example:

-   `644` = owner can read/write; group and others can read.
-   `755` = owner can read/write/execute; group and others can
    read/execute.

Change permissions:

``` bash
chmod 644 notes.txt
chmod 755 script.sh
chmod u+x script.sh
```

Change ownership (usually requires administrator privileges):

``` bash
sudo chown username:groupname notes.txt
```

Replace `username` and `groupname` with real values. Avoid changing
system-file ownership without a clear reason.

## 10. Processes: PID and PPID

A **process** is a running instance of a program.

-   **PID:** Process ID; identifies a process.
-   **PPID:** Parent Process ID; identifies the process that started or
    manages it.
-   A child process is created by another process. Its parent can
    finish, so process relationships may change over time.

List processes:

``` bash
ps
ps aux
ps -ef
```

Search for Firefox:

``` bash
ps aux | grep firefox
pgrep -a firefox
```

`pgrep -a firefox` is often simpler for finding Firefox processes. If
Firefox is not running, there may be no result.

Show PID and PPID:

``` bash
ps -eo pid,ppid,comm
```

Show details for a specific PID:

``` bash
ps -p 1234 -o pid,ppid,comm
```

Replace `1234` with the actual PID.

### Stop a process

``` bash
kill PID
```

Replace `PID` with the actual process ID. The default signal asks the
process to terminate gracefully. Use `kill -9 PID` only as a last resort
because it prevents normal cleanup. Do not kill processes you do not
recognize.

Other useful commands:

``` bash
top
```

`top` shows live process and resource usage; press `q` to quit.

## 11. System information and disk space

### Linux version and distribution

``` bash
uname -a
cat /etc/os-release
```

-   `uname -a` prints kernel and system information.
-   `/etc/os-release` contains Linux distribution details, such as
    Ubuntu version.

### Disk space

``` bash
df -h
df -h ~
```

`df` shows free and used space on mounted filesystems. `-h` makes the
sizes easier to read.

### Directory and file sizes

``` bash
du -sh ~/Downloads
du -sh ./*
```

-   `du` estimates space used by files and directories.
-   `-s` gives a summary.
-   `-h` uses readable units.

### Memory

``` bash
free -h
```

This displays memory and swap usage.

## 12. Installing software with APT

APT is commonly used to install and manage software on Ubuntu and other
Debian-based distributions.

Refresh package lists:

``` bash
sudo apt update
```

Install a package:

``` bash
sudo apt install package-name
```

Upgrade installed packages:

``` bash
sudo apt upgrade
```

Search for a package:

``` bash
apt search package-name
```

Remove a package:

``` bash
sudo apt remove package-name
```

Replace `package-name` with the actual package name. Read the proposed
changes before confirming. `sudo` grants administrator privileges, so
use it only when required.

## 13. SSH and rsync

### SSH

SSH provides a secure remote terminal connection to a computer that you
are authorized to access.

``` bash
ssh username@hostname
```

Replace `username` and `hostname` with the actual remote account and
host. You need network access and a configured SSH service or server.

### rsync

`rsync` copies and synchronizes files, locally or across a network.

``` bash
rsync -av source-folder/ destination-folder/
```

-   `-a` preserves common file attributes and copies directories
    recursively.
-   `-v` prints more details.
-   The trailing slash on `source-folder/` means to copy its contents.

For remote copying:

``` bash
rsync -av ./project/ username@hostname:~/project/
```

Check source and destination carefully before syncing. Options such as
`--delete` can remove destination files and should be used only when you
fully understand the effect.

## 14. Useful command-line shortcuts

  -----------------------------------------------------------------------
  Shortcut                            Action
  ----------------------------------- -----------------------------------
  `Ctrl + C`                          Interrupt the foreground command

  `Ctrl + D`                          Send end-of-input; in a shell, may
                                      log out if the prompt is idle

  `Ctrl + L`                          Clear the terminal display

  `Ctrl + A`                          Move to the start of the command
                                      line in Bash

  `Ctrl + E`                          Move to the end of the command line
                                      in Bash

  `Tab`                               Complete commands or paths

  Up/Down arrows                      Browse command history
  -----------------------------------------------------------------------

## 15. Practice exercises

Try these in a practice directory so you do not accidentally modify
important files.

### Exercise 1: Basic navigation

1.  Print your current directory.
2.  Move to your home directory.
3.  List files, including hidden files.

Commands to explore: `pwd`, `cd`, `ls -a`.

### Exercise 2: File operations

1.  Create a directory named `cli-practice`.
2.  Create `notes.txt` inside it.
3.  Write a line of text into the file.
4.  Display the file's contents.

Commands to explore: `mkdir`, `touch`, `echo`, `cat`.

### Exercise 3: Search

1.  Find all `.txt` files under your practice directory.
2.  Search a file for a word you choose.
3.  Count the lines in a text file.

Commands to explore: `find`, `grep`, `wc -l`.

### Exercise 4: Pipes

1.  List processes.
2.  Filter the output for a program name.
3.  Print the first few lines of a long command's output.

Commands to explore: `ps`, `grep`, `head`, `|`.

### Exercise 5: Permissions

1.  Display a file's permissions.
2.  Set a text file to `644`.
3.  Display the permissions again.

Commands to explore: `ls -l`, `chmod`.

### Exercise 6: System checks

Check the distribution version, disk space, and memory.

Commands to explore: `cat /etc/os-release`, `df -h`, `free -h`.

## 16. Suggested next steps

A useful order for continuing your learning is:

1.  **Redirection and streams:** understand standard input, standard
    output, and standard error.
2.  **Pipes and filters:** combine small commands to solve larger tasks.
3.  **Process management:** understand `ps`, `pgrep`, `top`, and
    signals.
4.  **Permissions:** practice numeric and symbolic `chmod` modes.
5.  **Bash scripting:** learn variables, conditions, loops, and
    functions.
6.  **Automation:** write small scripts to organize files and inspect
    system information.

## Quick reference

``` bash
pwd                    # Show current directory
ls -alhtr ~            # All home entries, readable sizes, oldest first
cd ~/Downloads         # Change directory
mkdir cli-practice     # Create directory
touch notes.txt        # Create empty file
cat notes.txt          # Display file
nano notes.txt         # Edit file
find ~ -name "*.py"    # Find Python files under home
ps -eo pid,ppid,comm   # Show PID, PPID, command
df -h                  # Disk space
free -h                # Memory usage
cat /etc/os-release    # Distribution information
```

------------------------------------------------------------------------

**Tip:** Do not try to memorize every command at once. Learn what each
command does, practise it in a safe directory, and use `man command` or
`command --help` when you need more information.
