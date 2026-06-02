# PicoCTF — Cryptography

**Platform:** PicoCTF / picoGym
**Status:** 🔄 In Progress

---

## 📋 Challenges List

| # | Challenge | Difficulty | Status |
|---|-----------|------------|--------|
| 01 | Caesar Cipher (Theory) | Easy | ✅ |
| 02 | Caesar Decrypt | Easy | ✅ |
| 03 | Base64 (Theory) | Easy | ✅ |
| 04 | interencdec | Easy | ✅ |
| 05 | New Caesar | Medium | ✅ |
| 06 | RSA Resources (Theory) | Easy | ✅ |
| 07 | Mind Your Ps and Qs | Medium | 🔄 |
| 08 | RSA Oracle | Hard | ✅ |

---

## Challenge 01 — Caesar Cipher (Theory)
**Status:** ✅ Reading Module

**💡 What is Caesar Cipher?**
Shifting every letter in the message by a fixed number.
Example: shift = 3 → A becomes D, B becomes E, Z becomes C

```
Original:  A B C D E F G H I J ...
Shifted+3: D E F G H I J K L M ...
```

**Formula:**
```
Encrypt: (position + shift) % 26
Decrypt: (position - shift) % 26
```

**Example:**
```
H (7) + 3 = 10 = K
E (4) + 3 = 7  = H
→ "HE" becomes "KH"
```

**Key Rule:** Only shift alphabet letters. Keep `{ } _ 0-9` unchanged.

---

## Challenge 02 — Caesar Decrypt
**Status:** ✅ Solved
**Flag:** `picoCTF{caesar_d3cr9pt3d_ea60e00b}`

**How I Solved It:**
Tried all 26 possible shifts until the output was readable English.

**💡 Key Learning:**
Caesar shift is not always 3. If shift is unknown,
try all 26 shifts — one of them will give readable text.
This is called brute forcing a Caesar cipher.

---

## Challenge 03 — Base64 (Theory)
**Status:** ✅ Reading Module

**💡 What is Base64?**
Base64 is an ENCODING method — not encryption.
It converts binary data into readable ASCII text.
Used to safely transmit data over text-based protocols.

**How to recognize Base64:**
- Only contains: A-Z, a-z, 0-9, +, /
- Often ends with `=` or `==` (padding)
- Looks like: `cGljb0NURns...`

**Decode in Linux:**
```bash
echo "cGljb0NURns=" | base64 -d
```

**Encode in Linux:**
```bash
echo "hello" | base64
```

**Important:** Base64 is NOT encryption — anyone can decode it instantly.

---

## Challenge 04 — interencdec
**Status:** ✅ Solved
**Flag:** `picoCTF{caesar_d3cr9pt3d_ea60e00b}`

**How I Solved It:**
```
Step 1: Saw == at end → Base64
Step 2: Decoded Base64 → still encoded
Step 3: Decoded Base64 again → got Caesar text
Step 4: Tried ROT shifts → ROT19 gave readable flag
```

**💡 Key Learning:**
Data can be encoded MULTIPLE times — always check
if the output is still encoded after decoding.
`==` at the end is a strong hint for Base64.

**Decoding chain:**
```
Base64 → Base64 → Caesar/ROT → Flag
```

---

## Challenge 05 — New Caesar
**Status:** ✅ Solved

**How I Solved It:**
Read the Python code → understood two-step encryption:
1. Custom base16 encoding
2. Caesar shift

Reversed the steps in opposite order:
1. Unshift Caesar
2. Decode base16

Tried all single-letter keys until output was readable.

**💡 Key Learning:**
To reverse any encryption, do the steps in REVERSE ORDER.
If encryption is: Step1 → Step2
Then decryption is: Undo Step2 → Undo Step1

---

## Challenge 06 — RSA Resources (Theory)
**Status:** ✅ Reading Module

**💡 What is RSA?**
RSA is a public key encryption system.
Uses two mathematically linked keys:
- Public Key → anyone can use it to ENCRYPT
- Private Key → only owner uses it to DECRYPT

**Key Values:**
```
p = large prime number
q = large prime number
n = p × q  (public, part of public key)
e = public exponent (usually 65537)
d = private exponent (secret)
```

