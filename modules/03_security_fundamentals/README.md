# Module 3: Security Fundamentals

Now that you know the basics of networking and operating systems, let’s look at the core principles of **cybersecurity**.  
These are the foundations you’ll use everywhere, whether you’re defending a website, a server, or your own laptop.

---

## 1. The CIA Triad

The three pillars of security:

- **Confidentiality** → Keep information secret.  
  - Example: Encrypting messages so only the right person can read them.  

- **Integrity** → Ensure data is not changed.  
  - Example: Digital signatures, file checksums (e.g., `sha256sum`).  

- **Availability** → Keep systems running.  
  - Example: DDoS protection, redundant servers.  

---

## 2. Authentication, Authorization, Accounting (AAA)

- **Authentication** → Proving who you are (password, fingerprint, etc.).  
- **Authorization** → What you are allowed to do.  
- **Accounting** → Keeping track of actions (logs, audits).  

Example (Linux):
```bash
whoami         # Authentication: which user you are
ls -l /home    # Authorization: what files you can access
last           # Accounting: see login history
````

---

## 3. Encryption Basics

Encryption scrambles data so only the right key can unlock it.

* **Symmetric Encryption** → Same key to lock and unlock (e.g., AES).
* **Asymmetric Encryption** → Public key locks, private key unlocks (e.g., RSA).
* **Hashing** → One-way function to verify integrity (e.g., SHA-256).

Example: Hash a file (Linux/macOS):

```bash
sha256sum file.txt
```

---

## 4. Firewalls & Defense

* **Firewall** → Blocks or allows traffic based on rules.

  * Windows: *Windows Defender Firewall*.
  * Linux: `ufw` (Uncomplicated Firewall).

Example (Linux):

```bash
sudo ufw enable
sudo ufw allow 22/tcp   # Allow SSH
sudo ufw status
```

---

## 5. Security Policies

Organizations use rules to stay safe:

* **Password Policies** → Long, complex, rotated.
* **Access Controls** → Principle of least privilege (only give access when needed).
* **Patch Management** → Update software to close vulnerabilities.

---

## 6. Logs and Monitoring

* Logs are records of what happened.
* Examples:

  * **Windows** → Event Viewer.
  * **Linux** → `/var/log/auth.log`, `/var/log/syslog`.
* Monitoring tools:

  * **SIEM (Security Information and Event Management)** → Collect and analyze logs.

---

## 7. Mini Labs (Try It Yourself)

### 🔑 Create a Secure Password Hash (Linux)

```bash
# Generate a SHA-256 hash of a password
echo -n "MyPassword123!" | sha256sum
```

### 🔒 Enable and Test a Firewall

```bash
sudo ufw enable
sudo ufw deny 80/tcp    # Block HTTP
```

Then try to open a website → it should fail.

---

## 8. Tools to Know

* **Wireshark** → Packet capture and analysis.
* **OpenSSL** → Encryption and certificates.
* **Fail2Ban** → Blocks IPs after repeated login failures.
* **SIEM Tools** → Splunk, ELK stack, Graylog.

---

## Why This Matters

Security fundamentals are like the "grammar" of cybersecurity.
Once you master these, you’ll be able to understand attacks, defend systems, and recognize risks in real-world environments.

