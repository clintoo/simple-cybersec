# Module 2: Operating Systems

To secure computers, you first need to understand how operating systems (OS) work.  
An **operating system** is the software that manages hardware and provides a platform for applications.

---

## 1. What is an Operating System?

- Acts as the "middleman" between hardware and software.  
- Manages **files, processes, memory, and users**.  
- Examples: **Windows, Linux, macOS**.

---

## 2. Key Components of an OS

- **Kernel** → The "core" that talks to hardware.  
- **Processes** → Running programs.  
- **File System** → How data is stored and organized.  
- **Users & Permissions** → Who can access what.  
- **Networking Stack** → Enables communication over networks.  

---

## 3. Common OS in Cybersecurity

### Windows
- Widely used in businesses → a common target for attackers.  
- Uses **PowerShell** for automation and security tasks.  
- User management: `net user`, `net localgroup`.  

### Linux
- Popular in servers, hacking tools, and security labs.  
- Open-source and highly customizable.  
- Terminal commands for system exploration:  
  ```bash
  whoami        # Show current user
  pwd           # Show current directory
  ls -la        # List files with permissions
  ps aux        # Show running processes

### macOS

* Unix-based (like Linux) but mainly used by individuals.
* Strong built-in security features, but less used in enterprise servers.

---

## 4. Users, Groups, and Permissions

Security starts with **controlling access**:

* **Windows**: Users and Groups (Local Users and Groups Manager).
* **Linux/macOS**: Users and Groups defined in `/etc/passwd` and `/etc/group`.
* File permissions in Linux/macOS:

  ```bash
  ls -l
  # Example output:
  -rw-r--r--  1 user user   1234 Sep 10  file.txt
  ```

  * `r` = read, `w` = write, `x` = execute.
  * Permissions are for **owner**, **group**, and **others**.

Change file permissions:

```bash
chmod 755 script.sh
```

---

## 5. OS Security Features

* **Windows Defender / Antivirus** – Detects malware.
* **Linux iptables/ufw** – Firewalls for network filtering.
* **macOS Gatekeeper** – Blocks unsigned apps.
* **User Account Control (UAC)** – Prevents unauthorized changes.
* **Patch Management** – Keep systems updated.

---

## 6. Mini Labs (Try It Yourself)

### 🔐 Linux User Exploration

```bash
# Create a new user
sudo adduser testuser

# Switch to that user
su - testuser

# Check permissions
ls -la
```

### 🖥️ Windows User Commands

Open **Command Prompt (cmd)** or **PowerShell**:

```powershell
# List all users
net user

# Create a new user
net user testuser MyPassword123! /add

# Add user to Administrators group
net localgroup administrators testuser /add
```

---

## 7. Tools to Know

* **PowerShell** (Windows) → automation, scripting, system management.
* **Bash** (Linux/macOS) → command-line shell.
* **Process Explorer** (Windows) → advanced task manager.
* **htop** (Linux) → interactive process viewer.

---

## 8. Why OS Knowledge Matters in Cybersecurity

* Attackers often exploit **misconfigurations** in OS.
* Defenders must know how to **check logs, monitor processes, and set permissions**.
* Many security tools are OS-specific (e.g., PowerShell Empire for Windows, Metasploit on Linux).
