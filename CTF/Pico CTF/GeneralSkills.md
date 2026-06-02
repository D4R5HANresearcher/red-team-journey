# PicoCTF — picoGym Beginner Complete Notes
# Challenge 01 to 24

**Platform:** PicoCTF / picoGym
**Section:** The Beginner's Guide to the picoGym
**Status:** ✅ Completed
**Author:** Darshan

---

## 📋 Full Challenge List

| # | Challenge | Category | Difficulty | Status |
|---|-----------|----------|------------|--------|
| 01 | Obedient Cat | General Skills | Easy | ✅ |
| 02 | Super SSH | General Skills | Easy | ✅ |
| 03 | What's a netcat? | General Skills | Easy | ✅ |
| 04 | Mod 26 | Cryptography | Easy | ✅ |
| 05 | Warm Up (Hex→Dec) | Cryptography | Easy | ✅ |
| 06 | Warm Up 2 (Dec→Binary) | Cryptography | Easy | ✅ |
| 07 | Base64 Decode | Cryptography | Easy | ✅ |
| 08 | Wave a Flag | General Skills | Easy | ✅ |
| 09 | Tab Tab Attack | General Skills | Easy | ✅ |
| 10 | insp3ct0r | Web Exploitation | Easy | ✅ |
| 11 | Strings | General Skills | Easy | ✅ |
| 12 | First Grep | General Skills | Easy | ✅ |
| 13 | Robots | Web Exploitation | Easy | ✅ |
| 14 | Python Wrangling | General Skills | Easy | ✅ |
| 15 | Pw Crack 1 | General Skills | Easy | ✅ |
| 16 | Pw Crack 2 | General Skills | Easy | ✅ |
| 17 | Pw Crack 3 | General Skills | Medium | ✅ |
| 18 | Pw Crack 4 | General Skills | Medium | ✅ |
| 19 | Pw Crack 5 | General Skills | Medium | ✅ |
| 20 | Enhance! | Forensics | Medium | ✅ |
| 21 | Big Zip | General Skills | Easy | ✅ |
| 22 | Vault-door-training | Reverse Engineering | Easy | ✅ |
| 23 | KeyGenPy | Reverse Engineering | Medium | ✅ |
| 24 | Buffer Overflow 0 | Binary Exploitation | Medium | ✅ |

---
---


# ═══════════════════════════════════
# SECTION 1 — SANITY CHECKS
# ═══════════════════════════════════

---


## Challenge 01 — Obedient Cat
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{s4n1ty_v3r1f13d_9b8fa0bc}`

### Goal
Download the file and read it.

### How I Solved It
```bash
cat flag.txt
```

### 💡 Full Explanation

**What is `cat`?**
`cat` stands for "concatenate". It reads a file
and prints its content to the terminal.

```bash
cat filename.txt        # read and print file
cat file1 file2         # print multiple files
cat -n filename.txt     # print with line numbers
```

**What is a CTF flag?**
In CTF (Capture The Flag) competitions, the goal
is to find a secret string called a FLAG.
Flags follow a format like: `picoCTF{something_here}`
Submit the flag to score points.

### Key Takeaway
Always start simple — download, read, check for the flag
before trying complex techniques.

---


## Challenge 02 — Super SSH
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{s3cur3_c0nn3ct10n_3e293eea}`

### Goal
Connect to a remote server using SSH on a custom port.

### How I Solved It
```bash
ssh username@server.picoctf.net -p 55788
```

### 💡 Full Explanation

**What is SSH?**
SSH = Secure Shell. It lets you connect to and
control a remote computer securely over a network.
Everything sent is encrypted — no one can see it.

**SSH Command Structure:**
```bash
ssh username@hostname -p port

# Examples:
ssh bandit0@bandit.labs.overthewire.org -p 2220
ssh ctf-player@server.picoctf.net -p 55788
```

**Key SSH Flags:**

| Flag | Meaning | Example |
|------|---------|---------|
| `-p` | Custom port (default is 22) | `-p 2220` |
| `-i` | Use private key file | `-i key.pem` |
| `-v` | Verbose mode (debug) | `-v` |
| `-L` | Port forwarding | `-L 8080:localhost:80` |

**Default port:** SSH uses port 22 by default.
If the server uses a different port, use `-p`.

### Key Takeaway
SSH is used constantly in pentesting to connect to
remote machines. Master the basic flags early.

---


## Challenge 03 — What's a netcat?
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{nEtCat_Mast3ry_575F8fFd}`

### Goal
Connect to a remote server as a CLIENT using netcat.

### How I Solved It
```bash
nc fickle-tempest.picoctf.net 55703
```


### 💡 Full Explanation

**What is Netcat (nc)?**
Netcat is called the "Swiss Army knife" of networking.
It creates raw TCP/UDP connections between computers.
Used for: testing connections, transferring files,
port scanning, creating listeners, CTF challenges.

**Two Modes:**

```bash
# CLIENT MODE — connect to a server
nc hostname port
nc fickle-tempest.picoctf.net 55703

