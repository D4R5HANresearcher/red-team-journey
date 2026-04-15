# Reflected XSS — Theory Notes

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