# OverTheWire — Bandit Writeups

**Platform:** OverTheWire (overthewire.org)
**Status:** ✅ Completed (Level 0 → 31)
**Category:** Linux Wargame (Beginner)

---

## 🎯 What is Bandit?
Bandit is aimed at absolute beginners to teach Linux basics
needed to play other wargames. Each level builds on the previous.

## 🧠 What I Learned Overall
- Linux file navigation and commands
- File permissions and SUID binaries
- Networking basics (ports, nc, telnet, SSL/TLS)
- Git fundamentals
- Cron jobs and schedulers
- Encoding and compression (base64, gzip, bzip2, hexdump)
- SSH keys and custom shells

---

## 📁 Notes Split By
- level-00-10.md → Basic Linux + file operations
- level-11-20.md → Encoding + networking + SSH
- level-21-31.md → Cron + Git + advanced techniques

# Bandit — Levels 0 to 10

---

## Level 0 — SSH Login
**Goal:** Log into the game via SSH
**Cmd:**
ssh bandit0@bandit.labs.overthewire.org -p 2220
**Password:** bandit0
**Learned:** SSH basics, how to connect to remote servers

---

## Level 0 → 1
**Goal:** Read a file in home directory
**Cmd:** ls, cat README
**Password:** ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
**Learned:** Basic ls and cat commands

---

## Level 1 → 2
**Goal:** Read file named `-` (dash)
**Cmd:** cat ./-
**Password:** 263JGJPfgU6LtdEvgfWU1XP5yac29mFx
**Learned:** How to handle special character filenames
**Key Trick:** Use `./` prefix to avoid dash being read as a flag

---

## Level 2 → 3
**Goal:** Read file with spaces in filename
**Cmd:** cat "spaces in this filename"
**Password:** MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
**Learned:** Use quotes around filenames with spaces

---

## Level 3 → 4
**Goal:** Read hidden file inside inhere directory
**Cmd:**
cd inhere
ls -la
cat ./"...Hiding-From-You"
**Password:** 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
**Learned:** `ls -la` shows hidden files (dot files)

---

