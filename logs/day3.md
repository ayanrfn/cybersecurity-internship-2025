
# 🧠 Day 3: CSRF, SSTI, Directory Traversal, IDOR, XSS, and Username Enumeration

---

## 🔐 Cross-Site Request Forgery (CSRF)

- **Definition**: CSRF tricks a victim's browser into making **unwanted authenticated requests** to a web application where the victim is already logged in.
- The attacker can perform actions on behalf of the victim **without their knowledge**.
- Example: Victim clicks a malicious link that makes them unknowingly post a comment, change their email, or even transfer funds — all under their session.
- The victim thinks they’re performing legitimate actions, but they're actually affecting **the attacker's account or goals**.

### ⚙️ Lab – Host Header Manipulation

- Logged into a website and captured the request using **Burp Suite**.
- Modified the **Host header** in the intercepted request from:
  ```
  Host: jimpl.com
  ```
  to:
  ```
  Host: bing.com
  ```
- Forwarded the request step-by-step. It caused the browser to redirect and load Bing.com.

![CSRF](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/bing.jpg)

> 💡 This demonstrates how **Host Header Injection** could be abused for cache poisoning, password reset poisoning, or even open redirect vulnerabilities.

---

## 🧠 Server-Side Template Injection (SSTI)

- Happens when user input is rendered inside a **server-side template engine** without sanitization.
- Allows execution of arbitrary expressions or code in server context.

### 🧪 Example:
Input:
```html
{{7*7}}
```
Output:
```
49
```
- This indicates that the template engine evaluated the input — a clear sign of SSTI.

### 🧱 Commonly vulnerable template engines:
- **Jinja2** (Python)
- **Twig** (PHP)
- **Freemarker** (Java)

---

## 🗂️ Directory Traversal Attack

- Allows attackers to access files **outside the intended directory**.
- This is achieved by manipulating file paths using `../` sequences.

### 🔓 Example:
A vulnerable file download URL:
```
http://example.com/download?file=report.pdf
```
An attacker modifies it to:
```
http://example.com/download?file=../../../../etc/passwd
```
- This accesses the system password file on Linux.

![Directory_trav](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/direc_trav.jpg)

---

## 🆔 Insecure Direct Object Reference (IDOR)

- Happens when a web app exposes references to internal objects (user IDs, document names) **without proper access control**.

### 🔍 Real-World Example:
```
http://example.com/user/12345/profile
```
- Changing `12345` to `12346` could show another user’s profile **if no access control is implemented**.

### 🔄 Types of Privilege Escalation:
- **Vertical Privilege Escalation**: A low-level user performs actions meant for admins.
- **Horizontal Privilege Escalation**: A user accesses **another user's data** without being authorized.

### 🔧 How it's done:
- Use **Burp Suite** to intercept the request.
- Modify the user ID or object reference.
- Forward the request and check if access is granted.

### 🔐 Defense Mechanisms:
1. **Cookie validation & integrity checks**
2. **Hashing/UUIDs** instead of raw IDs
3. **Multi-Factor Authentication (MFA)**

---

## 🧬 Username Enumeration

- Occurs when a system **reveals whether a username/email exists** based on login responses.

### 💡 Real-World Example:
- Login page shows:
  - `Invalid username` if user doesn't exist
  - `Incorrect password` if user exists
- This allows brute-force or phishing targeting valid users.

### ⚙️ Lab – Username Enumeration Using Burp Suite

1. Open login page and type any username/password.
2. **Enable Intercept** in Burp Suite and click login.
3. Capture the POST request.
4. Send it to **Intruder**.
5. Highlight the `username` field → click **Add $**.
6. Go to Payload tab → paste a **list of common usernames**.
7. Start attack.
8. Observe **length/status difference** → longest response indicates **valid username**.
9. Once found, password brute-force can be attempted.
![username_enumeration](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/dir_trav.jpg)

![username_enumeration](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/user_enu.jpg)

---

## ✅ Day 3 Summary

- Explored **CSRF** attack and how Burp Suite can be used to modify host headers.
- Understood **SSTI** by injecting expressions in templates.
- Practiced **Directory Traversal** to access protected server files.
- Studied **IDOR** and privilege escalation (horizontal & vertical).
- Learned about **username enumeration** and conducted a lab using Burp Intruder.

> 🔐 Real-world attacks are a combination of these vulnerabilities. Always validate inputs, restrict access, and monitor user actions.

---
