# Lab 02 — Stored XSS into HTML Context with Nothing Encoded

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
4. Fill in the required fields:     Name    → anything (e.g. attacker)
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