# SERVER MODE — listen for connections
nc -l -p 1234
nc -lvp 1234      # l=listen, v=verbose, p=port
```


**Common Uses in Security:**

| Use | Command |
|-----|---------|
| Connect to service | `nc host port` |
| Start a listener | `nc -lvp 4444` |
| Send a file | `nc host port < file.txt` |
| Receive a file | `nc -lvp 4444 > file.txt` |
| Banner grabbing | `nc host 80` then type `HEAD / HTTP/1.0` |

**TCP vs UDP:**
- TCP = reliable, ordered (default for nc)
- UDP = faster, no guarantee → use `nc -u host port`

### Key Takeaway
Netcat is used in almost every CTF and real pentest.
Learn both client and server modes — you'll use both.

---
---

# ═══════════════════════════════════
# SECTION 2 — CYBERCHEF (ENCODING)
# ═══════════════════════════════════

---

## Challenge 04 — Mod 26 (ROT13)
**Category:** Cryptography
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{next_time_I'll_try_2_rounds_of_rot13_45559abd}`

### Goal
Decode a ROT13 encoded string.

### How I Solved It
Manually converted ROT13 text back to readable text.
Or used: `echo "text" | tr 'A-Za-z' 'N-ZA-Mn-za-m'`

### 💡 Full Explanation

**What is ROT13?**
ROT13 = Rotate by 13 positions.
It's a special Caesar cipher where shift = 13.
Because alphabet has 26 letters, applying ROT13
TWICE gives back the original text.

```
Original:  A B C D E F G H I J K L M
ROT13:     N O P Q R S T U V W X Y Z

Original:  N O P Q R S T U V W X Y Z
ROT13:     A B C D E F G H I J K L M
```

**Examples:**
```
H → U
E → R
L → Y
L → Y
O → B
"HELLO" → "URYYB"
```

**Decode in Linux:**
```bash
echo "Gur synt vf cvpbPGS{...}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**How tr command works:**
```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
# 'A-Za-z'      = characters to convert FROM
# 'N-ZA-Mn-za-m' = characters to convert INTO
# N-Z = positions 14-26 map back to A-M
# A-M = positions 1-13 map back to N-Z
```

### Key Takeaway
ROT13 encodes and decodes with the SAME operation.
Run it twice → get original. Very common in CTFs.

---

## Challenge 05 — Warm Up (Hex to Decimal)
**Category:** Cryptography
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{61}`

### Goal
Convert hexadecimal `0x3D` to decimal.

### How I Solved It
```
0x3D = (3 × 16) + (13 × 1) = 48 + 13 = 61
```

### 💡 Full Explanation

**Number Systems — Quick Overview:**

| System | Base | Digits Used | Example |
|--------|------|-------------|---------|
| Binary | 2 | 0, 1 | 1010 |
| Decimal | 10 | 0-9 | 42 |
| Hexadecimal | 16 | 0-9, A-F | 0x2A |

**Hex Letter Values:**

| Hex | Decimal |
|-----|---------|
| A | 10 |
| B | 11 |
| C | 12 |
| D | 13 |
| E | 14 |
| F | 15 |

**Position Values (right to left):**

| Position | Power | Value |
|----------|-------|-------|
| 1st (rightmost) | 16⁰ | 1 |
| 2nd | 16¹ | 16 |
| 3rd | 16² | 256 |
| 4th | 16³ | 4096 |

**Formula:**
```
Each digit × its position value, then add all together
```

**Examples:**

```
0x3D
= (3 × 16¹) + (D × 16⁰)
= (3 × 16)  + (13 × 1)
= 48 + 13
= 61 ✅

0x2A
= (2 × 16) + (10 × 1)
= 32 + 10
= 42

0xFF
= (15 × 16) + (15 × 1)
= 240 + 15
= 255
```

**Quick Python conversion:**
```python
int("3D", 16)   # → 61
int("FF", 16)   # → 255
hex(61)         # → '0x3d'
```

**Common Mistakes:**
```
❌ Only multiplying the first digit by 16
✅ Every digit gets its own power of 16

❌ Forgetting A=10, B=11... F=15
✅ Convert letters to numbers first

❌ Forgetting to add all parts together
✅ Sum everything at the end
```

### Key Takeaway
Hex is used EVERYWHERE in security — memory addresses,
color codes, byte values, shellcode, network packets.
Learn to convert it quickly in your head.

---

## Challenge 06 — Warm Up 2 (Decimal to Binary)
**Category:** Cryptography
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{101010}`

### Goal
Convert decimal 42 to binary.

### How I Solved It
Using the power-of-2 method:
```
Powers: 128  64  32  16   8   4   2   1
Value:    0   0   1   0   1   0   1   0
→ 32 + 8 + 2 = 42 → Binary = 101010
```

### 💡 Full Explanation

**What is Binary?**
Binary uses only 2 digits: 0 and 1.
Each position is a power of 2 (right to left).

**Position Values:**
```
Position: 8    7    6    5    4    3    2    1
Power:    2⁷  2⁶   2⁵   2⁴   2³   2²   2¹   2⁰
Value:   128   64   32   16    8    4    2    1
```

**Method — Divide by 2:**
```
42 ÷ 2 = 21 remainder 0
21 ÷ 2 = 10 remainder 1
10 ÷ 2 =  5 remainder 0
 5 ÷ 2 =  2 remainder 1
 2 ÷ 2 =  1 remainder 0
 1 ÷ 2 =  0 remainder 1

