# Module 5: Tools & Labs

**Objective:**
This module introduces the practical toolset every beginner in cybersecurity should know, explains how to set up a safe practice lab, and walks through basic example commands and workflows. Think of this module as the bridge between theory and hands‑on practice.

> Important: Always practice **only** on systems you own or are authorized to test (your lab VMs, intentionally vulnerable VMs, or platforms like OverTheWire). Never scan or attack other people's systems without permission.

---

## What you'll learn in this module

* How to create a safe practice lab (VMs, snapshots, network choices).
* The core tools (what they are, when to use them).
* Install & quick-start commands for each tool (Linux / macOS / Windows notes).
* Example workflows: capture packets, scan a target, intercept web traffic, and basic local service tests.
* A roadmap for practice (OverTheWire + lab files).

---

## 1) Setting up a safe lab — step-by-step

You need a safe, isolated place to run scans, intercept traffic, and break things without harming others. Virtual machines (VMs) are ideal.

### A. Choose your hypervisor

* **VirtualBox** (free, cross-platform) — good for beginners.
* **VMware Workstation Player** (free for non-commercial use) — solid alternative.
* **WSL2 (Windows Subsystem for Linux)** — handy for Linux CLI tools on Windows, but not a full isolated VM.

### B. Create a base VM (example: Ubuntu 24.04 LTS or Kali Linux)

1. Download the ISO for your chosen Linux distribution (Ubuntu or Kali).
2. Create a new VM in VirtualBox:

   * Name: `lab-ubuntu`
   * Memory: 2048 MB (2 GB) or more if available
   * CPUs: 2 (if your host has >2 cores)
   * Disk: 20 GB (dynamically allocated is fine)
3. Mount the ISO and install the OS following prompts.

### C. Install Guest Additions / Tools

* VirtualBox: Install Guest Additions (improves clipboard, shared folders, screen resolution).
* Update packages:

```bash
sudo apt update && sudo apt upgrade -y
```

### D. Snapshot your clean state

* After the VM is updated and configured, **take a snapshot**. If you break the system, revert to this snapshot.

### E. Networking choices (very important for safety)

* **NAT (Network Address Translation)** — VM can access the internet but is not directly reachable from your LAN. Good for normal browsing/updates.
* **Host‑only Adapter** — VM can talk to your host and other VMs on a private network (useful for multi‑VM labs).
* **Bridged Adapter** — VM appears on your LAN as a separate device. Use with caution: scanning LAN devices with a bridged adapter may touch other people's machines.

**Recommendation for beginners:** Use **NAT + Host‑only**. Give your VM internet access to download tools (via NAT) and use host\_only for safe scans between VMs.

---

## 2) Core tools (what they do + install + quick examples)

Below are the most useful beginner tools. For each: short description, install steps (Ubuntu/macOS/Windows), and a tiny example with explanation.

> Note: Example commands assume you’re practicing on your lab VM or localhost (`127.0.0.1` / another VM IP).

### Wireshark — Packet capture and GUI analysis

**What it does:** Captures network traffic and lets you inspect packets visually (headers, payloads, protocols). Great for learning how protocols look on the wire.

**Install:**

* **Ubuntu/Debian:** `sudo apt install wireshark` (you may be asked whether non-root users can capture packets; choose carefully).
* **macOS:** `brew install --cask wireshark` (Homebrew required).
* **Windows:** Download the installer from [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html) and run it.

**Quick example:**

1. Start Wireshark and select your interface (e.g., `eth0`, `enp0s3`, or `wlan0`).
2. Click **Start** to capture packets.
3. In a browser, visit `http://httpbin.org/get`.
4. Back in Wireshark, apply a display filter: `http` or `ip.addr == 93.184.216.34` (replace with site IP).
5. Right-click a packet and **Follow → TCP Stream** to see the full HTTP conversation.

**Why this matters:** Seeing raw packets helps you understand how requests and responses are structured. It’s also how analysts detect suspicious traffic.

**Safety tip:** Captured packets may contain sensitive info — don’t share captures publicly without sanitizing.

---

### tcpdump — Command-line packet capture

**What it does:** CLI alternative to Wireshark for capturing packets. Can write pcap files for later analysis in Wireshark.

**Install:** `sudo apt install tcpdump` (Ubuntu).
**macOS:** `brew install tcpdump` (usually preinstalled).
**Windows:** Use WSL or WinPcap/Npcap with tools that support it.

**Example:**

```bash
# Capture 100 packets on the interface eth0 and save to a file
sudo tcpdump -i eth0 -c 100 -w /tmp/capture.pcap

# Capture only HTTP traffic (port 80) and show packet contents
sudo tcpdump -i eth0 -c 20 -A 'port 80'
```