## Level 4 → 5
**Goal:** Find human-readable file in inhere directory
**Cmd:**
cd inhere
file ./*
cat ./-file07
**Password:** 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
**Learned:** `file` command identifies file types

---

## Level 5 → 6
**Goal:** Find file that is human-readable, 1033 bytes, not executable
**Cmd:**
find . -type f ! -executable -size 1033c
cat ./inhere/.file2
**Password:** HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
**Learned:** `find` command with multiple filters
**Key Flags:**
- `-size 1033c` → size in bytes
- `! -executable` → not executable
- `-type f` → files only

---

## Level 6 → 7
**Goal:** Find file owned by bandit7, group bandit6, 33 bytes
**Cmd:**
find / -size 33c -group bandit6 -user bandit7 2>/dev/null
**Password:** morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
**Learned:** Search by ownership + `2>/dev/null` hides errors
**Key Trick:** `2>/dev/null` redirects stderr to suppress permission errors

---

## Level 7 → 8
**Goal:** Find password next to word "millionth" in data.txt
**Cmd:**
cat data.txt | grep "millionth"
**Password:** dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
**Learned:** `grep` to search inside files

---

## Level 8 → 9
**Goal:** Find line that occurs only once in data.txt
**Cmd:**
sort data.txt | uniq -u
**Password:** 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
**Learned:** `sort` + `uniq -u` to find unique lines
**Also Learned:** stdin, stdout, stderr and file descriptors

---

## Level 9 → 10
**Goal:** Find human-readable strings preceded by = characters
**Cmd:**
strings data.txt | grep "="
**Password:** FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
**Learned:** `strings` extracts readable text from binary files

# Bandit — Levels 11 to 20

---

## Level 10 → 11
**Goal:** Decode base64 encoded data.txt
**Cmd:**
cat data.txt | base64 -d
**Password:** dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
**Learned:** base64 encoding/decoding

---

## Level 11 → 12
**Goal:** ROT13 encoded file — all letters rotated by 13
**Cmd:**
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
**Password:** 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
**Learned:** ROT13 cipher and `tr` command
**Key Concept:**
- 'A-Za-z'       → characters to convert FROM
- 'N-ZA-Mn-za-m' → characters to convert INTO (shifted 13)

---

## Level 12 → 13
**Goal:** Hexdump file compressed multiple times — decompress all
**Cmd:**
xxd -r data.txt > file
mv file file.gz → gzip -d file.gz
mv file file.bz2 → bzip2 -d file.bz2
mv file file.tar → tar -xf file.tar
(repeat until data8.bin)
**Password:** FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
**Learned:** gzip, bzip2, tar, xxd, hexdump format
**Key Commands:**
- `xxd -r` → reverse hexdump to binary
- `file` → always check type before decompressing
- `gzip -d` / `bzip2 -d` / `tar -xf` → decompress each layer

---

## Level 13 → 14
**Goal:** Login using SSH private key instead of password
**Cmd:**
ssh bandit14@bandit.labs.overthewire.org -i sshkey.private -p 2220
**Password:** MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
**Learned:** SSH key-based authentication
**Key Concept:** `-i` flag specifies identity/key file

---

## Level 14 → 15
**Goal:** Submit password to port 30000 on localhost
**Cmd:**
nc localhost 30000
(paste password)
**Password:** 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
**Learned:** netcat (nc), localhost, ports, telnet basics

---

## Level 15 → 16
**Goal:** Submit password to port 30001 using SSL/TLS
**Cmd:**
openssl s_client -connect localhost:30001
**Password:** kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
**Learned:** SSL/TLS, OpenSSL client, encrypted connections

---

## Level 16 → 17
**Goal:** Find port speaking SSL among 31000-32000, get credentials
**Cmd:**
nmap -sV localhost -p 31000-32000
openssl s_client -connect localhost:<correct_port> -quiet
**Password:** Got RSA private key → EReVavePLFHtFlFsjn3hyzMlvSuSAcRD
**Learned:** Port scanning with nmap, SSL detection

---

## Level 17 → 18
**Goal:** Find difference between passwords.old and passwords.new
**Cmd:**
diff passwords.old passwords.new
**Password:** x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
**Learned:** `diff` command to compare files

---

## Level 18 → 19
**Goal:** Read readme — but .bashrc logs you out on SSH login
**Cmd:**
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
**Password:** cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
**Learned:** Pass commands directly via SSH without full login
**Key Trick:** `ssh user@host command` runs command without interactive shell

---

## Level 19 → 20
**Goal:** Use SUID binary to read password file
**Cmd:**
./bandit20-do cat /etc/bandit_pass/bandit20
**Password:** 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
**Learned:** SUID (Set User ID) concept
**Key Concept:**
- Normal → program runs as current user
- SUID → program runs as file OWNER
- ls -la shows `s` in permissions = SUID is set


# Bandit — Levels 21 to 31

---

## Level 20 → 21
**Goal:** Use suconnect binary — sends password, receives next one
**Cmd:**
echo -n "PASSWORD" | nc -l -p 1234 &
./suconnect 1234
**Password:** EeoULMCra2q0dSkYj561DX7s1CpBuOBt
**Learned:** netcat listener, background processes with `&`
**Key Concept:**
- `nc -l -p 1234` → listen on port 1234
- `&` → run process in background

---

## Level 21 → 22
**Goal:** Find what cron job is running and where output goes
**Cmd:**
cat /etc/cron.d/cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/<filename from script>
**Password:** tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
**Learned:** Cron jobs, crontab, automatic task scheduling

---

## Level 22 → 23
**Goal:** Understand cron script logic and predict output filename
**Cmd:**
cat /usr/bin/cronjob_bandit23.sh
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
cat /tmp/<md5 output>
**Password:** 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
**Learned:** md5sum hashing, cut command, reading scripts
**Pipeline:**
echo text → md5sum (makes hash) → cut (removes extra text)

---

## Level 23 → 24
**Goal:** Write a script that cron will execute as bandit24
**Cmd:**
mkdir /tmp/mydir
(write script to copy bandit24 password to /tmp/mydir/pass)
chmod 777 /tmp/mydir
(wait for cron to run)
cat /tmp/mydir/pass
**Password:** gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
**Learned:** Writing shell scripts, cron execution, file permissions

---

## Level 24 → 25
**Goal:** Brute force 4-digit pincode on port 30002
**Cmd:**
(write bash for loop 0000-9999)
for i in $(seq 0000 9999); do echo "PASSWORD $i"; done | nc localhost 30002
**Password:** iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
**Learned:** Bash for loops, brute forcing, piping to netcat

---

## Level 25 → 26
**Goal:** Break out of restricted shell (not /bin/bash)
**Cmd:**
cat /etc/passwd | grep bandit26   → sees custom shell
(shrink terminal window small)
ssh -i bandit26.sshkey bandit26@localhost -p 2220
(more pager appears → press v → opens vi)
:set shell=/bin/bash
:shell
cat /etc/bandit_pass/bandit26
**Password:** s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ
**Learned:** Custom shells, restricted environments, vi shell escape
**Key Concepts:**
- Not all users get /bin/bash
- Programs like `more` and `vi` can be abused to escape
- `:!bash` or `:set shell` in vi spawns a shell
- Think like attacker — find escape paths in programs

---

## Level 26 → 27
**Goal:** Use SUID binary to get bandit27 password
**Cmd:**
./bandit27-do cat /etc/bandit_pass/bandit27
**Password:** upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
**Learned:** SUID binary exploitation (same concept as level 19)

---

## Level 27 → 28
**Goal:** Clone git repo and read README
**Cmd:**
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
cat README
**Password:** Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
**Learned:** git clone over SSH

---

## Level 28 → 29
**Goal:** Password hidden in git commit history
**Cmd:**
git clone <repo>
git log
git checkout <commit-id>
cat README.md
**Password:** 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
**Learned:** git log, git checkout, reading commit history

---

## Level 29 → 30
**Goal:** Password on a different git branch
**Cmd:**
git clone <repo>
git branch -a
git checkout remotes/origin/dev
cat README.md
**Password:** qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
**Learned:** git branches, remote branches, git checkout

---

## Level 30 → 31
**Goal:** Password hidden in git tags
**Cmd:**
git clone <repo>
git ls-remote origin
git show <tag-name>
**Password:** fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
**Learned:** git tags — hidden references in repositories

---

## Level 31 → 32
**Goal:** Push a file to git repository
**Cmd:**
(create key.txt with required content)
git add -f key.txt
git commit -m "add key"
git push origin master
**Password:** 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
**Learned:** git add, commit, push workflow