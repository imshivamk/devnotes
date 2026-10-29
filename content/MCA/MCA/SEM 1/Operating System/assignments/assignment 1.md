
# Linux Commands Practical Guide

## Operating Systems Assignment — WSL Environment

### Environment Used

This practical guide assumes:

- Operating system: **WSL (Windows Subsystem for Linux)**
    
- Shell: **Bash**
    
- Working directory: `~/demo`
    
- Distribution: Ubuntu/Debian-style WSL unless otherwise mentioned
    

All practical files will be kept inside:

```
~/demo
```

---

# 0. Setup — Create the Demo Environment

## `cd`

### Purpose

`cd` means **change directory**. It changes the current working directory of the shell.

### Syntax

```
cd [directory]
```

### Parameters

```
directory → location to which you want to move
```

### Example

```
cd ~
```

`~` represents the current user's home directory.

Now create our demonstration directory:

```
mkdir -p demo
cd demo
```

### Verify

```
pwd
```

Expected output will look similar to:

```
/home/your_username/demo
```

---

## Create the demo structure

```
mkdir -p files directories backup text logs links
```

### `mkdir -p`

```
mkdir → make directory
-p    → create parent directories if necessary
```

For example:

```
mkdir -p directories/example/subdirectory
```

creates all three levels if they do not already exist.

Create some files:

```
touch files/file1.txt files/file2.txt files/file3.txt
```

Add content:

```
echo "Hello Linux" > files/file1.txt
echo "Operating Systems Assignment" > files/file2.txt
echo "Linux commands are useful" > files/file3.txt
```

Check:

```
ls -l files
```

---

# 1. Basic Shell Utilities

# `pwd`

## Purpose

`pwd` means **print working directory**.

It tells you which directory the shell is currently inside.

### Syntax

```
pwd
```

### Example

```
cd ~/demo
pwd
```

Example output:

```
/home/student/demo
```

### OS/Shell concept

The shell maintains a **current working directory** for each shell process.

---

# `ls`

## Purpose

Lists files and directories.

### Basic command

```
ls
```

### Long format

```
ls -l
```

`-l` means **long listing**.

Example:

```
-rw-r--r-- 1 student student 12 Oct 2 18:30 file1.txt
```

The long format provides information such as:

```
permissions
links
owner
group
size
modification time
filename
```

### Show hidden files

```
ls -a
```

`-a` = **all**

Hidden Linux files usually begin with `.`.

### Long + hidden

```
ls -la
```

Equivalent to:

```
ls -l -a
```

### Human-readable sizes

```
ls -lh
```

`-h` = human-readable.

For example:

```
1024       → 1.0K
1048576    → 1.0M
```

### Sort by modification time

```
ls -lt
```

`-t` = sort according to modification time.

### Combined example

```
ls -lah
```

Means:

```
-l → long format
-a → include hidden files
-h → human-readable sizes
```

### Assignment demonstration

```
cd ~/demo
ls
ls -la
ls -lh files
ls -lt files
```

---

# `cd`

### Syntax

```
cd directory
```

Examples:

```
cd files
pwd
```

Go one level up:

```
cd ..
pwd
```

`..` means **parent directory**.

Go home:

```
cd ~
```

Go directly to demo:

```
cd ~/demo
```

### Important special paths

```
.       current directory
..      parent directory
~       home directory
/       root directory
```

---

# `mkdir`

## Purpose

Creates directories.

```
mkdir testdir
```

Create nested directories:

```
mkdir -p testdir/subdir1/subdir2
```

### Flag

```
-p → create parent directories as required
```

Check:

```
ls -R testdir
```

`-R` means **recursive**.

---

# `rmdir`

## Purpose

Removes empty directories.

```
mkdir emptydir
rmdir emptydir
```

This works because `emptydir` contains nothing.

Try:

```
mkdir nonempty
touch nonempty/file.txt
rmdir nonempty
```

You should receive an error because the directory is not empty.

### Important

`rmdir` does **not** normally delete directories containing files.

---

# `touch`

## Purpose

Creates an empty file or updates a file's timestamp.

```
touch files/newfile.txt
```

Check:

```
ls -l files/newfile.txt
```

Create several files:

```
touch files/a.txt files/b.txt files/c.txt
```

### OS concept

`touch` demonstrates that a file has metadata such as timestamps in addition to its contents.

---

# `cp`

## Purpose

Copies files or directories.

### Copy a file

```
cp files/file1.txt backup/file1_backup.txt
```

Syntax:

```
cp source destination
```

Here:

```
source      → files/file1.txt
destination → backup/file1_backup.txt
```

### Copy a directory

```
cp -r files backup/files_copy
```

`-r` = recursive.

It is required for copying directories and their contents.

Check:

```
ls -R backup/files_copy
```

---

# `mv`

## Purpose

Moves or renames files.

Rename:

```
mv files/newfile.txt files/renamed.txt
```

Move:

```
mv files/renamed.txt backup/
```

### Important concept

Unlike `cp`, `mv` normally does not create a second copy of the file.

---

# `rm`

## Purpose

Removes files.

Create a disposable file:

```
touch files/delete_me.txt
```

Remove:

```
rm files/delete_me.txt
```

### Recursive removal

```
mkdir -p directories/remove_me/subdir
touch directories/remove_me/test.txt

rm -r directories/remove_me
```

```
-r → recursive
```

### Dangerous command

Never casually execute:

```
rm -rf /
```

`-f` means force, while `-r` means recursive.

For this practical, stay inside:

