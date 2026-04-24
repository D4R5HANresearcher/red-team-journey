# Lab 01 — Reflected XSS into HTML Context with Nothing Encoded

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
2. Type a test string to confirm reflection:test123
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