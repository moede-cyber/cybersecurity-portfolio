# OverTheWire Bandit Progress - Week 4

## What is Bandit?
Bandit is a wargame on overthewire.org that teaches Linux commands
through puzzles. Each level has a password hidden somewhere.
You find it using Linux commands, then use it to log into the next level.

## How to Connect
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
- Replace bandit0 with the level you are on (bandit1, bandit2 etc)
- Password for Level 0 is: bandit0
- Port is always 2220

## My Progress

---

### Level 0 → Level 1
**Goal:** Log in and read the readme file

**Command used:**
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
cat readme
```
**What I learned:** How to connect via SSH and read a file using cat

**Password found:** (write the password here)

---

### Level 1 → Level 2
**Goal:** Read a file called - (dash in the name)

**Command used:**
```bash
cat ./-
```
**What I learned:** Files starting with - need ./ before them so Linux
does not confuse them with a command flag

**Password found:**

---

### Level 2 → Level 3
**Goal:** Read a file with spaces in the name

**Command used:**
```bash
cat "spaces in this filename"
```
or
```bash
cat spaces\ in\ this\ filename
```
**What I learned:** Files with spaces need quotes around the name
or a backslash before each space

**Password found:**

---

### Level 3 → Level 4
**Goal:** Find a hidden file inside a folder called inhere

**Command used:**
```bash
cd inhere
ls -la
cat .hidden
```
**What I learned:** ls -la shows hidden files (files starting with a dot).
Plain ls does not show them.

**Password found:**

---

### Level 4 → Level 5
**Goal:** Find the only human-readable file in the inhere folder

**Command used:**
```bash
cd inhere
file ./-file0*
cat ./-file07
```
**What I learned:** The file command tells you what type each file is.
Human-readable means ASCII text. Binary files are not readable.

**Password found:**

---

### Level 5 → Level 6
**Goal:** Find a file that is human-readable, 1033 bytes, and not executable

**Command used:**
```bash
find . -size 1033c -not -executable
cat ./inhere/maybehere07/.file2
```
**What I learned:** The find command searches by size (-size), 
permissions (-not -executable), and file type.
c after the number means bytes.

**Password found:**

---

### Level 6 → Level 7
**Goal:** Find a file owned by user bandit7, group bandit6, 33 bytes

**Command used:**
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```
**What I learned:** 2>/dev/null hides error messages so you only
see the result you need. Always use this when searching the whole system.

**Password found:**

---

### Level 7 → Level 8
**Goal:** Find the password next to the word millionth in data.txt

**Command used:**
```bash
grep millionth data.txt
```
**What I learned:** grep searches inside files for a specific word
and shows the line it is on. Essential command for any cybersecurity work.

**Password found:**

---

### Level 8 → Level 9
**Goal:** Find the one line that appears only once in data.txt

**Command used:**
```bash
sort data.txt | uniq -u
```
**What I learned:** sort puts lines in order. uniq -u shows only
lines that appear exactly once. The pipe | sends output of one
command into the next.

**Password found:**

---

### Level 9 → Level 10
**Goal:** Find human-readable strings in data.txt preceded by = signs

**Command used:**
```bash
strings data.txt | grep ===
```
**What I learned:** strings extracts readable text from any file
including binary files. Combined with grep to filter results.

**Password found:**

---

## Commands Learned So Far
| Command | What it does |
|---------|-------------|
| ssh | Connect to a remote server |
| cat | Read a file |
| ls | List files |
| ls -la | List all files including hidden ones |
| cd | Move into a folder |
| file | Check what type a file is |
| find | Search for files by name, size, owner |
| grep | Search inside a file for a word |
| sort | Sort lines alphabetically |
| uniq -u | Show only unique lines |
| strings | Extract readable text from any file |
| 2>/dev/null | Hide error messages |

## Levels Still To Complete
- [ ] Level 10
- [ ] Level 11
- [ ] Level 12
- [ ] Level 13
- [ ] Level 14
- [ ] Level 15
