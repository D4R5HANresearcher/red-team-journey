   # Cross-Site Scripting (XSS) — Theory Notes

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
- Example:https://insecure-website.com/status?message=<script>alert(1)</script>
   
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