Read remainders bottom to top: 101010 ✅
```

**Method — Power of 2 Table:**
```
128 64 32 16  8  4  2  1
  0  0  1  0  1  0  1  0
       ↑     ↑     ↑
      32  +  8  +  2  = 42 ✅
```

**Quick Python conversion:**
```python
bin(42)         # → '0b101010'
bin(42)[2:]     # → '101010' (removes 0b prefix)
int('101010',2) # → 42 (binary back to decimal)
```

**Common Conversions to Remember:**
```
0 = 0000    8 = 1000
1 = 0001    9 = 1001
2 = 0010   10 = 1010
3 = 0011   11 = 1011
4 = 0100   12 = 1100
5 = 0101   13 = 1101
6 = 0110   14 = 1110
7 = 0111   15 = 1111
```

### Key Takeaway
Binary is the language of computers.
Used in file permissions, network masks,
binary exploitation, and CTF forensics.

---

## Challenge 07 — Base64 Decode
**Category:** Cryptography
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{l3arn_th3_r0p35}`

### Goal
Decode the Base64 string `bDNhcm5fdGgzX3IwcDM1`

### How I Solved It
```bash
echo "bDNhcm5fdGgzX3IwcDM1" | base64 -d
```

### 💡 Full Explanation

**What is Base64?**
Base64 converts binary data → readable ASCII text.
It uses 64 characters: A-Z, a-z, 0-9, +, /
Used to safely send binary data through text channels
like email, URLs, HTML pages.

**How to Recognize Base64:**
```
✅ Only contains: A-Z a-z 0-9 + / =
✅ Often ends with = or == (padding)
✅ Length is always multiple of 4
✅ Looks like: SGVsbG8gV29ybGQ=
```

**Decode and Encode:**
```bash
# Decode
echo "SGVsbG8=" | base64 -d          # → Hello
base64 -d encoded.txt                 # → decode file

# Encode
echo "Hello" | base64                 # → SGVsbG8K
base64 file.txt                       # → encode file
```

**Important:** Base64 is NOT encryption.
Anyone can decode it instantly — it's just encoding.
Never use Base64 to hide sensitive data.

**Multiple Encodings:**
Sometimes data is Base64 encoded multiple times.
If output still looks like Base64 → decode again!
Hint: `==` at end = Base64, try decoding again.

### Key Takeaway
Base64 is everywhere — JWT tokens, web images,
email attachments, API responses. Learn to spot
and decode it instantly.

---
---

# ═══════════════════════════════════
# SECTION 3 — GENERAL SKILLS
# ═══════════════════════════════════

---

## Challenge 08 — Wave a Flag
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{b1scu1ts_4nd_gr4vy_ac5832c}`

### Goal
Run a binary file with the help flag to get the flag.

### How I Solved It
```bash
chmod +x warm        # give execute permission first
./warm -h            # run with help flag
```

### 💡 Full Explanation

**What is chmod?**
`chmod` = change mode. It changes file permissions.
Downloaded files often don't have execute permission.
You must add it before running them.

**File Permissions:**
```
-rw-r--r--   (no execute)
-rwxr-xr-x   (has execute ✅)
```

```bash
chmod +x filename     # add execute permission
chmod 755 filename    # same thing using numbers
ls -la filename       # check permissions
```

**Running a binary:**
```bash
./filename            # ./ means "in current directory"
./filename -h         # run with help flag
./filename --help     # long form help flag
```

**Why -h?**
Most programs show their usage info with `-h` or `--help`.
In CTFs, flags are sometimes hidden in help output.
Always try `-h` on any unknown binary.

### Key Takeaway
`chmod +x` before running any downloaded binary.
Always check `-h` / `--help` on unknown programs.

---

## Challenge 09 — Tab, Tab, Attack
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{l3v3l_up!_t4k3_4_r35t!_fc588427}`

### Goal
Navigate a deeply nested directory using Tab autocomplete.

### How I Solved It
```bash
unzip Addadshashanameco.zip
cd Addadshashanameco/
# Press Tab after typing first few letters
# Terminal autocompletes the long directory names
cat <deeply-nested-file>
```

### 💡 Full Explanation

**What is Tab Autocomplete?**
Press Tab in terminal → Linux completes the filename
or directory name for you automatically.

**How it works:**
```bash
cd Add[TAB]          # completes to → cd Addadshashanameco/
cat ver[TAB]         # completes to → cat very_long_filename.txt

# If multiple matches → press Tab twice to show all options
```

**Unzip command:**
```bash
unzip filename.zip           # extract to current directory
unzip filename.zip -d folder # extract to specific folder
unzip -l filename.zip        # list contents without extracting
```

**Navigating directories:**
```bash
ls                   # list files
ls -la               # list all files including hidden
cd foldername        # enter folder
cd ..                # go up one level
cd ~                 # go to home directory
pwd                  # show current directory path
```

### Key Takeaway
Tab autocomplete saves massive time.
Real pentesters use it constantly for long paths,
tool names, and filenames during live tests.