```
~/demo
```

---

# 2. `cat`, `less`, `head`, `tail`

# `cat`

## Purpose

Displays file contents.

```
cat files/file1.txt
```

Multiple files:

```
cat files/file1.txt files/file2.txt
```

---

## Creating a file using `cat`

```
cat > files/cat_demo.txt <<'EOF'
Line 1
Line 2
Line 3
Line 4
Line 5
EOF
```

Then:

```
cat files/cat_demo.txt
```

### What is `>`?

`>` is the **output redirection operator**.

It sends command output into a file.

```
echo "Hello" > example.txt
```

If the file already exists, `>` normally replaces its contents.

---

## Append using `>>`

```
cat >> files/cat_demo.txt <<'EOF'
Line 6
Line 7
EOF
```

`>>` appends instead of replacing.

---

# `less`

Used for viewing long files one screen at a time.

```
less files/cat_demo.txt
```

Useful keys:

```
Space       next page
b           previous page
/word       search
q           quit
```

---

# `head`

Shows the beginning of a file.

```
head files/cat_demo.txt
```

Show exactly three lines:

```
head -n 3 files/cat_demo.txt
```

```
-n 3 → display 3 lines
```

---

# `tail`

Shows the end of a file.

```
tail files/cat_demo.txt
```

Last three lines:

```
tail -n 3 files/cat_demo.txt
```

### Follow a changing file

```
touch logs/app.log
tail -f logs/app.log
```

`-f` = follow.

In another terminal:

```
echo "Application started" >> ~/demo/logs/app.log
```

You should see the new line appear.

Stop with:

```
Ctrl+C
```

---

# 3. `echo`, `date`, `sleep`, `time`

# `echo`

Displays text.

```
echo "Hello Linux"
```

Environment variables:

```
echo $USER
echo $HOME
echo $PATH
```

The `$` means that the shell should expand the variable.

---

## Redirection

```
echo "First line" > files/echo.txt
echo "Second line" >> files/echo.txt

cat files/echo.txt
```

---

# `date`

Displays date and time.

```
date
```

Formatted date:

```
date +"%Y-%m-%d"
```

Important format codes:

```
%Y → four-digit year
%m → month
%d → day
%H → hour
%M → minute
%S → second
```

Example:

```
date +"%H:%M:%S"
```

---

# `sleep`

Pauses execution.

```
sleep 2
```

Demonstration:

```
echo "Starting..."
sleep 2
echo "Finished"
```

The shell waits for two seconds.

---

# `time`

Measures command execution time.

```
time sleep 2
```

Useful OS example:

```
time find . -type f
```

The output usually contains:

```
real
user
sys
```

These represent different aspects of execution time.

---

# 4. `clear`, `history`, `alias`, `unalias`

# `clear`

Clears the terminal.

```
clear
```

Keyboard shortcut:

```
Ctrl+L
```

---

# `history`

Displays previously executed shell commands.

```
history
```

Search:

```
history | grep mkdir
```

Here:

```
|      → pipe
grep   → searches input
mkdir  → search pattern
```

---

# `alias`

Create a shortcut:

```
alias ll='ls -lah'
```

Now:

```
ll
```

Another example:

```
alias demo-pwd='pwd'
demo-pwd
```

List aliases:

```
alias
```

---

# `unalias`

Remove an alias:

```
unalias demo-pwd
```

Now:

```
demo-pwd
```

will normally produce:

```
command not found
```

---

# 5. `find`

`find` is one of the most important Linux commands.

## Basic syntax

```
find [starting-path] [conditions] [actions]
```

---

## Find files

```
find . -type f
```

Breakdown:

```
.       → start in current directory
-type   → specify object type
f       → regular file
```

Find directories:

```
find . -type d
```

```
d → directory
```

---

## Find `.txt` files

```
find . -type f -name "*.txt"
```

Breakdown:

```
-name "*.txt" → filename matching pattern
*             → any sequence of characters
.txt          → filename ending
```

---

## Find recently modified files

```
find . -type f -mmin -10
```

```
-mmin → modification time in minutes
-10   → modified less than 10 minutes ago
```

---

## `find -exec`

```
find files -type f -name "*.txt" -exec wc -l {} \;
```

Breakdown:

```
-exec       → execute another command
wc -l       → count lines
{}          → current file found by find
\;          → marks the end of -exec command
```

This is an important command for practical exams.

---

# 6. `grep`

`grep` searches text.

Create data:

```
cat > text/students.txt <<'EOF'
Alice 85
Bob 72
Charlie 91
David 68
Eve 95
Frank 76
EOF
```

Search:

```
grep "Alice" text/students.txt
```

Case-insensitive:

```
grep -i "alice" text/students.txt
```

```
-i → ignore case
```

Line numbers:

```
grep -n "Alice" text/students.txt
```

```
-n → show line number
```

Invert match:

```
grep -v "Alice" text/students.txt
```

```
-v → show lines that do NOT match
```

Recursive:

```
grep -R "Linux" .
```

```
-R → search recursively through directories
```

---

# 7. File Permissions — `chmod`

Check permissions:

```
ls -l files
```

Create a shell script:

```
cat > files/hello.sh <<'EOF'
#!/bin/bash
echo "Hello from shell script"
EOF
```

Check:

```
ls -l files/hello.sh
```

Make executable:

```
chmod +x files/hello.sh
```

Run:

```
./files/hello.sh
```

---

## Permission model

Example:

```
-rwxr-xr--
```

Breakdown:

```
-    rwx    r-x    r--
│     │      │      │
│     │      │      └── others
│     │      └───────── group
│     └──────────────── owner
└────────────────────── file type
```

Permission characters:

```
r = read
w = write
x = execute
```

---

## Numeric permissions

```
chmod 644 files/file1.txt
```

The numbers represent:

```
4 = read
2 = write
1 = execute
```

Therefore:

```
6 = 4 + 2 = read + write
4 = read
4 = read
```

So:

```
644 = rw-r--r--
```

Another example:

```
chmod 755 files/hello.sh
```

means:

```
7 = rwx
5 = r-x
5 = r-x
```

Therefore:

```
755 = rwxr-xr-x
```

---

## Symbolic permissions

```
chmod u+x files/file1.txt
```

```
u → user/owner
+ → add
x → execute
```

Remove:

```
chmod u-x files/file1.txt
```

Group read:

```
chmod g+r files/file1.txt
```

Others read removal:

```
chmod o-r files/file1.txt
```

```
u → user
g → group
o → others
```

---

# 8. `chown`

`chown` means **change owner**.

Check:

```
ls -l files/file1.txt
```

Change owner to current user:

```
sudo chown $USER:$USER files/file1.txt
```

Breakdown:

```
sudo       → execute with elevated privileges
chown      → change owner
$USER      → current username
:          → separates user and group
```

Recursive:

```
sudo chown -R $USER:$USER backup
```

```
-R → recursive
```

Only use `sudo` when necessary.

---

# 9. `ln` — Links and Inodes

Linux supports hard links and symbolic links.

## Hard link

```
ln files/file1.txt links/file1_hardlink
```

Check inode numbers:

```
ls -li files/file1.txt links/file1_hardlink
```

`-i` displays the inode number.

The two names should have the same inode.

### OS concept

A hard link gives another directory entry referring to the same inode.

---

## Symbolic link

```
ln -s ../files/file1.txt links/file1_symlink
```

```
-s → symbolic link
```

Check:

```
ls -l links
```

Read:

```
cat links/file1_symlink
```

### Difference

```
Hard link:
filename → inode

Symbolic link:
filename → symlink → target path
```

---

# 10. `diff`, `cmp`, `comm`

Create files:

```
cat > text/a.txt <<'EOF'
apple
banana
cherry
EOF

cat > text/b.txt <<'EOF'
apple
banana
orange
EOF
```

## `diff`

```
diff text/a.txt text/b.txt
```

Shows line-oriented differences.

---

## `cmp`

```
cmp text/a.txt text/b.txt
```

Compares files byte-by-byte.

---

## `comm`

`comm` works with sorted files.

```
sort text/a.txt > text/a_sorted.txt
sort text/b.txt > text/b_sorted.txt

comm text/a_sorted.txt text/b_sorted.txt
```

The three output columns represent:

```
only in first file
only in second file
present in both
```

---

# 11. Text Processing

# `cut`

Extract the first field:

```
cut -d' ' -f1 text/students.txt
```

Breakdown:

```
-d' ' → delimiter is a space
-f1   → select field 1
```

Marks:

```
cut -d' ' -f2 text/students.txt
```

---

# `sort`

Alphabetical sorting:

```
sort text/students.txt
```

Sort according to second field numerically:

```
sort -k2 -n text/students.txt
```

```
-k2 → use field 2 as sorting key
-n  → numeric sorting
```

Reverse numeric order:

```
sort -k2 -nr text/students.txt
```

```
-r → reverse
```

---

# `wc`

```
wc text/students.txt
```

Counts:

```
lines
words
bytes
```

Individual options:

```
wc -l text/students.txt
wc -w text/students.txt
wc -c text/students.txt
```

```
-l → lines
-w → words
-c → bytes
```

---

# `paste`

Create:

```
cat > text/names.txt <<'EOF'
Alice
Bob
Charlie
EOF

cat > text/marks.txt <<'EOF'
85
72
91
EOF
```

Combine:

```
paste text/names.txt text/marks.txt
```

Output:

```
Alice   85
Bob     72
Charlie 91
```

---

# `awk`

`awk` is a powerful text-processing language.

Print names:

```
awk '{print $1}' text/students.txt
```

Here:

```
$1 → first field
$2 → second field
```

Print marks:

```
awk '{print $2}' text/students.txt
```

Print formatted output:

```
awk '{print "Student:", $1, "Marks:", $2}' text/students.txt
```

Filter marks:

```
awk '$2 > 80 {print $1, $2}' text/students.txt
```

Calculate average:

```
awk '{sum += $2; count++} END {print "Average:", sum/count}' text/students.txt
```

Important concepts:

```
sum += $2 → add marks
count++   → increment counter
END       → execute after all input is processed
```

---

# `sed`

`sed` is a stream editor.

Replace:

```
sed 's/Linux/UNIX/' files/file3.txt
```

Syntax:

```
s/old/new/
```

Delete line 2:

```
sed '2d' files/cat_demo.txt
```

```
2 → line number
d → delete
```

Global replacement:

```
sed -i 's/Line/ROW/g' files/cat_demo.txt
```

Breakdown:

```
-i → modify file in place
s  → substitute
Line → old text
ROW  → replacement
g    → all occurrences on each line
```

---

# 12. Archives — `tar`

Create archive:

```
tar -cvf backup.tar files/
```

Options:

```
-c → create
-v → verbose
-f → specify archive filename
```

List:

```
tar -tvf backup.tar
```

```
-t → list archive contents
-v → verbose
-f → archive file
```

