# Stored XSS — Theory Notes

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