# Basics of Cybersecurity

Welcome to the first step of your cybersecurity journey!  
This module introduces the foundational concepts you need to understand before diving into tools, techniques, and labs.

---

## 1. What is Cybersecurity?

Cybersecurity is the practice of protecting systems, networks, and data from digital attacks.  
Think of it as locking your doors and windows in the digital world — except attackers may be thousands of miles away.

**Key points:**
- Protects **confidentiality** (keeping secrets safe)
- Maintains **integrity** (ensuring data isn’t altered)
- Ensures **availability** (systems are accessible when needed)

Together, these are called the **CIA Triad**.

---

## 2. The CIA Triad

- **Confidentiality** → Only authorized people can access information.  
  *Example: Using passwords, encryption.*
- **Integrity** → Data must remain accurate and unaltered.  
  *Example: Hashing, file integrity checks.*
- **Availability** → Systems and data must be available when needed.  
  *Example: Redundant servers, DDoS protection.*

---

## 3. Types of Cybersecurity

1. **Network Security** – Protecting data as it travels.  
2. **Application Security** – Securing software and code.  
3. **Information Security** – Protecting data wherever it lives.  
4. **Operational Security (OpSec)** – Managing how people use and share data.  
5. **Physical Security** – Preventing unauthorized physical access.  

---

## 4. Common Cyber Threats

- **Malware** → Malicious software (viruses, worms, trojans).  
- **Phishing** → Tricking people into giving up sensitive info.  
- **Man-in-the-Middle (MITM)** → Eavesdropping on communication.  
- **Denial-of-Service (DoS/DDoS)** → Overloading a service to make it unavailable.  
- **SQL Injection** → Inserting malicious code into a database query.  

---

## 5. Basic Cyber Hygiene

Just like washing your hands, there are simple habits that keep systems safe:
- Use strong, unique passwords.  
- Enable multi-factor authentication (MFA).  
- Keep software and systems updated.  
- Be cautious with email links/attachments.  
- Backup data regularly.  

---

## 6. Try It Yourself (Mini Exercises)

### 🔑 Checking Password Strength
On Linux/macOS, try generating a strong password using `pwgen`:
```bash
# Install pwgen (Ubuntu/Debian)
sudo apt-get install pwgen

# Generate a random 16-character password
pwgen 16 1
````

### 🔍 Checking File Integrity

Create a file and generate its hash:

```bash
echo "Hello Cybersecurity" > file.txt
sha256sum file.txt
```

If the file changes, the hash will too — showing a loss of **integrity**.

---

## 7. Tools to Know (Basics)

* **Wireshark** → Analyzes network traffic.
* **Nmap** → Scans networks for open ports/services.
* **Hashing tools (sha256sum, md5sum)** → Verify data integrity.

(We’ll go deeper into these later modules.)

---

✅ You’ve now covered the essential basics.
Next, we’ll dive into **Networking Fundamentals** in Module 2.