Extract:

```
mkdir extracted
tar -xvf backup.tar -C extracted
```

```
-x → extract
-C → change to specified directory before extracting
```

---

## Gzip archive

```
tar -czvf backup.tar.gz files/
```

```
-z → gzip compression
```

Extract:

```
mkdir extracted_gz
tar -xzvf backup.tar.gz -C extracted_gz
```

---

# 13. `zip` / `unzip`

Create:

```
zip -r files.zip files/
```

```
-r → recursively include directory contents
```

List:

```
unzip -l files.zip
```

```
-l → list contents
```

Extract:

```
mkdir unzip_demo
unzip files.zip -d unzip_demo
```

```
-d → destination directory
```

---

# 14. Disk Usage — `df` and `du`

# `df`

Shows filesystem disk-space usage.

```
df
```

Human-readable:

```
df -h
```

```
-h → human-readable
```

Typical columns:

```
Filesystem
Size
Used
Avail
Use%
Mounted on
```

---

# `du`

Shows disk usage of files/directories.

```
du .
```

Human-readable:

```
du -h .
```

Summary:

```
du -sh .
```

```
-s → summary
-h → human-readable
```

Largest directories:

```
du -h --max-depth=1 . | sort -h
```

Important:

```
--max-depth=1 → show only one directory level deep
```

---

# 15. System Information

# `uname`

```
uname
```

Kernel name:

```
uname -r
```

All available information:

```
uname -a
```

```
-a → all available information
-r → kernel release
```

---

# `whoami`

```
whoami
```

Shows the current username.

---

# `id`

```
id
```

Shows:

```
UID
GID
groups
```

For a user:

```
id $USER
```

---

# `groups`

```
groups
```

Shows groups associated with the current user.

---

# `uptime`

```
uptime
```

Typically shows:

```
current time
system uptime
number of logged-in users
load average
```

---

# `free`

```
free
```

Human-readable:

```
free -h
```

```
-h → human-readable
```

It provides information about memory and swap.

---

# 16. Environment Variables

# `env`

```
env
```

Displays environment variables.

Specific variables:

```
echo $HOME
echo $USER
echo $PATH
```

---

# `export`

Create an environment variable:

```
export COLLEGE="Operating Systems"
```

Check:

```
echo $COLLEGE
```

Search:

```
env | grep COLLEGE
```

Remove:

```
unset COLLEGE
```

### Important concept

Environment variables can be inherited by child processes created by the shell.

---

# 17. Processes — `ps`

`ps` displays processes.

```
ps
```

More detailed:

```
ps aux
```

Common interpretation:

```
a → processes of all users with terminals
u → user-oriented format
x → include processes without terminals
```

Search:

```
ps aux | grep bash
```

Process tree:

```
ps -ef --forest
```

---

# 18. `top`

Run:

```
top
```

It provides real-time process information.

Important fields include:

```
PID
USER
%CPU
%MEM
COMMAND
```

Exit:

```
q
```

### OS concept

`top` is useful for observing process scheduling and resource consumption.

---

# 19. `htop`

Try:

```
htop
```

If unavailable:

```
sudo apt update
sudo apt install htop
```

Then:

```
htop
```

`htop` provides an interactive process monitor.

---

# 20. Process Control — `kill`

Start a harmless process:

```
sleep 300 &
```

`&` starts the process in the background.

Find it:

```
pgrep -a sleep
```

Terminate it:

```
kill <PID>
```

For example, if the PID is `1234`:

```
kill 1234
```

### Signals

A normal `kill PID` sends:

```
SIGTERM
```

This requests that the process terminate.

Force termination:

```
kill -9 PID
```

`-9` sends `SIGKILL`.

### Important

Prefer normal `kill PID` before `kill -9`.

---

# 21. `killall`

Start examples:

```
sleep 300 &
sleep 300 &
```

Check:

```
pgrep -a sleep
```

Terminate matching processes:

```
killall sleep
```

Be careful: `killall` can terminate **multiple processes** matching the specified name.

---

# 22. `jobs`, `fg`, `bg`

Start:

```
sleep 30 &
```

Check:

```
jobs
```

Foreground:

```
fg
```

Stop with:

```
Ctrl+C
```

### OS/shell concept

A shell can manage **jobs** independently from individual process IDs.

---

# 23. `watch`

Run:

```
watch free -h
```

It repeatedly executes the command.

Exit:

```
Ctrl+C
```

Another example:

```
watch ls -lh files
```

---

# 24. `$!` and Process IDs

Start:

```
sleep 100 &
```

Print the PID of the most recently started background process:

```
echo $!
```

Check:

```
ps -p $!
```

Terminate:

```
kill $!
```

Important:

```
$! → PID of most recently launched background process
```

---

# 25. `strace`

`strace` traces **system calls** made by a program.

Try:

```
strace ls files
```

Or:

```
strace echo "Hello"
```

You may see calls such as:

```
openat()
read()
write()
close()
```

### OS concept

This demonstrates:

```
User program
      ↓
Library / system-call interface
      ↓
System call
      ↓
Kernel
```

Install if necessary:

```
sudo apt install strace
```

---

# 26. `lsof`

`lsof` means **list open files**.

```
lsof
```

Specific file:

```
lsof files/file1.txt
```

Network-related open files:

```
sudo lsof -i
```

### OS concept

Linux treats many resources as file descriptors/files. `lsof` helps inspect which processes have resources open.

---

# 27. `dmesg`

Displays kernel messages:

