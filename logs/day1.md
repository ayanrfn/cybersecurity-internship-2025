# 🗓️ Day 1 – Cybersecurity Internship Log  
**Date:** July 7, 2025  
**Focus Areas:** Linux Fundamentals, Networking Concepts, Cryptography, Web Exploitation (LFI, Injection)

---

## 📌 Topics Covered

---

### 🔧 1. Basic Linux Command Line Usage

We began with an introduction to essential Linux commands, which are fundamental for system navigation, scripting, and offensive security tasks.

#### 🧱 File and Directory Operations

| Command        | Description                                  |
|----------------|----------------------------------------------|
| `cd`           | Change directory                             |
| `cd ..`        | Move one level up in the directory structure |
| `mkdir`        | Create a new directory                       |
| `touch`        | Create a new empty file                      |
| `mv`           | Move or rename a file or directory           |
| `rm`           | Remove files or directories                  |
| `ls`           | List files in the current directory          |
| `pwd`          | Print working directory                      |
| `clear`        | Clear the terminal screen                    |
| `cat`          | Display file contents                        |
| `nano` / `vim` | Edit files from the terminal                 |

#### 🔒 Privilege Management

- `sudo`: Run a command with **superuser (root)** privileges. Required for system-level tasks like installing packages, editing config files, or accessing restricted directories.

---

### 🌐 2. Network Interfaces & Configuration

#### 🔎 Checking Network Info

- `ifconfig`: Displays network configuration for all interfaces.  
  (*Note: Replaced by `ip a` in modern Linux systems*)

#### 📡 Common Interface Types

| Interface | Description                                                   |
|-----------|---------------------------------------------------------------|
| `eth0`    | Ethernet interface (usually the primary wired network)        |
| `wlan0`   | Wireless LAN interface (Wi-Fi)                                |
| `lo`      | Loopback interface (used for internal system communication)   |


---

### 🧠 3. MAC Address & IP Addressing

- **MAC Address (Media Access Control):**  
  A hardware identifier linked to the **NIC (Network Interface Card)**. It's permanent and used for Layer 2 communication.

- **IP Address (Internet Protocol):**  
  Used to identify a device at Layer 3.  
  - **IPv4:** 32-bit (e.g., `192.168.1.10`)  
  - **IPv6:** 128-bit (e.g., `2001:db8::ff00:42:8329`)

- **Router Table:**  
  Maintains mappings of destination IP addresses to MAC addresses and routes traffic accordingly. Can be viewed with commands like `route -n` or `ip route`.

---

### 🔐 4. Cryptographic Hashing

We discussed **hash functions** and their role in cybersecurity.

| Hash Function | Description                                   |
|---------------|-----------------------------------------------|
| MD5           | Fast but insecure; 128-bit; not collision-safe |
| SHA1          | More secure than MD5 but still outdated        |

#### 🔍 Key Concepts

- Hashes are **irreversible** – one-way functions
- Used for:
  - Password storage
  - File integrity verification
  - Digital signatures
- **Rainbow Tables:** Precomputed hash databases used for cracking weak passwords
- **Integrity Check:** If a hash changes during transmission, a **man-in-the-middle** attack might be present

---

### 💻 5. Lab 1: Extracting Information from HTML Comments

#### 🧪 Objective:
Find sensitive information hidden in the source code of a demo web application.

#### 💡 Key Takeaways:
- Developers often leave debugging info in `<!-- HTML comments -->`
- These may reveal:
  - Usernames
  - Password hints
  - Internal URLs or file paths
- We inspected the **page source (Ctrl + U)** to find hardcoded credentials used for login.

---

### 🗂️ 6. LFI – Local File Inclusion

#### 🧠 What is LFI?

LFI allows an attacker to **include and read files from the server’s local filesystem** by manipulating parameters in the URL.

#### 🔧 Requirements:

- A vulnerable parameter like:  http://vulnerable-site.com/index.php?page=home
- If the application includes the file blindly, we can manipulate it: http://vulnerable-site.com/index.php?page=../../../../etc/passwd
- This reveals sensitive files like `/etc/passwd` (Linux user file)

#### 🛡️ Bypass Techniques:

- Encode payloads: `..%2f..%2f..%2fetc%2fpasswd`
- Try variations: `.../.../.../etc/passwd` or use null bytes

---

### 🧪 Lab 2: Practical LFI Exploitation

#### 🧠 Objective:

To exploit a vulnerable web application that allows **Local File Inclusion (LFI)** by manipulating a URL parameter to load unintended files from the server.

---

#### 🧪 Scenario:

We were provided a web application with a URL structure like: http://vulnerable-site.com/index.php?file=about.html

This `file` parameter was vulnerable to LFI. By replacing it with a file path pointing to sensitive system files, we attempted to retrieve their contents.

---

#### 📂 What does `../` mean?

- `../` is a **directory traversal operator**.
- It means **"go up one folder"** in the directory structure.
- Multiple `../` chained together allow us to **navigate to higher-level directories** from the current one.

##### 🔄 Example:

Assuming the vulnerable script is located in: /var/www/html/includes/

Then:
- `../` moves to `/var/www/html/`
- `../../` moves to `/var/www/`
- `../../../etc/passwd` would eventually lead to `/etc/passwd`, a file containing Linux user account info.

So, the payload: ?file=../../../../etc/passwd

Attempts to access a **sensitive system file** by navigating up the directory tree.

---

#### 🛡️ Bypass Techniques:

To overcome basic filtering or blocklists, we tried different encodings and variations of traversal:

- **Encoded Traversal:**  ..%2f..%2f..%2fetc%2fpasswd
- **Alternate Formats:**  .../.../.../etc/passwd
                          ..//..//..//etc/passwd


These bypass common naive filters that only check for standard `../` patterns.

---

#### ✅ Outcome:

- We successfully used directory traversal to read the contents of `/etc/passwd`
- Understood how improper validation of user-supplied parameters can expose sensitive system files

> 🔐 Lesson: Always sanitize and validate user input — especially in file paths.

### 💉 7. Injection Attacks – Focus on SQL Injection

#### 🔍 What is Injection?

Injection occurs when **user input is passed directly into backend queries** like SQL, allowing attackers to manipulate them.

---

#### 📌 SQL Injection Key Concepts:

| Technique                | Description |
|--------------------------|-------------|
| `' OR 1=1 --`            | Makes the condition always true |
| `'--`                    | Comments out the rest of the SQL query |
| `' OR 'x'='x'`           | Another always-true condition |
| `SLEEP(5)` (Blind SQLi) | Server delay proves database execution |
| `?` (Prepared Statement) | Used in safe parameterized queries |

---

#### 🔧 Requirements for SQLi:

- A user input field (username, search, login, etc.)
- A connection to an SQL backend
- Either visible **error messages** or **time-based testing** to confirm injection

---

#### 🧪 Lab 3: Exploiting a Login Form

**Scenario:** Login to an admin account without knowing the password

- **Username:** `admin' --`  
- **Password:** `anything`  
(gets ignored due to the comment `--`)

- Backend Query becomes:
```sql
SELECT * FROM users WHERE username = 'admin' --' AND password = 'anything';




