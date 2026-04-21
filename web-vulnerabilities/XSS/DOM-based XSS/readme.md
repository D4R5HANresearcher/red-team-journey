# DOM-based XSS — Theory Notes

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