```
dmesg | head
```

Depending on WSL configuration:

```
sudo dmesg | head
```

Search:

```
sudo dmesg | grep -i usb
```

```
-i → case-insensitive search
```

WSL may expose a different or more limited kernel environment than a traditional Linux installation.

---

# 28. `vmstat`

Basic:

```
vmstat
```

Periodic monitoring:

```
vmstat 2 5
```

Parameters:

```
2 → interval in seconds
5 → number of reports
```

Thus:

```
vmstat 2 5
```

means:

> Display virtual-memory/system statistics every 2 seconds, five times.

---

# 29. `iostat`

If installed:

```
iostat
```

Install:

```
sudo apt install sysstat
```

Detailed example:

```
iostat -xz 2 3
```

Common options:

```
-x → extended statistics
-z → omit devices with no activity
```

Parameters:

```
2 → interval
3 → number of reports
```

---

# 30. Networking — `ip`

`ip` is the modern Linux networking utility.

Show addresses:

```
ip a
```

`a` means `address`.

Equivalent longer form:

```
ip addr
```

Interfaces:

```
ip link
```

Routing table:

```
ip route
```

Specific interface:

```
ip addr show eth0
```

### OS concept

Networking involves:

```
Network interface
       ↓
IP address
       ↓
Routing table
       ↓
Network
```

---

# 31. `ifconfig`

Older networking utility:

```
ifconfig
```

If unavailable:

```
sudo apt install net-tools
```

Then:

```
ifconfig
```

For modern Linux, learn `ip` as the preferred tool.

---

# 32. `ping`

Test connectivity:

```
ping -c 4 google.com
```

```
-c 4 → send four packets
```

Loopback:

```
ping -c 4 127.0.0.1
```

`127.0.0.1` is the IPv4 loopback address.

---

# 33. `ss`

Show sockets:

```
ss -l
```

TCP:

```
ss -t
```

TCP listening sockets:

```
ss -tln
```

All TCP/UDP listening sockets:

```
ss -tuln
```

Breakdown:

```
-t → TCP
-u → UDP
-l → listening
-n → numeric addresses/ports
```

---

# 34. `netstat`

Older networking utility:

```
netstat -tuln
```

If unavailable:

```
sudo apt install net-tools
```

---

# 35. `curl`

Fetch a webpage:

```
curl https://example.com
```

Headers only:

```
curl -I https://example.com
```

```
-I → HEAD request / response headers
```

Save output:

```
curl -o example.html https://example.com
```

```
-o → output filename
```

---

# 36. `wget`

Download:

```
wget https://example.com
```

Specify filename:

```
wget -O example2.html https://example.com
```

```
-O → output filename
```

---

# 37. `nslookup`

DNS lookup:

```
nslookup example.com
```

It demonstrates the relationship between:

```
Domain name → DNS → IP address
```

---

# 38. `traceroute`

Trace network path:

```
traceroute example.com
```

If missing:

```
sudo apt install traceroute
```

The command attempts to show intermediate network hops between your system and the destination.

---

# 39. `nc` — Netcat

`nc` is useful for demonstrating client-server communication.

### Terminal 1

```
nc -l 5000
```

### Terminal 2

```
nc localhost 5000
```

Type:

```
Hello from client
```

The text should appear in Terminal 1.

Breakdown:

```
nc       → netcat
-l       → listen
5000     → port number
localhost → local machine
```

Stop with:

```
Ctrl+C
```

---

# 40. `tcpdump`

List interfaces:

```
sudo tcpdump -D
```

Capture loopback:

```
sudo tcpdump -i lo
```

Generate traffic:

```
ping -c 3 127.0.0.1
```

Stop:

```
Ctrl+C
```

Filter ICMP:

```
sudo tcpdump -i lo icmp
```

### OS/networking concept

This demonstrates packet-level network observation.

---

# 41. `iptables`

Inspect rules only:

```
sudo iptables -L -n -v
```

Breakdown:

```
-L → list rules
-n → don't resolve addresses/ports to names
-v → verbose output
```

For an assignment, inspection is sufficient.

Do **not** randomly add firewall rules to WSL.

Modern systems may use:

```
sudo nft list ruleset
```

---

# 42. `ufw`

Check status:

```
sudo ufw status
```

Verbose:

```
sudo ufw status verbose
```

Do not blindly enable or modify firewall settings in WSL.

---

# 43. SSH

Check SSH client:

```
ssh -V
```

If an SSH server is actually configured:

```
ssh localhost
```

Remote syntax:

```
ssh username@hostname
```

Exit:

```
exit
```

Concept:

```
SSH → Secure Shell
```

It provides encrypted remote terminal access.

---

# 44. `scp`

Copy a file to a remote system:

```
scp files/file1.txt username@hostname:/path/to/destination/
```

General syntax:

```
scp source user@host:destination
```

Copy directory:

```
scp -r files username@hostname:/path/to/destination/
```

```
-r → recursive
```

Only execute remote examples if you actually have an SSH server and permission to use it.

---

# 45. `rsync`

Create:

```
mkdir -p sync_demo
```

Synchronize:

```
rsync -av files/ sync_demo/
```

Options:

```
-a → archive mode
-v → verbose
```

Run again:

```
rsync -av files/ sync_demo/
```

The second synchronization should have less work because unchanged files do not need to be transferred again.

---

# 46. `ftp`

Check:

```
ftp
```

Conceptually:

```
ftp hostname
```

Common commands:

```
user
ls
get file
put file
bye
```

