# Lab 03 — Reflected XSS into HTML Context with Most Tags and Attributes Blocked

**Level:** Practitioner
**Type:** Reflected XSS + WAF Bypass
**Status:** ✅ Solved
**Platform:** PortSwigger Web Academy

---

## 🎯 Objective
Bypass a WAF blocking most XSS tags and attributes,
and call `print()` with no user interaction required.

---

## 🧠 Concept
A Web Application Firewall (WAF) blocks common XSS payloads
like `<script>`, common event handlers like `onclick`, `onmouseover`.
The goal is to find tags and attributes the WAF **missed**
and craft a payload that fires automatically without user interaction.

---

## 🪜 Steps to Reproduce

### Step 1 — Confirm Basic XSS is Blocked
<script>print()</script>
→ 400 Bad Request — WAF blocks it

### Step 2 — Fuzz for Allowed Tags Using Burp Intruder
1. Send search request to **Burp Intruder**
2. Set payload position: /search?q=<§tag§>

3. Use PortSwigger XSS cheatsheet tag list as payload
4. Look for responses with **200 status** (not blocked)
5. Allowed tag found:<body>
````
Step 3 — Fuzz for Allowed Events/Attributes

1.Set payload position on event attribute:
/search?q=<body+§event§=print()>

2.Use PortSwigger XSS cheatsheet events list as payload
3.Look for 200 status responses
4.Allowed event found:    onresize

Step 4 — Build the Payload
<body onresize=print()>
But this needs the body to actually resize — needs user interaction.
Step 5 — Deliver via iframe (No User Interaction)
Wrap it in an iframe that auto-resizes on load:
<iframe 
  src="https://YOUR-LAB-ID.web-security-academy.net/?search=<body onresize=print()>" 
  onload="this.style.width='100px'">
</iframe>

iframe loads the vulnerable page with the payload
onload resizes the iframe width
This triggers onresize on the <body> inside
print() fires automatically ✅

Step 6 — Deliver the Exploit

Go to Exploit Server
Paste the iframe payload in the body
Click Deliver to Victim
Lab solved ✅

💉 Final Payload
<iframe 
  src="https://YOUR-LAB-ID.web-security-academy.net/?search=<body onresize=print()>" 
  onload="this.style.width='100px'">
</iframe>
Updating Github readme with completed projectsYou said: We are updating my Github reame what i have compeletetd just only that part okay this is my readme file# 🔴 Red Team Journey

> *"I don't wait for opportunities. I build the skills to create them."*

