# Module 4: Threats & Vulnerabilities

To defend systems, you need to understand what you’re up against.  
This module introduces the **common threats** and **vulnerabilities** that attackers exploit.

---

## 1. What is a Threat vs a Vulnerability?

- **Threat** → A potential danger.  
  Example: Hackers trying to steal passwords.  

- **Vulnerability** → A weakness that can be exploited.  
  Example: An outdated program with a known bug.  

- **Exploit** → The actual method used to take advantage of a vulnerability.  
  Example: Running malicious code to break into the system.

---

## 2. Common Threats

- **Malware** → Malicious software (viruses, worms, Trojans, ransomware).  
- **Phishing** → Fake emails/websites tricking users into giving credentials.  
- **Denial of Service (DoS/DDoS)** → Flooding a system with traffic to take it offline.  
- **Man-in-the-Middle (MitM)** → Intercepting communication between two parties.  
- **Insider Threats** → Employees misusing access.  
- **Social Engineering** → Tricking people into giving up access or information.  

---

## 3. Common Vulnerabilities

- **Unpatched Software** → Old versions with known flaws.  
- **Weak Passwords** → Easy to guess or reused.  
- **Misconfigured Systems** → Default settings left unchanged.  
- **Open Ports/Services** → Services running unnecessarily.  
- **SQL Injection (SQLi)** → Inputting malicious database queries.  
- **Cross-Site Scripting (XSS)** → Injecting malicious scripts into websites.  

---

## 4. Vulnerability Databases

Security researchers share known vulnerabilities:
- **CVE (Common Vulnerabilities and Exposures)** → Public list of flaws.  
- **NVD (National Vulnerability Database)** → Official U.S. gov. database.  
- Example CVE: *CVE-2017-0144* (the Windows SMB bug used in WannaCry ransomware).  

---

## 5. Real-World Example

- **WannaCry Ransomware (2017):**  
  - Exploited a Windows vulnerability in SMB protocol.  
  - Spread rapidly across the globe.  
  - Encrypted files and demanded Bitcoin ransom.  
  - Lesson: Patching systems is critical.

---

## 6. Mini Labs (Try It Yourself)

### 🕵️ Explore CVE Database
1. Visit [https://cve.mitre.org](https://cve.mitre.org).  
2. Search for a vulnerability in software you know (e.g., "Windows 10").  
3. Read the description and think: *How could this be dangerous?*

### 🔑 Weak Password Test (Linux)
```bash
# Try cracking your own weak password hash
echo -n "password123" | md5sum
````

(Shows how attackers can guess simple hashes quickly.)

---

## 7. Tools to Know

* **Nmap** → Scan for open ports.
* **Nessus** → Vulnerability scanner.
* **OpenVAS** → Open-source vulnerability scanning.
* **Burp Suite** → Web app vulnerability testing.

---

## Why This Matters

Cybersecurity isn’t just about defending blindly.
You need to **know the threats** and **recognize vulnerabilities** before attackers do.
This knowledge is the foundation for penetration testing, ethical hacking, and defense strategies.