FTP is an older file-transfer protocol and does not provide the same security model as SFTP/SSH.

---

# 47. `sftp`

Connect:

```
sftp username@hostname
```

Inside SFTP:

```
ls
pwd
lpwd
get file.txt
put file.txt
exit
```

Important distinction:

```
FTP  → File Transfer Protocol
SFTP → SSH File Transfer Protocol
```

---

# 48. Users — `id` and `groups`

```
whoami
id
groups
```

Current user:

```
id $USER
```

### OS concept

Linux uses:

```
UID → user ID
GID → group ID
```

These IDs are used in permission checks.

---

# 49. `useradd` / `adduser`

Only perform user-management experiments if you have permission.

Create:

```
sudo useradd -m demo_user
```

```
-m → create home directory
```

Check:

```
id demo_user
```

Ubuntu's interactive command:

```
sudo adduser demo_user2
```

---

# 50. `passwd`

Set password:

```
sudo passwd demo_user
```

Password status:

```
sudo passwd -S demo_user
```

---

# 51. `usermod`

Add user to a supplementary group:

```
sudo usermod -aG sudo demo_user
```

Breakdown:

```
-a → append
-G → supplementary groups
```

Check:

```
groups demo_user
```

### Important

Do not omit `-a` casually when modifying supplementary groups. Without append semantics, existing supplementary group memberships can be replaced.

---

# 52. `deluser` / `userdel`

Delete:

```
sudo deluser demo_user
```

Alternative:

```
sudo userdel demo_user
```

Delete with home directory:

```
sudo userdel -r demo_user
```

```
-r → remove associated home/mail files where applicable
```

Use carefully.

---

# 53. `chage`

Display password-aging information:

```
sudo chage -l demo_user2
```

Set maximum age:

```
sudo chage -M 90 demo_user2
```

```
-M → maximum password age
```

Force password change:

```
sudo chage -d 0 demo_user2
```

```
-d → last password-change date
0  → force change at next login
```

---

# 54. `newgrp`

Check:

```
groups
```

Switch group context:

```
newgrp sudo
```

Check:

```
id
```

Exit:

```
exit
```

---

# 55. `sudo`

Run a command with elevated privileges:

```
sudo whoami
```

Expected:

```
root
```

Open root shell:

```
sudo -i
```

Check:

```
whoami
```

Return:

```
exit
```

### Important OS concept

`sudo` provides controlled privilege elevation.

---

# 56. `mount`

View mounted filesystems:

```
mount
```

A more readable alternative:

```
findmnt
```

Check Windows mounts:

```
ls /mnt
```

Usually:

```
ls /mnt/c
```

### Concept

Mounting makes a filesystem accessible through a directory in the Linux filesystem hierarchy.

Do **not** randomly mount or unmount devices in WSL.

---

# 57. `umount`

General syntax:

```
sudo umount /path/to/mount
```

Concept:

```
mount   → attach filesystem
umount  → detach filesystem
```

Do not experiment by unmounting essential WSL filesystems such as `/` or `/mnt/c`.

---

# 58. `systemctl`

Check:

```
systemctl
```

WSL may not have systemd enabled.

If systemd is enabled:

```
systemctl status
```

Example:

```
systemctl status ssh
```

### Concept

`systemctl` is the main command-line interface for controlling systemd services.

---

# 59. `service`

Depending on the environment:

```
service --status-all
```

Example:

```
sudo service ssh status
```

`service` is associated with traditional service-management interfaces and may act as a compatibility layer on systems using systemd.

---

# 60. `journalctl`

If systemd/journald is available:

```
journalctl
```

Last 20 entries:

```
journalctl -n 20
```

```
-n 20 → show 20 recent entries
```

Follow logs:

```
journalctl -f
```

```
-f → follow new entries
```

Exit:

```
Ctrl+C
```

---

# 61. `man`

Manual pages are one of the most important Linux learning tools.

```
man ls
```

Search inside the manual:

```
/recursive
```

Exit:

```
q
```

Other useful pages:

```
man chmod
man grep
man ps
man fork
```

### Important

When you don't know a command's options:

```
man command
```

is often the first place to look.

---

# 62. `whereis`

```
whereis ls
whereis bash
whereis python
```

Shows locations associated with a command, such as binaries and manual pages.

---

# 63. `whatis`

```
whatis ls
whatis grep
whatis chmod
whatis ps
```

Provides a short description from the manual-page database.

---

# 64. `cal`

```
cal
```

If unavailable:

```
sudo apt install ncal
```

Then:

```
cal
```

---

# 65. Package Management — `apt`

For Ubuntu/Debian WSL, `apt` is the relevant package manager.

Search:

```
apt search htop
```

Install:

```
sudo apt install htop
```

Update package information:

```
sudo apt update
```

Upgrade installed packages:

```
sudo apt upgrade
```

Check installed package:

```
apt list --installed | grep htop
```

Remove:

```
sudo apt remove htop
```

### Important distinction

```
apt update
```

updates package information.

It does **not** mean that installed software itself has been upgraded.

```
apt upgrade
```

actually upgrades installed packages.

---

# 66. `pacman`

Used by Arch-based distributions.

```
pacman -Q
pacman -Ss htop
sudo pacman -S htop
```

Options:

```
-Q  → query installed packages
-Ss → search package repositories
-S  → synchronize/install packages
```

**Do not run these on Ubuntu/Debian WSL.**

---

# 67. `yum`

Used by older RPM-based systems.

```
yum search htop
sudo yum install htop
```