* `-i eth0` → interface
* `-c 100` → capture 100 packets then stop
* `-w file.pcap` → write raw pcap file
* `-A` → print packets in ASCII (readable text where available)

You can open `/tmp/capture.pcap` later with Wireshark:

```bash
wireshark /tmp/capture.pcap &
```

---

### Nmap — Network discovery & port scanning

**What it does:** Finds hosts on a network and discovers open ports and running services.

**Install:**

* **Ubuntu:** `sudo apt install nmap`
* **macOS:** `brew install nmap`
* **Windows:** Download from [https://nmap.org/download.html](https://nmap.org/download.html) or use `choco install nmap` if you have Chocolatey.

**Quick examples:**

```bash
# Basic discover of open ports on a target IP
nmap 192.168.56.101

# Service/version detection on common ports (requires root for some scan types)
sudo nmap -sV 192.168.56.101

# Aggressive scan (service detection + script scanning + OS detection)
sudo nmap -A 192.168.56.101

# Scan a range of ports
nmap -p 1-1000 192.168.56.101
```

**What to look for:** open ports (e.g., 22 → SSH), service versions (e.g., `OpenSSH 8.2p1`) and OS hints. These clues tell you how to interact with a host.

**Ethics:** Scanning public IPs you don’t own can be considered intrusive or illegal. Stick to your lab.

---

### Netcat (nc) — Swiss Army knife for TCP/UDP

**What it does:** Create simple TCP/UDP connections, listen on ports, transfer files, and test services.

**Install:** Usually preinstalled. On Ubuntu: `sudo apt install netcat-openbsd`.

**Examples:**

```bash
# Listen on port 4444 and show incoming data
nc -lvp 4444

# From another machine, connect to the listener
nc 192.168.56.101 4444

# Banner grabbing (connect to HTTP port and send a GET)
printf 'GET / HTTP/1.1\r\nHost: example.com\r\n\r\n' | nc example.com 80
```

**Use-cases:** Debugging services, quick file transfers between VMs, and basic port testing.

---

### curl — Interact with web servers from CLI

**What it does:** Download web pages, send HTTP requests, test APIs.

**Install:** `sudo apt install curl` (Ubuntu).
**macOS:** preinstalled or via Homebrew.
**Windows:** available via PowerShell (Invoke-WebRequest) or install curl.

**Examples:**

```bash
# Simple GET
curl -i http://httpbin.org/get

# Send custom headers and POST JSON
curl -X POST -H "Content-Type: application/json" -d '{"name":"korvo"}' https://httpbin.org/post
```

**Why curl:** Great for testing endpoints quickly and reproducing browser requests from the terminal.

---

### openssl — Inspect TLS certificates and encrypt/decrypt data

**What it does:** Manage keys, certificates, and debug TLS connections.

**Install:** `sudo apt install openssl` (Ubuntu; usually preinstalled).

**Examples:**

```bash
# Inspect an HTTPS server's certificate
openssl s_client -connect example.com:443 -servername example.com

# Generate a private key and self-signed cert (for local testing)
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes
```

**Tip:** Use `openssl s_client` to debug TLS handshake issues and view certificate chains.

---

### Burp Suite (Community) — Web application testing proxy

**What it does:** Acts as a proxy between your browser and web servers so you can intercept, view, and modify HTTP(s) traffic.

**Install & setup (high-level):**

1. Download Burp Suite Community from [https://portswigger.net/burp/releases](https://portswigger.net/burp/releases).
2. Run Burp (`java -jar burpsuite_community.jar`) or launch the installer on Windows.
3. Configure your browser to use proxy `127.0.0.1:8080` and import Burp's CA certificate into the browser so HTTPS sites load without certificate errors.

**Quick workflow:**

* Browse a local web app while Burp is intercepting requests.
* Modify parameters in intercepted requests (e.g., change `id=5` → `id=6`) and forward the modified request.
* Use **Repeater** to resend single requests with changes.

**Important:** Only intercept traffic you own or have permission to test.

---

### Metasploit Framework — Exploitation & post-exploitation (intro)

**What it does:** Large framework with exploit modules, payloads, and helper utilities. Useful in learning how exploits are organized and for testing in controlled labs.

**Install (Ubuntu example):**

```bash
# Simple install on Debian/Ubuntu-based systems (official packages vary by distro)
sudo apt update
sudo apt install metasploit-framework
```

**Example (safe, local use):**

```bash
# Start the console
msfconsole

# Search for modules (example)
search vsftpd

# Use a module (example only in lab)
use exploit/unix/ftp/vsftpd_234_backdoor
show options
set RHOST 192.168.56.102
set LHOST 192.168.56.1
# run
```

**Warning:** Metasploit is powerful. Only use it in isolated labs. Never against third-party systems without permission.

---

### Kali Linux — Pentesting distribution

**What it is:** A Linux distro preloaded with many security tools (Nmap, Metasploit, Burp, John, Hashcat, etc.). Good for learning, but you can also install individual tools on Ubuntu.

**Tip:** Using Kali is convenient, but it’s not required — you can learn the same tools on an Ubuntu VM with manual installs.

---

### Password/hashing tools: `sha256sum`, `md5sum`, John the Ripper, Hashcat

* **sha256sum/md5sum** — compute file hashes.
* **John the Ripper (john)** — password-cracking tool for learning password security (install: `sudo apt install john`).
* **Hashcat** — GPU-accelerated password recovery (more advanced; requires GPU drivers).

**Example:** Hash a password and try to crack it with John (in a controlled lab):

```bash
# Create a sample password file
echo 'password123' > pass.txt
# Hash it (example using md5 — insecure in real life)
md5sum pass.txt > pass.hash

# john can attack various hash formats; learning how to use john requires reading its docs
```

**Ethical note:** Password cracking is a legitimate security exercise but must be done only on your own data or with explicit permission.

---

## 3) Common workflows — step-by-step examples

### A. Capture an HTTP request and inspect it

1. Start `tcpdump` on your VM and save to a file:

```bash
sudo tcpdump -i eth0 -w /tmp/http.pcap port 80
```

2. In a browser (on host or VM), visit a test local web server (e.g., `http://<vm-ip>/`).
3. Stop tcpdump (Ctrl+C).
4. Open the pcap in Wireshark: `wireshark /tmp/http.pcap` and filter `http`.
5. Follow the TCP Stream to view the request/response in plain text.

**Learning goal:** See how a browser request becomes packets on the wire.

---

### B. Scan a local target with Nmap

```bash
# Find live hosts on the host-only network (replace subnet with your lab network)
nmap -sn 192.168.56.0/24

# Scan a single host for common open ports and service versions
sudo nmap -sS -sV -p1-1024 192.168.56.102
```

**Learning goal:** Identify what services a host exposes and gather version info to know if they might be vulnerable.

---

### C. Intercept and modify a web request with Burp

1. Launch Burp and set Intercept → On.
2. Configure your browser proxy to `127.0.0.1:8080` and install Burp’s CA certificate.
3. Visit your local test app and submit a form.
4. Burp will pause the request — change a parameter and forward it.
5. Observe the app’s response.

**Learning goal:** Understand how inputs travel from browser → server and how attackers might tamper with them.

---

## 4) Labs roadmap and suggested progression

**Short-term practice (first 2 weeks):**

* Set up a VM and snapshot it (lab-00-setup).
* Do Bandit (OverTheWire) — first wargame for Linux basics.
* Capture a simple HTTP request with Wireshark (lab-01-wireshark-basics).
* Run `nmap` on a lab VM and interpret results (lab-02-nmap-and-scanning).

**Next (weeks 3–6):**

* Learn Burp basics and intercept local web apps (lab-03-burp-intro).
* Try basic password cracking exercises on your own hashes with John (ethical, local only).
* Play CTFs that focus on web and binary basics (e.g., OverTheWire bandit, picoCTF beginner tracks).

**Links and references:**

* OverTheWire Wargames (start with **Bandit**): [https://overthewire.org/wargames/](https://overthewire.org/wargames/)

**Labs folder plan:** `docs/labs/` will contain:

* `lab-00-setup.md` — VM setup and safe networking guide.
* `lab-01-wireshark-basics.md` — capture a GET and inspect headers.
* `lab-02-nmap-and-scanning.md` — scanning a lab VM and reading output.
* `lab-03-burp-intro.md` — intercepting and manipulating web requests.
* `lab-ctf-overview.md` — how to approach OverTheWire and CTF basics.

---

## 5) Troubleshooting & tips

* **Capture nothing in Wireshark?**

  * Check you selected the right interface.
  * Ensure you have permissions to capture (run as root if needed).

* **Nmap shows ports filtered/closed:**

  * A firewall may be blocking probes — try scanning from another VM on the same host-only network.

* **Burp shows SSL errors in the browser:**

  * Install Burp’s CA certificate into your browser (do this only for your lab browser profile).

* **Too many false positives in scans:**

  * Re-run scans more selectively (`-p` for port ranges) and cross-check results with other tools.

---

## 6) Safety, ethics, and legal reminders

* **Never** scan or attack systems you do not own or have written permission to test.
* Use host‑only or NAT networking when practicing on your home machine to avoid impacting others.
* If you find a real vulnerability by accident on a third‑party service, stop and follow responsible disclosure practices (see Module 7).

---

## 7) Recommended reading & resources

* Nmap Reference Guide (official)
* Wireshark User Guide
* Burp Suite docs (PortSwigger)
* OverTheWire wargames — Bandit (start here)