---

## Challenge 10 — insp3ct0r
**Category:** Web Exploitation
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?302945a7}`

### Goal
Find the flag hidden across HTML, CSS, and JS files.

### How I Solved It
Opened browser DevTools → inspected:
1. HTML source (Ctrl+U or F12 → Elements)
2. CSS file (F12 → Sources → style.css)
3. JavaScript file (F12 → Sources → script.js)
Flag was split across all three files.

### 💡 Full Explanation

**Browser DevTools — Your Best Friend:**
```
F12 or Ctrl+Shift+I → Open DevTools

Tabs inside DevTools:
→ Elements   = HTML structure of the page
→ Sources    = All files loaded (HTML, CSS, JS)
→ Network    = All requests made
→ Console    = Run JavaScript, see errors
→ Storage    = Cookies, localStorage, sessionStorage
```

**View Page Source:**
```
Ctrl+U → shows raw HTML source
Right click → View Page Source
```

**What to look for in CTFs:**
```
<!-- HTML comments -->
/* CSS comments */
// JavaScript comments
Hidden input fields: <input type="hidden" value="...">
Data attributes: <div data-secret="...">
Console.log() messages
```

**robots.txt:**
```
Always check: website.com/robots.txt
Lists pages search engines should not index
Often reveals hidden admin pages in CTFs
```

### Key Takeaway
Always inspect all three: HTML, CSS, JS.
Developers often leave comments, flags, or
sensitive data in client-side code.
This is a REAL bug bounty technique too.

---

## Challenge 11 — Strings
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{5tRIng5_1T_d6306c19}`

### Goal
Find the flag hidden inside a binary file.

### How I Solved It
```bash
strings filename | grep picoCTF
```

### 💡 Full Explanation

**What is `strings`?**
Binary files contain machine code that looks like
random characters. But they also contain readable
text like error messages, URLs, passwords, flags.
`strings` extracts all readable text from ANY file.

```bash
strings filename              # print all readable strings
strings filename | grep flag  # search for specific text
strings -n 8 filename         # only strings 8+ chars long
strings -a filename           # scan entire file
```

**Why combine with grep?**
`strings` output can be thousands of lines long.
Pipe to `grep` to find exactly what you need instantly.

```bash
strings binary | grep picoCTF     # find CTF flags
strings binary | grep password    # find passwords
strings binary | grep http        # find URLs
strings binary | grep -i secret   # case insensitive
```

**Pipe operator `|`:**
```
command1 | command2
Output of command1 becomes input of command2

Example:
strings file | grep "pico"
→ strings finds all text
→ grep filters for lines containing "pico"
```

### Key Takeaway
`strings` is used in malware analysis, reverse
engineering, and CTF forensics constantly.
Always try it on unknown binary files.

---

## Challenge 12 — First Grep
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{grep_is_good_to_find_things_e3C4b360}`

### Goal
Find the flag in a large text file using grep.

### How I Solved It
```bash
grep "picoCTF" file.txt
# or
cat file.txt | grep picoCTF
```

### 💡 Full Explanation

**What is grep?**
grep = Global Regular Expression Print.
It searches for a pattern inside files and
prints every line that matches.
One of the most used commands in security.

**Basic Usage:**
```bash
grep "pattern" filename          # search in file
grep "pattern" file1 file2       # search multiple files
cat file | grep "pattern"        # pipe from another command
```

**Essential grep Flags:**

| Flag | Meaning | Example |
|------|---------|---------|
| `-i` | Case insensitive | `grep -i "password"` |
| `-r` | Recursive (all subfolders) | `grep -r "flag" .` |
| `-n` | Show line numbers | `grep -n "error"` |
| `-v` | Invert (show non-matches) | `grep -v "comment"` |
| `-l` | Show filenames only | `grep -l "secret"` |
| `-c` | Count matches | `grep -c "error"` |
| `-A 2` | Show 2 lines AFTER match | `grep -A 2 "flag"` |
| `-B 2` | Show 2 lines BEFORE match | `grep -B 2 "flag"` |
| `--include` | Filter file types | `grep -r --include="*.py"` |

**Real Security Uses:**
```bash
# Find passwords in source code
grep -r "password" /var/www/html/

# Find API keys
grep -r "api_key" .

# Find flags in CTF
grep -r "picoCTF" .

# Search all .txt files recursively
grep -r --include="*.txt" "flag" .

# Find IP addresses
grep -E "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" file.txt
```

### Key Takeaway
grep is essential for every security task.
Master all its flags — you'll use them daily
in bug bounty, CTFs, and real pentesting.

---

## Challenge 13 — Robots
**Category:** Web Exploitation
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{ca1cu1at1ng_Mach1n3s_cc6b1}`

### Goal
Find hidden content by reading robots.txt

### How I Solved It
```
1. Visited: https://target.picoctf.net/robots.txt
2. Found hidden file path: /ccbb2
3. Visited: https://target.picoctf.net/ccbb2
4. Got the flag
```

### 💡 Full Explanation

**What is robots.txt?**
robots.txt is a file at the root of every website.
It tells search engine bots which pages NOT to crawl.
In security, it often reveals hidden admin paths,
backup files, and sensitive directories.