Do not use this on Ubuntu/Debian.

---

# 68. `rpm`

Query installed RPM packages:

```
rpm -qa
```

```
-q → query
-a → all
```

Query a specific package:

```
rpm -q bash
```

---

# 69. `dd`

`dd` is a low-level data-copying utility.

Create a safe 10 MB test image:

```
dd if=/dev/zero of=files/test.img bs=1M count=10
```

Breakdown:

```
dd          → data-copy utility
if=         → input file
/dev/zero   → source of zero bytes
of=         → output file
bs=1M       → block size = 1 MiB
count=10    → copy 10 blocks
```

Check:

```
ls -lh files/test.img
```

Copy it:

```
dd if=files/test.img of=files/test_copy.img bs=1M
```

Compare:

```
cmp files/test.img files/test_copy.img
```

### Extremely important

Never casually use `dd` against actual disks such as:

```
/dev/sda
/dev/nvme0n1
```

A wrong `dd` command can destroy data.

---

# 70. `watch` + `date`

Run:

```
watch -n 1 date
```

Breakdown:

```
watch → repeatedly execute command
-n 1  → wait 1 second between executions
date  → command being executed
```

Exit:

```
Ctrl+C
```

---

# 71. `shutdown` / `reboot`

Know the syntax for the assignment:

```
sudo shutdown
```

```
sudo shutdown now
```

```
sudo reboot
```

However, do **not** casually execute these inside your WSL environment.

WSL is not identical to a traditional physical Linux machine.

---

# 72. Useful Command Combinations

Linux becomes particularly powerful when commands are combined.

## Processes containing bash

```
ps aux | grep bash
```

Concept:

```
ps → generate process information
|
grep → filter the output
```

---

## Largest directories

```
du -h --max-depth=1 . | sort -h
```

Pipeline:

```
du → produces disk usage
 |
sort → sorts the result
```

---

## Find text files containing Linux

```
find . -type f -name "*.txt" -exec grep -l "Linux" {} \;
```

Important:

```
find → locate files
-exec → execute command
grep → search text
-l → print matching filenames
{} → current find result
\; → end of exec command
```

---

## Count files

```
find . -type f | wc -l
```

This means:

```
find files
   ↓
pipe
   ↓
count lines
```

Each matching filename is generally one line.

---

## Count lines across text files

```
find . -name "*.txt" -exec wc -l {} +
```

Here:

```
+ → allows find to pass multiple files to one invocation
```

This differs from:

```
\;
```

which generally invokes the command separately for each result.

---

## List only `.txt` files

```
find . -type f -name "*.txt" | sort
```

---

## Search command history

```
history | grep chmod
```

---

## Show listening network ports

```
ss -tuln
```

---

## Show memory

```
free -h
```

---

## Show disk space

```
df -h
```

---

# 73. Compact Practical Demonstration

The following is a useful sequence to actually execute during a practical session.

```
cd ~
mkdir -p demo/{files,backup,text,logs,links}
cd demo

# Basic information
pwd
ls -la
date
whoami
uname -a

# Create files
echo "Hello Linux" > files/file1.txt
echo "Operating Systems Assignment" > files/file2.txt
echo "Linux is powerful" > files/file3.txt

touch files/file4.txt

# Read files
cat files/file1.txt
head files/file1.txt
tail files/file1.txt

# Copy and move
cp files/file1.txt backup/
mv files/file4.txt files/renamed.txt

# Links
ln files/file1.txt links/hardlink
ln -s ../files/file1.txt links/symlink
ls -li files/file1.txt links/

# Permissions
chmod 644 files/file1.txt
chmod 755 files/file2.txt
ls -l files/

# Search
find . -type f
find . -name "*.txt"
grep -R "Linux" .

# Text processing
cat > text/students.txt <<'EOF'
Alice 85
Bob 72
Charlie 91
David 68
Eve 95
EOF

cat text/students.txt
cut -d' ' -f1 text/students.txt
sort -k2 -n text/students.txt
awk '$2 > 80 {print $1, $2}' text/students.txt
wc -l text/students.txt

# Archive
tar -czvf backup.tar.gz files/
mkdir extracted
tar -xzvf backup.tar.gz -C extracted

# Disk
df -h
du -sh .

# Processes
ps
ps aux | head

sleep 60 &
ps aux | grep "[s]leep"
kill $!

# Memory/system
free -h
uptime
vmstat 1 3

# Networking
ip a
ip route
ss -tuln
ping -c 4 127.0.0.1
curl -I https://example.com

# Environment
echo $HOME
echo $PATH

export DEMO="Operating Systems"
echo $DEMO
unset DEMO

# History
history | tail -20
```

---

# 74. OS Viva — Important Concepts Behind the Commands

## Process

A running program is represented by a process.

Useful commands:

```
ps
top
kill
jobs
```

---

## PID

Every process has a process ID.

Example:

```
sleep 100 &
echo $!
```

`$!` gives the most recently started background process ID.

---

## File descriptor

Linux processes use file descriptors for resources such as:

```
0 → standard input
1 → standard output
2 → standard error
```

This is related to:

```
lsof
```

and shell redirection.

---

## Pipe

The pipe:

```
|
```

connects the output of one command to the input of another.

Example:

```
ps aux | grep bash
```

Conceptually:

```
ps aux
  │
  │ stdout
  ▼
 pipe
  │
  ▼
grep bash
```

---

## Redirection

Output:

```
command > file
```

Append:

```
command >> file
```

Input:

```
command < file
```

Error redirection:

```
command 2> error.txt
```

Both standard output and standard error:

```
command > output.txt 2>&1
```

---

## Inode

A Linux inode contains metadata associated with a filesystem object.

Hard links demonstrate inode relationships:

```
ln files/file1.txt links/hardlink
ls -li files/file1.txt links/hardlink
```

---

## Permissions

Linux permissions are generally represented for:

```
owner
group
others
```

and include:

```
read
write
execute
```

Example:

```
755
```

means:

```
owner  → rwx
group  → r-x
others → r-x
```

---

## System Call

A user program cannot directly perform every privileged kernel operation.

A simplified model is:

```
User program
     ↓
System call
     ↓
Kernel
     ↓
Hardware/resource
```

`strace` lets you observe system calls made by programs.

Example:

```
strace ls
```

---

## User and Kernel Space

A simplified Linux architecture:

```
+-----------------------------+
|        User Programs        |
| ls, grep, bash, Python etc. |
+-----------------------------+
              |
              | system calls
              ▼
+-----------------------------+
|           Kernel            |
| processes | memory | files  |
| networking | drivers | IPC  |
+-----------------------------+
              |
              ▼
+-----------------------------+
|          Hardware           |
+-----------------------------+
```

---

# 75. Commands to Memorize for an OS Practical

Prioritize these commands:

```
pwd
ls
cd
mkdir
rmdir
touch
cp
mv
rm

cat
head
tail
less

find
grep

chmod
chown
ln

tar
df
du
dd

ps
top
kill
killall
jobs
fg
bg
sleep

whoami
id
groups
sudo
useradd
passwd
usermod
chage

mount
umount

ip
ping
ss
netstat
curl
wget
traceroute
nslookup
nc

awk
sed
cut
sort
wc
diff
cmp
comm

strace
lsof
dmesg
vmstat

man
whereis
whatis
uname

env
export
alias
history
```

---

# 76. Flag Cheat Sheet

Some of the most frequently encountered flags in this practical:

|Flag|Common meaning|Example|
|---|---|---|
|`-a`|all|`ls -a`|
|`-l`|long format|`ls -l`|
|`-h`|human-readable|`df -h`|
|`-r`|recursive/reverse depending on command|`cp -r`|
|`-R`|recursive|`grep -R`|
|`-v`|verbose/reverse depending on command|`tar -v`|
|`-f`|force/file/follow depending on command|`rm -f`|
|`-n`|number/numeric/newline depending on command|`grep -n`|
|`-i`|ignore case/inode depending on command|`grep -i`|
|`-x`|command-specific|`iostat -x`|
|`-t`|time/TCP depending on command|`ls -t`, `ss -t`|
|`-u`|user/UDP depending on command|`ps -u`, `ss -u`|
|`-s`|summary/silent depending on command|`du -s`|
|`-d`|directory/date/destination depending on command|command-specific|
|`-p`|parent/preserve/port depending on command|command-specific|

### Important warning

A flag does **not** have one universal meaning across Linux commands.

For example:

```
ls -t
```

means sort by time.

But:

```
ss -t
```

means TCP.

Therefore, always check:

```
man command
```

or:

```
command --help
```

---

# 77. Most Important Command Syntax Pattern

Many Linux commands follow this general structure:

```
command [options] [arguments]
```

Example:

```
grep -n "Linux" files/file1.txt
```

Breakdown:

```
grep             → command
-n               → option
"Linux"          → search pattern/argument
files/file1.txt  → input file/argument
```

Another:

```
cp -r files backup/
```

Breakdown:

```
cp       → command
-r       → option
files    → source argument
backup/  → destination argument
```

Understanding this structure is more useful than memorizing commands blindly.

---

# 78. WSL Safety Rules

For this practical, avoid blindly executing:

```
reboot
shutdown
mount
umount
iptables
systemctl
```

Also be extremely careful with:

```
rm -rf
dd
chmod
chown
userdel
```

Keep experiments inside:

```
~/demo
```

A useful safety habit is:

```
pwd
```

before destructive operations.

For example:

```
pwd
ls
```

Then verify that you are actually inside:

```
/home/your_username/demo
```

before removing or modifying files.

---

# 79. Final Practical Workflow

For most demonstrations, start with:

```
cd ~/demo
pwd
ls -la
```

Perform the experiment.

Then verify the result using:

```
ls
cat
grep
find
ps
```

or another appropriate inspection command.

This follows an important Linux learning pattern:

```
Create
   ↓
Modify
   ↓
Inspect
   ↓
Verify
```

For example:

```
touch files/test.txt
ls -l files/test.txt
chmod 755 files/test.txt
ls -l files/test.txt
```

This is better for an assignment than merely memorizing:

```
chmod 755
```

because you can explain **what changed and why**.

# Conclusion

The commands in this practical cover several major Operating Systems concepts:

```
Linux Shell
    │
    ├── Files & Directories
    ├── Permissions & Ownership
    ├── Inodes & Links
    ├── Processes
    ├── Signals
    ├── Users & Groups
    ├── Memory
    ├── Filesystems
    ├── System Calls
    ├── Networking
    ├── Package Management
    └── Shell Scripting / Text Processing
```

For an OS assignment, understanding the relationship between the command and the underlying operating-system concept is more important than memorizing every available option.

A good practical answer should therefore be able to explain:

```
What does the command do?
        ↓
What do its flags mean?
        ↓
What arguments does it receive?
        ↓
What output should I expect?
        ↓
What OS concept does it demonstrate?
```