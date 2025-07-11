
# 🧠 Day 5: SSTI Payloads, IDOR & Price Manipulation CTFs, and Digital Forensics

---

## 🔁 Server-Side Template Injection (SSTI) – Advanced Payload

We explored an **advanced SSTI payload**:
```jinja
{{cycler.__init__.__globals__.os.popen('whoami').read()}}
```

- This payload uses Jinja2’s internal Python objects to break out of the template and execute system-level commands like `whoami`.
- It leverages Python's object chaining and the `os.popen()` function to run commands on the server.

> ✅ Note: Ensure the server is using **Jinja2 or a compatible Python-based template engine** for this to work.

---

## 📊 CVSS – Calculating Vulnerability Severity

We used the [FIRST.org CVSS Calculator](https://www.first.org/cvss/calculator/3.1) to **rate vulnerabilities** based on various metrics.

### 🔍 CVSS Parameters:
- **Attack Vector** (Network, Adjacent, Local, Physical)
- **Attack Complexity** (Low/High)
- **Privileges Required** (None/Low/High)
- **User Interaction**
- **Confidentiality/Integrity/Availability Impact**

The result is a score from **0 (None)** to **10 (Critical)**.

---

## 🕹️ CTF 1: IDOR - Password Reset Exploit

### 🔧 Scenario:
- Given: Credentials to log in to a website.
- Goal: Access and modify **another user's password**.

### 🛠️ Steps:
1. Log in with provided credentials.
2. Go to the "Change Password" page.
3. Form has 3 fields: `username`, `old_password`, `new_password`.
4. Enable **Burp Suite Intercept**.
5. Fill valid data and click **Submit**.
6. Modify the **username field** in the intercepted request to the **target user**.
7. Forward the request.
8. Login to the target user account with the new password.

![CTF_IDOR](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/Screenshot%202025-07-11%20110336.png)

> 💡 **Key lesson**: The vulnerability lies in the server not verifying if the requester is changing **their own** password.

---

## 🕹️ CTF 2: E-Commerce Price Manipulation

### 🎯 Challenge:
Buy a $1500 PS5 while having only $1000.

### 🛠️ Steps:
1. Choose an affordable product.
2. Fill in purchase details.
3. Enable **Burp Suite Intercept**.
4. Capture request on **checkout**.
5. Modify:
   - `product_id` → change to PS5's ID
   - `price` → reduce to under $10
   - `product_name` → update if **UTF-16 encoded**
![CTF](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/Screenshot%202025-07-11%20110253.png)

> 💡 The server-side validation was missing or weak, allowing arbitrary client-side data manipulation.

---

## 🔍 Autopsy – Digital Forensics Tool

**Autopsy** is a GUI-based digital forensics platform used to:
- Recover deleted files
- Investigate disk images
- Analyze timeline activity
- Examine file metadata

Useful in:
- **Cybercrime investigations**
- **File recovery**
- **Incident response**

---

## 💽 TRIM & Filesystem Behavior

To check TRIM support on Windows:
```bash
fsutil behavior query DisableDeleteNotify
```

### 📌 Output Meaning:
- `DisableDeleteNotify = 0`: TRIM is **enabled**
- `DisableDeleteNotify = 1`: TRIM is **disabled**

TRIM helps SSDs manage unused data blocks efficiently. If TRIM is disabled, **deleted files can often be recovered**, which is useful for forensic purposes.

> 🕗 Autopsy may take longer to upload large files for analysis, especially from SSDs with TRIM.

---

## 🌐 Network Forensics with Wireshark

**Wireshark** is a packet analyzer used to capture and inspect network traffic in real time.

![Wireshark](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/wireshark.jpg)

### 🛠️ Lab Command:
```bash
http.request.method == "POST"
```
This filter shows **only HTTP POST requests**, which often contain:
- Login data
- Form submissions
- File uploads

> 🔍 In the lab, we inspected HTML traffic and found the flag in POST data under the **HTML protocol**.

![Wireshark](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/wireshark_lab.jpg)

---

## 🎯 Grabify – IP Logger

**Grabify** is a tool that:
- Generates **tracking URLs**
- Logs IP address, user-agent, location, and other metadata
- Commonly used for **social engineering and phishing awareness**

### 🔧 Usage:
1. Visit [grabify.link](https://grabify.link)
2. Input a redirect URL (e.g., msn.com)
3. Generate a tracking link
4. Send it to the victim
5. Once clicked, you can view detailed request logs

![grabify](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/grabify.jpg)

---

## 🛡️ VirusTotal – Malware Scanner

**VirusTotal** is a web-based platform to:
- Scan **files, URLs, IPs, and domains** for malicious content
- Analyze behavior with **static and dynamic** techniques
- Powered by multiple antivirus engines

Great for:
- Suspicious file uploads
- Investigating phishing sites
- Malware sample analysis

![virustotal](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/virustotal.jpg)

---

## 🔎 theHarvester – OSINT Tool

**theHarvester** is a Kali Linux tool for **gathering emails, usernames, domains, and IPs** from public sources.

### 📌 Usage:
```bash
theHarvester -d example.com -l 100 -b google
```

### 🔧 Flags:
- `-d`: Domain name to search
- `-l`: Limit the number of results
- `-b`: Source engine (google, bing, linkedin, etc.)
- `-h`: Show help menu

Used for **reconnaissance in penetration testing**.

---


---

## 🧠 Maltego – Network Forensics & OSINT Tool

**Maltego** is a powerful data mining tool used in:
- **Network forensics**
- **Threat intelligence**
- **OSINT (Open-Source Intelligence)**

### 🔍 Features:
- Visual link analysis graphs
- Entity correlation across people, companies, domains, emails, and infrastructure
- API integrations with Threat Intelligence sources (like Shodan, VirusTotal, etc.)

### 🛠️ Use Cases:
- Tracking relationships between threat actors
- Mapping target infrastructure during reconnaissance
- Investigating data breaches or malicious campaigns

![maltigo](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/maltigo.jpg)

> 🚀 Maltego excels at identifying complex relationships across large datasets.


## ✅ Day 5 Summary

- Advanced SSTI exploitation
- Rated real-world CVSS vulnerabilities
- Solved IDOR and e-commerce logic flaws in CTFs
- Explored digital forensics using Autopsy & TRIM behavior
- Practiced network forensics in Wireshark
- Discovered powerful OSINT and malware analysis tools:
  - Grabify
  - VirusTotal
  - theHarvester

---