```
# Example robots.txt content:
User-agent: *
Disallow: /admin/
Disallow: /backup/
Disallow: /secret-flag-here/
Disallow: /internal-api/
```

**Why attackers check robots.txt:**
Admins add sensitive paths to robots.txt to hide
them from Google — but this actually REVEALS them
to attackers. Classic security mistake.

**Other hidden files to always check:**
```
/robots.txt          ← hidden paths
/sitemap.xml         ← all pages listed
/.git/               ← source code leak
/.env                ← environment variables, secrets
/backup.zip          ← backup files
/admin               ← admin panels
/config.php          ← configuration files
/phpinfo.php         ← server information
```

**This is a real bug bounty technique:**
Always check robots.txt when testing a target.
Many real vulnerabilities are discovered this way.

### Key Takeaway
robots.txt is one of the first things to check
on any web target. It's a roadmap of hidden pages.

---

## Challenge 14 — Python Wrangling
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{4p0110_1n_7h3_h0us3_9c5f9bcf}`

### Goal
Run a Python script correctly by reading its instructions.

### How I Solved It
```bash
python3 ende.py --help     # read how to use it
python3 ende.py -d flag    # decrypt mode
# paste the token when asked → got the flag
```

### 💡 Full Explanation

**Running Python Scripts:**
```bash
python3 script.py              # run script
python3 script.py --help       # show help
python3 script.py -d file      # pass flags/arguments
```

**Reading Error Messages:**
Python errors tell you EXACTLY what is wrong.
Always read the full error before panicking.

```
Common Python Errors:
FileNotFoundError  → file doesn't exist
PermissionError    → no read/write permission
SyntaxError        → mistake in the code
TypeError          → wrong type passed (string vs int)
ImportError        → missing library
```

**Running scripts with arguments:**
```bash
python3 script.py arg1 arg2    # positional args
python3 script.py -f file.txt  # flag argument
python3 script.py --decrypt    # long flag
```

**Making a Python file executable:**
```bash
chmod +x script.py
./script.py           # run directly
# OR always just use:
python3 script.py     # safer and clearer
```

### Key Takeaway
Read error messages carefully — they guide you
to the exact fix. Python is used heavily in CTF
tooling and security automation.

---

## Challenge 15 — Pw Crack 1
**Category:** General Skills — Python
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{545h_r1ng1ng_fa343060}`

### Goal
Find the hardcoded password in Python source code.

### How I Solved It
Read the source code → found password stored
as plain text in the code → entered it → got flag.

### 💡 Full Explanation

**What is a Hardcoded Password?**
When a developer puts a password directly in the code:
```python
# BAD practice — hardcoded password
password = "secretpass123"
if user_input == password:
    print("Access granted")
```

**This is a REAL vulnerability.**
In bug bounty and pentesting, hardcoded credentials
in source code are a HIGH severity finding.

**Where to find hardcoded secrets:**
```python
password = "..."         # obvious
api_key = "..."          # API keys
secret = "..."           # secrets
token = "..."            # tokens
key = "..."              # encryption keys
```

**How to read Python code fast:**
```
1. Look for variable assignments (=)
2. Look for if conditions (if x == y)
3. Look for print statements (what gets printed?)
4. Trace the flow: input → check → output
```

### Key Takeaway
Hardcoded credentials are one of the most common
real-world vulnerabilities. Source code review
is a core penetration testing skill.

---

## Challenge 16 — Pw Crack 2
**Category:** General Skills — Python
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{tr45h_51ng1ng_489dea9a}`

### Goal
Password is stored as hex values — decode to find it.

### How I Solved It
```
Code contained: 0x65, 0x37
Hex → Decimal: 101, 55
Decimal → ASCII: 'e', '7'
Password = "e7"
```

```python
chr(0x65)   # → 'e'
chr(0x37)   # → '7'
```

### 💡 Full Explanation

**ASCII Table — What is it?**
ASCII maps numbers to characters.
Every character on your keyboard has a number.

```
Important ASCII values:
48-57  → '0'-'9' (digits)
65-90  → 'A'-'Z' (uppercase)
97-122 → 'a'-'z' (lowercase)
32     → ' ' (space)
```

**Conversion Chain:**
```
Hexadecimal → Decimal → ASCII Character
0x65        → 101     → 'e'
0x41        → 65      → 'A'
0x61        → 97      → 'a'
0x30        → 48      → '0'
```

**Python ASCII Functions:**
```python
chr(65)      # → 'A'   (decimal to character)
chr(0x41)    # → 'A'   (hex to character)
ord('A')     # → 65    (character to decimal)
ord('a')     # → 97

# Common mistake:
char(65)     # ❌ NameError — wrong function name
chr(65)      # ✅ correct
```

**ASCII ≠ regular decimal:**
Regular decimal 65 ≠ ASCII 65 meaning.
In ASCII, 65 = letter 'A', not the number sixty-five.

### Key Takeaway
Understanding hex → decimal → ASCII conversion
is essential for reverse engineering, forensics,
and analyzing protocols and file formats.

---

## Challenge 17 — Pw Crack 3
**Category:** General Skills — Python
**Difficulty:** Medium
**Status:** ✅ Solved
**Flag:** `picoCTF{m45h_fl1ng1ng_2b072a90}`

### Goal
Password is one from a list — use a loop to try all.

### How I Solved It
```python
# The code had a list of possible passwords
# I used a for loop to try each one:
for password in possible_passwords:
    if check_password(password):
        print("Found:", password)
        break