![Status](https://img.shields.io/badge/Status-Active%20Learning-red?style=for-the-badge)
![Focus](https://img.shields.io/badge/Focus-Offensive%20Security-black?style=for-the-badge)
![Goal](https://pastedWe are updating my Github reame what i have compeletetd just only that part okay this is my readme file6 AprClaude responded: What have you completed that you'd like to add or update in the "✅ What I Have Completed" section?What have you completed that you'd like to add or update in the "✅ What I Have Completed" section?You said: Linux basicsLinux basics
Networking + HTTP basics
Burp Suite — Intercept + Repeater
PortSwigger — Access Control, IDOR, Path Traversal, Information Disclosure and Playing Bandit level6 AprClaude responded: Here's the updated ✅ What I Have Completed section:Here's the updated ✅ What I Have Completed section:
markdown## ✅ What I Have Completed

| Topic | Platform | Status |
|-------|----------|--------|
| Linux Basics | LabEx / Linux Journey | ✅ Done |
| Networking + HTTP Basics | Self Study | ✅ Done |
| Burp Suite — Intercept & Repeater | Self Practice | ✅ Done |
| Access Control Vulnerabilities | PortSwigger Web Academy | ✅ Done |
| IDOR (Insecure Direct Object Reference) | PortSwigger + Bug Bounty Research | ✅ Done |
| Path Traversal | PortSwigger Web Academy | ✅ Done |
| Information Disclosure | PortSwigger Web Academy | ✅ Done |
| Playing Bandit Levels | OverTheWire | ✅ Done |
| GitHub Portfolio Setup | GitHub | ✅ Done |
Just replace your existing completed section with this. Let me know if you want to tweak any platform names or add anything else!You said: NOw I'm solving my previous so we can upload on Github notes what is the file structure i have to make it ?NOw I'm solving my previous so we can upload on Github notes what is the file structure i have to make it ?7 AprClaude responded: Based on your README, here's the file structure you should follow:Based on your README, here's the file structure you should follow:
red-team-journey/
│
├── web-vulnerabilities/
│   ├── access-control.md
│   ├── idor-techniques.md
│   ├── path-traversal.md
│   ├── information-disclosure.md
│   └── burp-cheatsheet.md
│
├── tools/
│   └── burp-suite-basics.md        ← Intercept & Repeater notes
│
├── networking/
│   └── http-basics.md
│
├── linux/
│   ├── linux-basics.md
│   └── bandit-writeups.md          ← OverTheWire Bandit levels
│
├── ctf-writeups/
│   ├── picoctf/
│   └── hackthebox/
│
├── bug-bounty-notes/
│   ├── methodology.md
│   └── idor-checklist.md
│
└── README.md
For each .md file, keep this simple structure inside:
markdown# Topic Name

## What is it?
## How it works
## Lab / Practice Notes
## Key Payloads / Commands
## Resources
Start with what you've already completed — linux-basics.md, bandit-writeups.md, http-basics.md, access-control.md, idor-techniques.md, path-traversal.md, information-disclosure.md, and burp-cheatsheet.md. Want me to help you write any of these notes?You said: What should i add about that Web - Security and what bug bounty structureWhat should i add about that Web - Security and what bug bounty structure7 AprClaude responded: Here's a complete breakdown for both:Here's a complete breakdown for both:

🌐 Web Vulnerabilities — What to Add in Each File
access-control.md
- What is Access Control?
- Types (Vertical, Horizontal, Context-dependent)
- Common misconfigurations
- PortSwigger Lab notes (what you did, what you found)
- Bypass techniques
- Real-world examples
- Burp Suite steps used
idor-techniques.md
- What is IDOR?
- Where to look (API endpoints, cookies, headers, body params)
- Testing methodology (step by step)
- PortSwigger Lab notes
- IDOR in JSON / encoded values (base64, hashed IDs)
- Checklist used
path-traversal.md
- What is Path Traversal?
- Basic payloads  ../../../etc/passwd
- Bypass techniques (URL encoding, double encoding)
- PortSwigger Lab notes
- Where to test (file upload, file download params)
information-disclosure.md
- What is Information Disclosure?
- Common sources (error messages, robots.txt, .git, debug pages)
- Tools used (Burp, Wayback Machine, Google Dorks)
- PortSwigger Lab notes
- What sensitive data to look for
burp-cheatsheet.md
- Intercept — how to set up, what to look for
- Repeater — when and how to use it
- Intruder — attack types (Sniper, Cluster Bomb)
- Useful shortcuts
- Common workflow for testing a target

🐛 Bug Bounty File Structure
bug-bounty-notes/
│
├── methodology.md          ← Your full recon to report process
├── recon-checklist.md      ← Subdomain, endpoint, param discovery
├── idor-checklist.md       ← Specific IDOR hunting steps
├── targets-tested.md       ← Programs you tested (no sensitive info)
├── report-template.md      ← How to write a good bug report
└── resources.md            ← Tools, wordlists, references
What goes in each:
methodology.md
1. Recon       → subdomain enum, JS files, endpoints
2. Mapping     → understand app flow, features, roles
3. Testing     → apply vuln techniques per feature
4. Confirm     → reproduce the bug clearly
5. Report      → write clean, professional report
recon-checklist.md
- Subdomains (subfinder, amass)
- Endpoints (waybackurls, gau)
- JS files (LinkFinder)
- Parameters (Arjun)
- Google Dorks for target
report-template.md
Title:
Severity: (Critical / High / Medium / Low)
Target URL:
Steps to Reproduce:
  1.
  2.
  3.
Impact:
Proof of Concept: (screenshot / video)
Suggested Fix:
targets-tested.md
| Program | Scope | Date | Finding | Status |
|---------|-------|------|---------|--------|
| Example | *.example.com | Apr 2026 | IDOR | Submitted |

Start filling these one by one as you solve labs. Want me to write a full template for any specific file?You said: Lab: Information disclosure in error messagesLab: Information disclosure in error messages
APPRENTICE
LABSolved
* 
* 
* 
* 
* 
* 
This lab's verbose error messages reveal that it is using a vulnerable version of a third-party framework. To solve the lab, obtain and submit the version number of this framework.7 AprClaude responded: Great!Great! Here's the full note for this lab:

markdown# Information Disclosure — Lab Notes

## Lab 1: Information Disclosure in Error Messages
**Level:** Apprentice
**Platform:** PortSwigger Web Academy
**Status:** ✅ Solved

---

## 🎯 Objective
Find the version number of a vulnerable third-party framework
revealed through verbose error messages.

---

## 🧠 Concept
When applications show detailed error messages to users,
they can accidentally leak sensitive technical information like:
- Framework name and version
- Stack traces
- File paths
- Database queries

---

## 🪜 Steps to Reproduce

1. Open the lab and browse the shop
2. Click on any product
3. In the URL, modify the `productId` parameter to an 
   unexpected value (like a string instead of a number)
/product?productId=abc
4. The app throws a verbose error message
5. Read the error — it reveals the framework and version number
6. Submit the version number to solve the lab

---

## 🔍 What to Look For in Error Messages

| Info Leaked | Example |
|-------------|---------|
| Framework version | Apache Struts 2.3.31 |
| File paths | /var/www/html/app.py |
| Stack trace | line 42 in views.py |
| Database errors | SQL syntax error near... |

---

## 🛠️ Tools Used
- Burp Suite — Intercept / Repeater
- Browser URL manipulation

---

## 💡 Key Takeaway
Always test unexpected inputs in parameters.
Errors = information. Information = attack surface.

---

## 🔗 Reference
- https://portswigger.net/web-security/information-disclosure

Save this as information-disclosure.md inside your web-vulnerabilities/ folder. Want me to write the next lab note too?You said: Now I'm learning XSS just start learning I will share some theory from portswiger so will uplod that I create seprate folder under the Web - vulnerabilites tha…Now I'm learning XSS just start learning I will share some theory from portswiger so will uplod that I create seprate folder under the Web - vulnerabilites that is xss13 AprClaude responded: Perfect plan!Perfect plan! Here's the folder structure to create:
web-vulnerabilities/
│
├── xss/
│   ├── README.md          ← XSS overview + your progress
│   ├── theory.md          ← PortSwigger theory notes (you'll share here)
│   ├── labs/
│   │   ├── lab-01.md
│   │   ├── lab-02.md
│   │   └── ...
│   └── cheatsheet.md      ← Payloads, bypass tricks (builds over time)
│
├── access-control.md
├── idor-techniques.md
├── path-traversal.md
├── information-disclosure.md
└── burp-cheatsheet.md

Go ahead and share the PortSwigger XSS theory — I'll format it cleanly into theory.md for you.
As you solve labs one by one, we'll add each as lab-01.md, lab-02.md and so on. By the end you'll have a complete XSS folder ready to push to GitHub. 🔥You said: --> There have few question in the Docoument try to add few more if you like it or find good answer we be short like current same answers and create good notes…Cross-site scripting
In this section, we'll explain what cross-site scripting is, describe the different varieties of cross-site scripting vulnerabilities, and spell out how to find and prevent cross-site scripting.

What is cross-site scripting (XSS)?
Cross-site scripting (also known as XSS) ispasted--> There have few question in the Docoument try to add few more if you like it or find good answer we be short like current same answers and create good notes after this i will be learn main three types of xss by following portswiger pattern13 AprClaude responded: Here's your theory.Here's your theory.md file for XSS:
markdown# Cross-Site Scripting (XSS) — Theory Notes

**Source:** PortSwigger Web Academy
**Status:** 🔄 In Progress

---

## 🔍 What is XSS?
A web security vulnerability that allows an attacker to inject
malicious scripts into web pages viewed by other users.
It bypasses the **Same Origin Policy** — which is designed to
keep different websites isolated from each other.

---

## ⚙️ How Does XSS Work?
The attacker manipulates a vulnerable site to return malicious
JavaScript to users. When it executes in the victim's browser,
the attacker can fully compromise their session.

---

## 🧩 Types of XSS

| Type | Where the payload lives | Execution |
|------|------------------------|-----------|
| Reflected | Current HTTP request | Immediate |
| Stored | Website's database | Persistent |
| DOM-based | Client-side JavaScript | In the browser |

### 1️⃣ Reflected XSS
- Malicious script comes from the **current HTTP request**
- Simplest type of XSS
- Not stored anywhere — works via crafted URL
- Example:
https://insecure-website.com/status?message=<script>alert(1)</script>

### 2️⃣ Stored XSS (Persistent)
- Malicious script is **saved in the database**
- Affects every user who loads that page
- Common in: blog comments, chat messages, user profiles
- More dangerous than Reflected XSS

### 3️⃣ DOM-based XSS
- Vulnerability lives in **client-side JavaScript**
- Data from untrusted source written directly to the DOM
- No server interaction needed
- Harder to detect than Reflected/Stored
- Example sink: `innerHTML`, `document.write`, `setTimeout`

---

## 🎯 What Can XSS Be Used For?

- Steal session cookies → Account takeover
- Capture login credentials (keylogging)
- Perform actions on behalf of the victim
- Deface the website visually
- Redirect users to malicious sites
- Inject trojan/backdoor functionality

---

## 💥 Impact — Depends On:

| Scenario | Impact |
|----------|--------|
| Public app, no sensitive data | Low |
| Banking / Healthcare / Email app | High |
| Admin/privileged user compromised | Critical |

---

## 🔎 How to Find XSS

### Manual Testing
- Inject unique string (e.g. `xss1234`) in every input field
- Check where it appears in the HTTP response
- Try payloads based on the context (HTML, JS, attribute)

### Reflected & Stored
- Test every entry point — URL params, forms, headers, cookies
- Look where input is reflected back in response

### DOM-based
- Use browser DevTools to search DOM for your input
- Review JavaScript for dangerous sinks:
  - `innerHTML`
  - `document.write()`
  - `eval()`
  - `setTimeout()`
  - `location.href`

### With Burp Suite
- Use the web vulnerability scanner
- Combines static + dynamic JS analysis
- Reliable for DOM-based detection

---

## 🛡️ How to Prevent XSS

| Method | Description |
|--------|-------------|
| Filter input on arrival | Allow only expected/valid characters |
| Encode output | HTML/JS/URL encode before rendering |
| Use correct headers | `Content-Type`, `X-Content-Type-Options` |
| Content Security Policy (CSP) | Last line of defense, limits script execution |

---

## ✅ XSS Proof of Concept (PoC) Payloads

```javascript
// Standard PoC
<script>alert(1)</script>

// Chrome (v92+) — use print() instead of alert()
<script>print()</script>

// Image tag with onerror
<img src=1 onerror=alert(1)>

// Without parentheses
<img src=x onerror=alert`1`>
```

> ⚠️ Chrome v92+ blocks `alert()` in cross-origin iframes.
> Use `print()` for PortSwigger labs when needed.

---

## ❓ Common Questions

**Q: How common is XSS?**
Very common — likely the most frequently occurring web vulnerability.

**Q: XSS vs CSRF?**
XSS = inject malicious script into the site.
CSRF = trick victim into performing unintended actions.

**Q: XSS vs SQL Injection?**
XSS = client-side, targets users.
SQLi = server-side, targets the database.

**Q: How to prevent in PHP?**
Use `htmlentities()` with `ENT_QUOTES` for HTML context.
Use JavaScript Unicode escapes for JS context.

**Q: How to prevent in Java?**
Use Google Guava library to HTML-encode output.
Whitelist allowed characters on input.

**Q: Can XSS bypass Same Origin Policy?**
Yes — that is the core reason XSS is dangerous.
It tricks the browser into thinking malicious script
is from a trusted origin.

**Q: What is a sink in DOM XSS?**
A sink is a JavaScript function/property that can
execute or render data — like `innerHTML`, `eval()`,
`document.write()`. If attacker-controlled data
reaches a sink, it's exploitable.

**Q: What is CSP and does it fully stop XSS?**
CSP (Content Security Policy) is a browser-level defense
that restricts which scripts can run. It reduces impact
but can often be bypassed — not a complete fix alone.

---

## 🔗 Resources
- https://portswigger.net/web-security/cross-site-scripting
- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
- https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting

Save this as web-vulnerabilities/xss/theory.md.
Whenever you're ready, share the Reflected XSS theory from PortSwigger and we'll build reflected-xss.md next! 🔥You said: lab-1 I solve by my self it is easy to solvelab-1 I solve by my self it is easy to solve
Lab: Reflected XSS into HTML context with nothing encoded
APPRENTICE
LABNot solved
This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.
To solve the lab, perform a cross-site scripting attack that calls the alert function.Show more14 AprClaude responded: Here's your lab-01.Here's your lab-01.md note:
markdown# Lab 01 — Reflected XSS into HTML Context with Nothing Encoded

**Level:** Apprentice
**Type:** Reflected XSS
**Status:** ✅ Solved (Self)
**Platform:** PortSwigger Web Academy

---

## 🎯 Objective
Perform a reflected XSS attack that calls the `alert()` function
using the search functionality.

---

## 🧠 Concept
The search input is reflected directly back into the HTML response
with **no encoding or sanitization** — whatever you type appears
raw in the page source. This makes it the most basic form of
Reflected XSS.

---

## 🪜 Steps to Reproduce

1. Open the lab — it has a search bar
2. Type a test string to confirm reflection:
test123
3. Right-click → View Page Source
4. Confirm your input appears raw in the HTML
5. Now inject the XSS payload in the search bar:
```javascript
   <script>alert(1)</script>
```
6. Press Enter / Submit
7. `alert(1)` fires → Lab solved ✅

---

## 💉 Payload Used
```javascript
<script>alert(1)</script>
```

---

## 🔍 Why It Works
- Input is reflected directly into HTML with zero filtering
- Browser sees `<script>` tag as valid JavaScript
- Executes immediately in the victim's browser context

---

## 💡 Key Takeaway
Always test search bars and any input that reflects
back to the page. No encoding = instant XSS.
This is the most basic reflected XSS — real targets
will have filters but the concept stays the same.

---

## 🛠️ Tools Used
- Browser only (no Burp needed for this one)

---

## 🔗 Reference
- https://portswigger.net/web-security/cross-site-scripting/reflected

Save this as web-vulnerabilities/xss/labs/lab-01.md 🔥
Whenever you solve the next lab share it and we'll document it! Are you moving to lab-02 Stored XSS next or continuing Reflected XSS labs?You said: this I learn refelected XSS now so make concise notes and add More Question in question sectionsReflected XSS
In this section, we'll explain reflected cross-site scripting, describe the impact of reflected XSS attacks, and spell out how to find reflected XSS vulnerabilities.

What is reflected cross-site scripting?
Reflected cross-site scripting (or XSS) arises when an application receivespastedthis I learn refelected XSS now so make concise notes and add More Question in question sections14 AprClaude responded: Here's your reflected-xss.Here's your reflected-xss.md file:
markdown# Reflected XSS — Theory Notes

**Source:** PortSwigger Web Academy
**Status:** ✅ Completed

---

## 🔍 What is Reflected XSS?
When an application takes data from an HTTP request and includes
it directly in the immediate response **without safe processing**.
The malicious script is not stored — it lives in the crafted URL.

**Simple Example:**
Normal request
https://insecure-website.com/search?term=gift
→ <p>You searched for: gift</p>
Attacker's crafted URL
https://insecure-website.com/search?term=<script>alert(1)</script>
→ <p>You searched for: <script>alert(1)</script></p>

---

## 💥 Impact — What Attacker Can Do

| Action | Description |
|--------|-------------|
| Perform actions | Do anything the victim user can do |
| Steal data | View any info the victim can access |
| Modify data | Change any info the victim can modify |
| Pivot attacks | Launch attacks that appear from victim |

---

## 📦 How Attack is Delivered
Since reflected XSS needs the victim to visit a crafted URL,
attackers deliver it via:
- Malicious links on attacker-controlled websites
- Phishing emails / tweets / messages
- Links planted on third-party sites
- Targeted against specific users or indiscriminate

> ⚠️ Because it needs an **external delivery mechanism**,
> Reflected XSS is generally **less severe than Stored XSS**

---

## 🔎 How to Find Reflected XSS — Step by Step

### Step 1 — Test Every Entry Point
- URL query string parameters
- URL file path
- Message body parameters
- HTTP headers (Referer, User-Agent, X-Forwarded-For)

### Step 2 — Submit Random Alphanumeric Value
- Use ~8 character unique string (e.g. `xss12abc`)
- Short enough to bypass filters
- Long enough to avoid accidental matches
- Check if it appears anywhere in the response

### Step 3 — Determine Reflection Context
Where does your input appear?

| Context | Example |
|---------|---------|
| Between HTML tags | `<p>your input</p>` |
| Inside HTML attribute | `<input value="your input">` |
| Inside JavaScript string | `var x = 'your input'` |
| Inside URL | `href="your input"` |

### Step 4 — Test a Candidate Payload
- Send request to **Burp Repeater**
- Inject XSS payload based on context
- Check response to see if payload survived

### Step 5 — Test Alternative Payloads
- If payload was blocked or modified → try bypasses
- Adjust based on what filtering is applied
- Refer to XSS cheatsheet for context-specific payloads

### Step 6 — Confirm in Browser
- Paste crafted URL in real browser
- Use `alert(document.domain)` to confirm execution
- Confirms the JS actually runs, not just appears in source

---

## 🔄 Reflected XSS in Different Contexts
The **location** of reflected data affects what payload works:

| Reflection Context | Payload Needed |
|-------------------|----------------|
| Raw HTML | `<script>alert(1)</script>` |
| HTML attribute | `"><script>alert(1)</script>` |
| JavaScript string | `'-alert(1)-'` |
| URL parameter | `javascript:alert(1)` |

---

## ❓ Common Questions

**Q: Reflected XSS vs Stored XSS?**
Reflected = input returned in immediate response via crafted URL.
Stored = input saved in DB, affects every user who visits the page.
Stored is more dangerous — no delivery mechanism needed.

**Q: Reflected XSS vs Self-XSS?**
Self-XSS = victim has to paste the payload themselves in their
own browser. Cannot be triggered via crafted URL.
Considered low impact because it requires social engineering.
Usually not accepted in bug bounty programs.

**Q: Why is Reflected XSS less severe than Stored?**
It needs an external delivery mechanism — the attacker must
trick the victim into clicking a crafted link.
Stored XSS is self-contained inside the vulnerable app itself.

**Q: What entry points should I always test?**
URL parameters, form fields, HTTP headers (User-Agent, Referer),
URL path segments, hidden form fields, and JSON/XML body params.

**Q: How do I confirm a reflection point is exploitable?**
First inject a unique string → confirm it appears in response.
Then inject a payload matching the reflection context.
Finally test in a real browser to confirm JS execution.

**Q: Why use `alert(document.domain)` instead of `alert(1)`?**
`alert(document.domain)` proves the script ran in the context
of the target domain — stronger proof of concept for reports.

**Q: Can Reflected XSS be used for account takeover?**
Yes — if the app uses cookies for sessions and HttpOnly is not
set, attacker can steal the session cookie via `document.cookie`
and hijack the victim's account.

**Q: What is the best tool to find Reflected XSS fast?**
Burp Suite's web vulnerability scanner finds most reflected XSS
quickly. For manual testing, Burp Intruder + Repeater workflow
is the most efficient approach.

**Q: Does HTTPS prevent Reflected XSS?**
No. HTTPS encrypts the connection but does not sanitize input
or output. XSS is an application-layer vulnerability.

**Q: What makes a good XSS bug bounty report?**
Clear reproduction steps, crafted URL, proof of impact
(session steal > alert popup), and suggested fix.
`alert(document.domain)` PoC is stronger than `alert(1)`.

---

## 🔗 Resources
- https://portswigger.net/web-security/cross-site-scripting/reflected
- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

Save this as web-vulnerabilities/xss/reflected-xss.md 🔥
Ready for Stored XSS theory whenever you share it!You said: keep this information with you at that end I will ask you for Question for Job Preparation you have to give me okay when we finish the xsskeep this information with you at that end I will ask you for Question for Job Preparation you have to give me okay when we finish the xss14 AprClaude responded: Got it!Got it! ✅
I'll keep track of everything we've covered so far:

✅ XSS Theory (types, impact, prevention)
✅ Reflected XSS Theory (how it works, finding it, contexts)
✅ Lab 01 — Reflected XSS (nothing encoded)

At the end when you finish XSS, just say "Give me Job Prep Questions" and I'll give you a solid set of interview questions based on everything we covered together.
Now share the Stored XSS theory whenever you're ready! 🔥You said: Lab: Stored XSS into HTML context with nothing encodedLab: Stored XSS into HTML context with nothing encoded
APPRENTICE
This lab contains a stored cross-site scripting vulnerability in the comment functionality.
To solve this lab, submit a comment that calls the alert function when the blog post is viewed.-> Solve this lab15 AprClaude responded: Here's your lab-02.Here's your lab-02.md note:
markdown# Lab 02 — Stored XSS into HTML Context with Nothing Encoded

**Level:** Apprentice
**Type:** Stored XSS
**Status:** ✅ Solved
**Platform:** PortSwigger Web Academy

---

## 🎯 Objective
Submit a comment containing an XSS payload that calls `alert()`
when any user views the blog post.

---

## 🧠 Concept
The comment functionality stores user input in the database
and displays it to every user who visits the page —
with **zero encoding or sanitization**.
This makes it Stored XSS — more dangerous than Reflected
because no crafted URL is needed.

---

## 🪜 Steps to Reproduce

1. Open the lab — it has a blog with a comment section
2. Click on any blog post
3. Scroll down to the comment form
4. Fill in the required fields:
Name    → anything (e.g. attacker)
Email   → anything valid (e.g. test@test.com)
Website → anything (e.g. https://test.com)
Comment → <script>alert(1)</script>
5. Click **Post Comment**
6. Get redirected back to the blog post
7. Page loads → `alert(1)` fires → Lab solved ✅

---

## 💉 Payload Used
```javascript
<script>alert(1)</script>
```
→ Injected in the **Comment** field

---

## 🔍 Why It Works
- Comment input is saved directly into the database
- When the page loads, stored comment is rendered as raw HTML
- Browser executes the `<script>` tag immediately
- Fires for **every user** who visits the blog post

---

## ⚡ Reflected vs Stored — Key Difference

| | Reflected | Stored |
|---|-----------|--------|
| Payload lives in | Crafted URL | Database |
| Who is affected | Only victims who click link | Every user who visits |
| Delivery needed | Yes (phishing link) | No |
| Severity | Medium | High |

---

## 💡 Key Takeaway
Always test comment boxes, review fields, profile bios,
chat messages — anywhere input is saved and displayed later.
Stored XSS is far more dangerous because it is
self-contained and requires no victim interaction beyond
simply visiting the page.

---

## 🛠️ Tools Used
- Browser only (no Burp needed for this one)

---

## 🔗 Reference
- https://portswigger.net/web-security/cross-site-scripting/stored

Save this as web-vulnerabilities/xss/labs/lab-02.md 🔥
Ready for Stored XSS theory or the next lab whenever you are!You said: Don't take labs I will give you labs sepraely and simple explantion of both of thisStored XSS
In this section, we'll explain stored cross-site scripting, describe the impact of stored XSS attacks, and spell out how to find stored XSS vulnerabilities.

What is stored cross-site scripting?
Stored cross-site scripting (also known as second-order or persistent XSS) arises when an pastedDOM-based XSS
In this section, we'll describe DOM-based cross-site scripting (DOM XSS), explain how to find DOM XSS vulnerabilities, and talk about how to exploit DOM XSS with different sources and sinks.

What is DOM-based cross-site scripting?
DOM-based XSS vulnerabilities usually arise when JpastedDon't take labs I will give you labs sepraely and simple explantion of both of this16 AprClaude responded: Here's your stored-xss.Here's your stored-xss.md and dom-xss.md notes:

markdown# Stored XSS — Theory Notes

**Source:** PortSwigger Web Academy
**Status:** ✅ Completed

---

## 🔍 What is Stored XSS?
Also known as **Persistent** or **Second-Order XSS**.
Arises when an application receives data from an untrusted source
and includes it in later HTTP responses unsafely.
Payload is **saved in the database** — fires for every user
who visits the page.

---

## ⚡ Key Difference from Reflected XSS

| | Reflected | Stored |
|---|-----------|--------|
| Payload lives in | Crafted URL | Database |
| Needs delivery? | Yes (phishing link) | No |
| Who is affected | Victim who clicks link | Every user who visits |
| Severity | Medium | High |

---

## 📥 Entry Points (Where to Inject)
- URL query string / message body parameters
- URL file path
- HTTP request headers
- Out-of-band routes (emails, tweets, third-party feeds)

## 📤 Exit Points (Where it Appears)
- Any HTTP response returned to any user
- Comments, profiles, chat messages, reviews, audit logs

---

## 🔎 How to Find Stored XSS

1. Submit unique value into every entry point
2. Monitor all responses and pages for that value appearing
3. Confirm it is **stored** not just reflected
4. Identify the context where it appears
5. Test appropriate payload for that context
6. Confirm execution in real browser

> ⚠️ Challenge: Any entry point could emit from any exit point.
> Work systematically — submit → monitor → confirm storage.

---

## 💡 Key Takeaway
Stored XSS is self-contained inside the app itself.
Attacker plants the payload and simply waits.
Especially dangerous when targeting logged-in users —
victim is **guaranteed** to be logged in when they encounter it.

markdown# DOM-based XSS — Theory Notes

**Source:** PortSwigger Web Academy
**Status:** ✅ Completed

---

## 🔍 What is DOM-based XSS?
Arises when JavaScript takes data from an attacker-controllable
**source** and passes it to a dangerous **sink**.
Vulnerability lives entirely in **client-side code** —
the server is not involved.

---

## 🔑 Two Key Concepts

| Term | Meaning | Example |
|------|---------|---------|
| Source | Where attacker-controlled data comes from | `location.search`, `location.hash`, `document.cookie` |
| Sink | Function that executes or renders data dangerously | `eval()`, `innerHTML`, `document.write()` |

---

## 🪜 How it Works
Source → JavaScript processes it → Sink → Code executes
Most common source = **URL** via `window.location`

---

## 🔎 How to Find DOM XSS

### HTML Sinks
- Inject unique string into source (e.g. URL parameter)
- Use **DevTools → Ctrl+F** to search DOM for your string
- Identify context → craft payload to break out
- ⚠️ View Source won't work — use DevTools Inspector

### JavaScript Execution Sinks
- Use **Ctrl+Shift+F** in DevTools to search all JS code
- Find where source is referenced
- Add breakpoints in JS debugger
- Track data flow from source → sink

### With Burp Suite
- Use **DOM Invader** extension (built into Burp's browser)
- Automates finding taint flow from source to sink

---

## ⚠️ Common Sinks to Look For

### Native JS Sinks
document.write()
document.writeln()
element.innerHTML
element.outerHTML
element.insertAdjacentHTML
eval()
setTimeout()
location.href

### jQuery Sinks
html()
append()
after()
before()
$() selector
attr()
jQuery.parseHTML()

---

## 🔥 Special Cases

### jQuery `attr()` Attack
```javascript
// Vulnerable code
$('#backLink').attr("href", location.search.get('returnUrl'));

// Payload
?returnUrl=javascript:alert(document.domain)
```

### jQuery `$()` hashchange Attack
```html
<iframe src="https://vulnerable-site.com#"
onload="this.src+='<img src=1 onerror=alert(1)>'">
```

### AngularJS Attack
- Sites using `ng-app` process AngularJS expressions
- Can execute JS inside `{{ }}` without angle brackets
{{constructor.constructor('alert(1)')()}}

---

## 💡 Key Takeaway
DOM XSS is hardest to find — no server interaction,
no response to analyze. You must read the JavaScript,
find the source → sink flow, and exploit it client-side.
Always use DevTools + DOM Invader for efficiency.

Save these as:

web-vulnerabilities/xss/stored-xss.md
web-vulnerabilities/xss/dom-xss.md

Ready for the labs whenever you share them! 🔥You said: Lab: Reflected XSS into HTML context with most tags and attributes blockedLab: Reflected XSS into HTML context with most tags and attributes blocked
PRACTITIONER
LABSolved
This lab contains a reflected XSS vulnerability in the search functionality but uses a web application firewall (WAF) to protect against common XSS vectors.
To solve the lab, perform a cross-site scripting attack that bypasses the WAF and calls the print() function.
Note
Your solution must not require any user interaction. Manually causing print() to be called in your own browser will not solve the lab.Show more17 AprClaude responded: Here's your lab-03.Here's your lab-03.md note:
markdown# Lab 03 — Reflected XSS into HTML Context with Most Tags and Attributes Blocked

**Level:** Practitioner
**Type:** Reflected XSS + WAF Bypass
**Status:** ✅ Solved
**Platform:** PortSwigger Web Academy

---

## 🎯 Objective
Bypass a WAF blocking most XSS tags and attributes,
and call `print()` with no user interaction required.

---

## 🧠 Concept
A Web Application Firewall (WAF) blocks common XSS payloads
like `<script>`, common event handlers like `onclick`, `onmouseover`.
The goal is to find tags and attributes the WAF **missed**
and craft a payload that fires automatically without user interaction.

---

## 🪜 Steps to Reproduce

### Step 1 — Confirm Basic XSS is Blocked
<script>print()</script>
→ 400 Bad Request — WAF blocks it

### Step 2 — Fuzz for Allowed Tags Using Burp Intruder
1. Send search request to **Burp Intruder**
2. Set payload position:
/search?q=<§tag§>
3. Use PortSwigger XSS cheatsheet tag list as payload
4. Look for responses with **200 status** (not blocked)
5. Allowed tag found:
   <body>
````
Step 3 — Fuzz for Allowed Events/Attributes

Set payload position on event attribute:

   /search?q=<body+§event§=print()>

Use PortSwigger XSS cheatsheet events list as payload
Look for 200 status responses
Allowed event found:

   onresize
Step 4 — Build the Payload
html<body onresize=print()>
But this needs the body to actually resize — needs user interaction.
Step 5 — Deliver via iframe (No User Interaction)
Wrap it in an iframe that auto-resizes on load:
html<iframe 
  src="https://YOUR-LAB-ID.web-security-academy.net/?search=<body onresize=print()>" 
  onload="this.style.width='100px'">
</iframe>

iframe loads the vulnerable page with the payload
onload resizes the iframe width
This triggers onresize on the <body> inside
print() fires automatically ✅

Step 6 — Deliver the Exploit

Go to Exploit Server
Paste the iframe payload in the body
Click Deliver to Victim
Lab solved ✅


💉 Final Payload
html<iframe 
  src="https://YOUR-LAB-ID.web-security-academy.net/?search=<body onresize=print()>" 
  onload="this.style.width='100px'">
</iframe>

🔍 Why It Works

WAF blocks <script> and common events like onclick
But <body> tag and onresize event were not blacklisted
iframe auto-triggers resize without any user interaction
Bypasses both the WAF and the no-interaction requirement


🧱 WAF Bypass Methodology
Step Action
1.Fuzz all HTML tags → find allowed ones
2.Fuzz all event attributes → find allowed ones
3Build payload using allowed tag + event
4.Wrap in iframe for auto-execution