**How it works:**
```
Encrypt: ciphertext = message^e mod n
Decrypt: message = ciphertext^d mod n
```

**Why it's secure:**
Breaking RSA requires factoring `n` back into `p` and `q`.
When p and q are very large primes, this is computationally
impossible with current computers.

---

## Challenge 07 — Mind Your Ps and Qs
**Status:** 🔄 In Progress

**How to Solve:**
```python
# Given: n (small enough to factor), e, c (ciphertext)

# Step 1: Factor n into p and q
# Use factordb.com or sympy

# Step 2: Calculate phi(n)
phi = (p - 1) * (q - 1)

# Step 3: Find private key d
from sympy import mod_inverse
d = mod_inverse(e, phi)

# Step 4: Decrypt
m = pow(c, d, n)

# Step 5: Convert to text
import binascii
flag = binascii.unhexlify(hex(m)[2:]).decode()
print(flag)
```

**💡 Key Learning:**
If n is small, it can be factored → RSA is broken.
Real RSA uses n that is 2048+ bits long.
This is why choosing large primes is critical.

---

## Challenge 08 — RSA Oracle
**Status:** ✅ Solved
**Flag:** `picoCTF{su((3ss_(r@ck1ng_r3@_4955eb5d}`

**How I Solved It:**
```
1. Connected to RSA oracle
2. Could not decrypt password.enc directly (blocked)
3. Used RSA Multiplicative Property:
   Encrypt(a × b) = Encrypt(a) × Encrypt(b) mod n
4. Encrypted value 2
5. Multiplied Encrypt(2) × password.enc
6. Sent modified ciphertext → oracle decrypted it
7. Divided result by 2 → recovered original password
8. Used password to decrypt secret.enc with OpenSSL
```

**💡 Key Learning — RSA Multiplicative Property:**
```
RSA has a mathematical property:
Encrypt(m1 × m2) = Encrypt(m1) × Encrypt(m2) mod n

This means:
If you want to decrypt blocked message C:
→ Multiply C by Encrypt(2)
→ Oracle decrypts it (not the original C anymore)
→ Divide result by 2 → get original message
```

This is called a **Chosen Ciphertext Attack** or
**Decryption Oracle Attack**.
Exposing a decryption oracle is a critical vulnerability in real systems.

---

## 🗺️ Cryptography Roadmap for Pentesting

### Level 1 — Recognition (Must Know)
Instantly recognize these when you see them:

| Encoding | How to Spot |
|----------|-------------|
| Base64 | Ends with = or ==, uses A-Za-z0-9+/ |
| Hex | Only 0-9 and A-F characters |
| Binary | Only 0s and 1s |
| ROT13 | Letters shifted, punctuation unchanged |
| URL Encoding | %XX format (e.g. %20 = space) |
| JWT | Three parts separated by dots |
| MD5 Hash | 32 hex characters |
| SHA256 Hash | 64 hex characters |

### Level 2 — Hashing
```
Hash = one-way function (cannot be reversed)
MD5, SHA1, SHA256, bcrypt, argon2

Real use: password storage, file integrity
Bug bounty: weak hashing = vulnerability
Tool: CrackStation to crack weak hashes
```

### Level 3 — Symmetric Encryption
```
AES = most important to know
Key + data in → encrypted data out
Same key used for both encrypt and decrypt

Weak modes to look for in bug bounty:
- AES ECB Mode (patterns visible)
- Hardcoded keys in source code
- Weak key generation
```

### Level 4 — RSA (Asymmetric)
```
Two keys: public (encrypt) + private (decrypt)
Attacks: small e, weak n, oracle attacks
Practice: CryptoHack RSA Track
```

### Level 5 — JWT (Bug Bounty Gold)
```
JSON Web Tokens used in web authentication
Structure: header.payload.signature
Common bugs:
- Algorithm confusion (RS256 → HS256)
- None algorithm attack
- Weak secret key
Practice: JWT.io
```

---

## 🔗 Resources
- https://cryptohack.org (best practical crypto)
- https://cyberchef.org (encode/decode everything)
- https://crackstation.net (hash cracking)
- https://jwt.io (JWT debugging)
- https://factordb.com (factor large numbers)