```

### 💡 Full Explanation

**What is a Dictionary Attack?**
Instead of trying random combinations (brute force),
a dictionary attack tries passwords from a known list.
Much faster because real users pick common passwords.

**Python For Loop — Quick Review:**
```python
# Loop through a list
for item in my_list:
    print(item)

# Loop with index
for i, item in enumerate(my_list):
    print(i, item)

# Range loop
for i in range(10):   # 0 to 9
    print(i)
```

**Real Tools That Do This:**
```bash
# Password cracking tools use same concept:
hydra -l admin -P wordlist.txt ssh://target
john --wordlist=rockyou.txt hash.txt
hashcat -m 0 hash.txt rockyou.txt
```

**Popular Wordlists:**
```
rockyou.txt      → 14 million real passwords
SecLists         → comprehensive security wordlists
```

### Key Takeaway
Dictionary attacks are used in real pentesting.
Understanding the code behind them helps you
use and adapt real tools better.

---

## Challenge 18 — Pw Crack 4
**Category:** General Skills — Python
**Difficulty:** Medium
**Status:** ✅ Solved
**Flag:** `picoCTF{fl45h_5pr1ng1ng_ae0fb77c}`

### Goal
Write code to automatically try all passwords
and verify using a function.

### How I Solved It
```python
# Added for loop + function call + condition:
for password in password_list:
    if check_password(password):  # call verify function
        print("Password:", password)
        break
```

### 💡 Full Explanation

**Reading Existing Python Code:**
When you see unfamiliar code, ask:
```
1. What does this function take as input?
2. What does it return? (True/False? string? number?)
3. What condition triggers success?
```

**Function Call Pattern:**
```python
def check_password(pw):
    # does some verification
    return True or False

# Call it like:
result = check_password("myguess")
if result:
    print("Correct!")
```

**If Statement with Function:**
```python
if check_password(password):    # True → runs block
    print("Found it!")

if check_password(password) == True:  # same thing
    print("Found it!")
```

**Break Statement:**
```python
for pw in list:
    if check(pw):
        print(pw)
        break    # ← stops the loop immediately after finding
```

### Key Takeaway
Being able to read and modify existing code
is more valuable than writing from scratch.
This skill is used in CTFs and real security work.

---

## Challenge 19 — Pw Crack 5
**Category:** General Skills — Python
**Difficulty:** Medium
**Status:** ✅ Solved
**Flag:** `picoCTF{h45h_sl1ng1ng_40f26f81}`

### Goal
Passwords come from a file — read each line and try it.

### How I Solved It
```python
with open('wordlist.txt', 'r') as f:
    for line in f:
        password = line.strip()    # remove \n at end
        if check_password(password):
            print("Password:", password)
            break
```

### 💡 Full Explanation

**Reading Files in Python:**
```python
# Method 1 — with statement (recommended)
with open('file.txt', 'r') as f:
    content = f.read()         # read entire file
    lines = f.readlines()      # read as list of lines

# Method 2 — line by line (memory efficient)
with open('file.txt', 'r') as f:
    for line in f:
        print(line)
```

**strip() — Why It Matters:**
```python
line = "password123\n"   # file lines end with \n
line.strip()             # → "password123" (removes \n)

# Without strip:
"password123\n" == "password123"   # ❌ False — won't match!
# With strip:
"password123" == "password123"     # ✅ True — matches!
```

**File Modes:**
```python
open('file', 'r')   # read only
open('file', 'w')   # write (overwrites)
open('file', 'a')   # append
open('file', 'rb')  # read binary
```

**MD5 Hashing Preview:**
This level introduced checking hashed passwords.
```python
import hashlib
hashlib.md5(password.encode()).hexdigest()
# converts password → MD5 hash for comparison
```

### Key Takeaway
Real password cracking tools read from wordlists
and hash each guess — exactly what this code does.
This is the foundation of tools like hashcat and john.

---

## Challenge 20 — Enhance!
**Category:** Forensics
**Difficulty:** Medium
**Status:** ✅ Solved
**Flag:** `picoCTF{3nh4nc3d_24374675}`

### Goal
Read an SVG/XML image file to find the flag
hidden as text elements inside it.

### How I Solved It
```bash
cat image.svg | grep -i "pico"
# or open in text editor — SVG is XML/text format
# found flag letters hidden as individual <text> elements
```

### 💡 Full Explanation

**What is SVG?**
SVG = Scalable Vector Graphics.
Unlike JPG/PNG (pixel images), SVG is actually
TEXT (XML format). You can open and read it like code.

```xml
<!-- SVG file looks like this inside: -->
<svg xmlns="http://www.w3.org/2000/svg">
  <text x="10" y="20">p</text>
  <text x="20" y="20">i</text>
  <text x="30" y="20">c</text>
  <!-- flag hidden letter by letter! -->
</svg>
```

**Forensics Mindset:**
Not all files are what they seem.
Always check the actual file content, not just
how it looks when opened normally.

**Useful File Analysis Commands:**
```bash
file image.svg         # identify file type
cat image.svg          # read if text-based
strings image.png      # extract text from binary image
xxd image.png | head   # view hex dump
exiftool image.jpg     # read metadata
binwalk image.png      # find hidden files inside
```

**File Type Tricks in CTF:**
```bash
file suspicious.jpg    # might say "ASCII text"!
# rename and open accordingly
mv suspicious.jpg suspicious.txt
cat suspicious.txt
```

### Key Takeaway
Files are not always what their extension says.
Always check actual content with `file` command
and `cat` for text-based formats like SVG, XML, HTML.

---

## Challenge 21 — Big Zip
**Category:** General Skills
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{gr3p_15_m4g1c_ef8790dc}`

### Goal
Find the flag hidden in one file among hundreds.

### How I Solved It
```bash
unzip archive.zip
grep -r --include="*.txt" "picoCTF" .
```

### 💡 Full Explanation

**Recursive grep — The Power Move:**
```bash
grep -r "pattern" .
# -r = recursive (search all subdirectories)
# "pattern" = what to search for
# . = start from current directory

# With file type filter:
grep -r --include="*.txt" "picoCTF" .
grep -r --include="*.py" "password" .
grep -r --include="*.config" "secret" .
```

**Why this is a real security skill:**
In real bug bounty and pentesting, you often get
access to large codebases or file systems.
You need to find secrets, passwords, and keys
buried in hundreds of files fast.

```bash
# Real pentest use cases:
grep -r "password" /var/www/html/         # web app passwords
grep -r "api_key" . --include="*.js"      # JavaScript API keys
grep -r "SECRET" . --include="*.env"      # environment files
grep -r "BEGIN RSA" .                      # private keys
grep -r "AKIA" .                           # AWS access keys
```

**grep with multiple patterns:**
```bash
grep -E "password|secret|key|token" file.txt
# -E enables regex
# | means OR
```

### Key Takeaway
`grep -r` is one of the most powerful commands
for finding sensitive data in large file systems.
Master it for real bug bounty work.

---
---

# ═══════════════════════════════════
# SECTION 4 — REVERSE ENGINEERING
# ═══════════════════════════════════

---

## Challenge 22 — Vault-door-training
**Category:** Reverse Engineering
**Difficulty:** Easy
**Status:** ✅ Solved
**Flag:** `picoCTF{w4rm1ng_Up_w1tH_jAv4_000AXPNPN0i}`

### Goal
Read Java source code and find the hardcoded password.

### How I Solved It
Opened the `.java` file → read the code →
found the password hardcoded in a String variable.

### 💡 Full Explanation

**What is Reverse Engineering?**
RE = understanding how a program works by
reading its code or analyzing its behavior
WITHOUT having the original documentation.

**Reading Java Code (Basics):**
```java
// Java looks like this:
public class VaultDoor {

    // Method (function)
    public boolean checkPassword(String password) {
        return password.equals("w4rm1ng_Up_w1tH_jAv4...");
                                 ↑
                        flag is right here!
    }
}
```

**What to look for in RE challenges:**
```
String comparisons   → password.equals("...")
if conditions        → if (input == expected)
Hardcoded values     → String key = "secretvalue"
Return statements    → return true if correct
```

**Java vs Python — Key Differences:**
```
Java:    String s = "hello";      (type required)
Python:  s = "hello"              (no type needed)

Java:    if (a.equals(b))         (for strings)
Python:  if a == b                (for strings)

Java:    System.out.println(x);   (print)
Python:  print(x)                 (print)
```

### Key Takeaway
RE is about reading code you didn't write and
understanding its logic. Start with Java/Python
source code before moving to binary analysis.

---

## Challenge 23 — KeyGenPy
**Category:** Reverse Engineering
**Difficulty:** Medium
**Status:** ✅ Solved
**Flag:** `picoCTF{1n_7h3_kk3y_of_08c46aa4}`

### Goal
Understand the license key generation algorithm
and create a valid key.

### How I Solved It
Read the Python code → understood the key format
and validation rules → wrote new Python code
to generate a valid license key → submitted it.

### 💡 Full Explanation

**What is a Key Generator?**
Programs sometimes check if you have a valid
license key. In RE challenges, you:
1. Read how the key is validated
2. Reverse the logic
3. Generate a key that passes validation

**Approach:**
```python
# Original code validates a key like:
def validate(key):
    if len(key) != 16:
        return False
    if key[0:4] != "PICO":
        return False
    # ... more checks
    return True

# Your job: create a key that passes all checks
# Write a generator:
key = "PICO" + generate_rest()
```

**Steps for any RE challenge:**
```
1. Read the code top to bottom
2. Find the validation/check function
3. Understand each condition
4. Work backwards to create valid input
5. Test your solution
```

### Key Takeaway
KeyGen challenges teach you to reverse-engineer
validation logic — same skill used to bypass
license checks, authentication, and access controls.

---
---

# ═══════════════════════════════════
# SECTION 5 — BINARY EXPLOITATION
# ═══════════════════════════════════

---

## Challenge 24 — Buffer Overflow 0
**Category:** Binary Exploitation
**Difficulty:** Medium
**Status:** ✅ Solved
**Flag:** `picoCTF{1n_7h3_kk3y_of_08c46aa4}`

### Goal
Exploit a buffer overflow vulnerability to get the flag.

### How I Solved It
```bash
# Sent more input than the buffer could hold
# Sent input longer than 16 characters repeatedly
python3 -c "print('A' * 100)" | nc server port
# or just type a very long string when prompted
```

### 💡 Full Explanation

**What is a Buffer Overflow?**
A buffer is a fixed-size memory space for storing data.
If you put MORE data than the buffer can hold,
the extra data OVERFLOWS into adjacent memory.
This can crash programs or give attackers control.

```
Normal input (8 bytes):  [H][E][L][L][O][_][_][_]
Overflow input (16+ bytes): [A][A][A][A][A][A][A][A][A][A]→→→CRASH/FLAG
                                                              ↑
                                              overflows into next memory area
```

**Why Does This Give the Flag?**
In this challenge, the overflow triggered a signal
handler (SIGSEGV) which printed the flag.
The program was designed to reveal the flag
when the buffer overflows.

**Real Impact of Buffer Overflows:**
```
Level 0: Crash the program (Denial of Service)
Level 1: Overwrite return address → jump to any code
Level 2: Inject shellcode → arbitrary code execution
Level 3: Full system compromise (root/admin)
```

**Classic Buffer Overflow Pattern:**
```c
// Vulnerable C code:
char buffer[16];           // only 16 bytes allocated
gets(buffer);              // reads unlimited input ← DANGEROUS

// Safe version:
fgets(buffer, 16, stdin);  // limits input to 16 bytes
```

**Testing for Buffer Overflow:**
```bash
# Try increasingly long inputs:
python3 -c "print('A' * 50)"  | ./program
python3 -c "print('A' * 100)" | ./program
python3 -c "print('A' * 200)" | ./program

# Or use pattern to find exact offset:
# (Advanced — covered in later challenges)
```

### Key Takeaway
Buffer overflows are one of the oldest and most
important vulnerability classes. Understanding
the basic concept is required for any
binary exploitation or Red Team role.

---
---

# ═══════════════════════════════════════════════
# 🔑 MASTER COMMAND REFERENCE
# ═══════════════════════════════════════════════

## Linux Commands

```bash
# File Operations
cat file.txt                    # read file
strings binary                  # extract text from binary
file unknown                    # identify file type
xxd file | head                 # view hex dump
chmod +x file                   # add execute permission

# Searching
grep "pattern" file             # search in file
grep -r "pattern" .             # search recursively
grep -i "pattern" file          # case insensitive
grep -n "pattern" file          # show line numbers
grep -r --include="*.txt" "flag" .   # filter file type

# Navigation
ls -la                          # list all files
cd folder                       # change directory
pwd                             # print current path
find . -name "*.txt"            # find files by name
find . -type f -size +1k        # find by size

# Archives
unzip file.zip                  # extract zip
tar -xf file.tar                # extract tar
tar -xzf file.tar.gz            # extract tar.gz
gzip -d file.gz                 # decompress gzip
```

## Network Commands

```bash
# SSH
ssh user@host -p port           # connect to server
ssh user@host -i key.pem        # connect with key
ssh user@host command           # run command without login

# Netcat
nc host port                    # connect to server (client)
nc -lvp 4444                    # listen on port (server)
nc -lvp 4444 > file.txt         # receive file

# Other
curl http://target.com          # make HTTP request
wget http://target.com/file     # download file
```

## Python Quick Reference

```python
# Encoding conversions
chr(65)                         # → 'A' (decimal to char)
ord('A')                        # → 65 (char to decimal)
int("3D", 16)                   # → 61 (hex to decimal)
hex(61)                         # → '0x3d' (decimal to hex)
bin(42)                         # → '0b101010' (to binary)
int('101010', 2)                # → 42 (binary to decimal)

# Base64
import base64
base64.b64decode("SGVsbG8=")    # → b'Hello'
base64.b64encode(b"Hello")      # → b'SGVsbG8='

# Hashing
import hashlib
hashlib.md5(b"password").hexdigest()     # → MD5 hash
hashlib.sha256(b"password").hexdigest()  # → SHA256 hash

# File reading
with open('file.txt') as f:
    for line in f:
        print(line.strip())
```

## Encoding Cheatsheet

```
Recognize Base64:  ends with = or ==, uses A-Za-z0-9+/
Recognize Hex:     only 0-9 and A-F
Recognize Binary:  only 0s and 1s
Recognize ROT13:   readable but wrong letters
Recognize URL enc: %XX format (%20 = space)

Decode Base64:     echo "text" | base64 -d
Decode ROT13:      echo "text" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
Decode Hex:        echo "68656c6c6f" | xxd -r -p
Decode URL:        python3 -c "import urllib.parse; print(urllib.parse.unquote('text'))"
```

---

*Notes by Darshan | PicoCTF picoGym Beginner Guide*
*Platform: picoctf.org*