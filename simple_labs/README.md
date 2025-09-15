# Labs — Detailed Hands-on Guides

This document contains **thorough, step-by-step lab guides** you can follow inside your safe practice environment. Each lab is written for beginners and explains *why* you do each step, not just *how*.

> **Safety reminder:** Run these labs only inside your lab VMs or other authorized environments. Never run scans, packet captures, or interception tools against networks or systems you do not own or have explicit permission to test.

---

## Lab 00 — Lab Setup: VirtualBox, Ubuntu VM, Snapshots, and Safe Networking

**Goal:** Build a safe, revertible lab environment using VirtualBox and a Linux VM (Ubuntu). You’ll learn how to configure networking (NAT + Host-only), take snapshots, and install common tooling.

### A. Download required software

1. **VirtualBox**: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) — pick the installer for your OS.
2. **Ubuntu Desktop or Server ISO** (e.g., Ubuntu 24.04 LTS): [https://ubuntu.com/download](https://ubuntu.com/download)
3. Optional: **VirtualBox Extension Pack** (matches your VirtualBox version) — enables USB 2/3 and other features.

### B. Create the VM (step-by-step)

1. Open VirtualBox → **New**.
2. Name: `lab-ubuntu`. Type: `Linux`. Version: `Ubuntu (64-bit)`.
3. Memory: 2048–4096 MB (2–4 GB recommended).
4. Create a virtual hard disk: VDI, dynamically allocated, 20 GB.
5. After creation, select the VM → **Settings → Storage**, click the optical drive, choose the downloaded Ubuntu ISO.

### C. Configure networking for safety

1. **Adapter 1**: Attached to `NAT` — allows the VM to reach the internet for package installs but prevents inbound access from your LAN.
2. **Adapter 2**: Enable and attach to `Host-only Adapter` (VirtualBox Host-Only Network) — this creates a private network between your host and VMs for safe scanning and multi-VM labs.

> Why this combo? NAT lets you fetch updates; Host-only lets you safely scan the lab network without touching other devices on your physical LAN.

### D. Install Ubuntu

1. Start the VM and follow the Ubuntu installer steps (language, keyboard, install updates).
2. Create your primary user (e.g., `student`).
3. Finish installation and reboot the VM.

### E. Install Guest Additions (improves integration)

1. In VirtualBox menu when the VM is running → Devices → Insert Guest Additions CD image.
2. Inside the VM, mount and run the installer (`sudo sh VBoxLinuxAdditions.run`) or follow distro instructions.

### F. Update the system and install basic tools

Open a terminal in the VM and run:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y net-tools iproute2 vim curl wget git build-essential
```

### G. Create a snapshot (clean state)

1. Power off the VM cleanly (shutdown from the OS).
2. In VirtualBox Manager: Right-click the VM → Snapshots → Take Snapshot. Name it `clean-install-YYYYMMDD`.

### H. Create a second VM (optional) for target host

If you want to scan a second machine, create another VM named `lab-target` and configure it with Host-only networking only. This way your attacker VM and target VM share the host-only network.

### I. Test connectivity

From the main `lab-ubuntu` VM, find your IP addresses:

```bash
ip addr show
```

* Host-only network usually shows an IP like `192.168.56.x` (VirtualBox default).
* Test ping to another VM or to the host:

```bash
ping -c 3 192.168.56.1   # host or other VM IP
```

### Troubleshooting

* If Host-only adapter isn’t present on your host, install/enable it via VirtualBox → File → Host Network Manager.
* If VM cannot reach the internet, check Adapter 1 is NAT and the host allows VirtualBox outgoing connections.

---

## Lab 01 — Wireshark & tcpdump: Capture and Inspect an HTTP Request

**Goal:** Capture network traffic for a single HTTP GET request to a local test server, open it in Wireshark, and inspect the request and response headers.

### A. Install Wireshark and tcpdump

```bash
sudo apt update
sudo apt install -y wireshark tcpdump
```

* During Wireshark install on Ubuntu, you may be asked whether non-root users should be able to capture packets. For safety, you can say **No** and run Wireshark with `sudo` or add your user to the `wireshark` group after installation.

### B. Start a simple HTTP server (local target)

We’ll use Python’s built-in HTTP server so traffic is predictable and local.

```bash
# In a new terminal, create a test folder
mkdir ~/lab-http && cd ~/lab-http
echo "<html><body><h1>Lab HTTP Server</h1></body></html>" > index.html
# Start a Python 3 HTTP server on port 8000
python3 -m http.server 8000
```

This serves `index.html` at `http://<vm-ip>:8000/`.

### C. Capture packets with tcpdump (CLI)

Find your interface (e.g., `eth0`, `enp0s3`, `enp0s8`):

```bash
ip addr show
```

Start a capture of HTTP traffic and save it to a file:

```bash
sudo tcpdump -i enp0s8 -w /tmp/http_lab.pcap port 8000
```

* `-i enp0s8` → replace with your host-only interface name.
* `-w /tmp/http_lab.pcap` → write capture to file so Wireshark can open it.

Now request the page from your host or another VM:

```bash
curl http://192.168.56.102:8000/
```

(Use the actual IP of the VM running the Python server.)

Stop tcpdump (Ctrl+C) after the request completes.

### D. Open the capture in Wireshark

```bash
wireshark /tmp/http_lab.pcap &
```

* In Wireshark apply a display filter: `http` or `tcp.port == 8000`.
* Find the HTTP GET packet. Right-click → **Follow** → **TCP Stream**. You’ll see the entire HTTP conversation: the GET request and the server response (HTML).

### E. Inspect headers & payload

* In the HTTP request, note `Host`, `User-Agent`, and `Accept` headers.
* In the response, note `HTTP/1.0 200 OK` (or `HTTP/1.1`) and `Content-Type: text/html`.

### F. Save a sanitized capture

Before sharing a pcap, sanitize (remove local IPs or sensitive data). For now, keep captures local.

### G. Advanced filters & tcpdump quick tips

* Capture only traffic to/from a host: `sudo tcpdump -i enp0s8 host 192.168.56.102 -w file.pcap`
* Capture only GET requests (Wireshark filter): `http.request.method == "GET"`

### Expected learning outcomes

* You can capture packets, open them in Wireshark, find HTTP requests, and read headers.
* You understand the relationship between an application request (browser/curl) and packets on the wire.

---

## Lab 02 — Nmap: Scanning & Interpreting Results

**Goal:** Learn how to scan hosts on your host-only network, interpret open ports, service versions, and basic OS detection hints. Practice safe scanning techniques.

> **Important:** Only scan hosts within your lab network.

### A. Install Nmap

```bash
sudo apt update
sudo apt install -y nmap
```

### B. Discover live hosts on your host-only network

Determine your subnet (replace with your VM’s network):

```bash
ip -4 addr show enp0s8
# Example output might show 192.168.56.101/24
```

Scan for live hosts (ping scan):

```bash
nmap -sn 192.168.56.0/24
```

* `-sn` (ping scan) reports which hosts are up without port scanning.

### C. Scan a single host for open ports

```bash
nmap 192.168.56.102
```

Interpretation example (sample output):

```
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
139/tcp  closed netbios-ssn
```

This tells you which services may be running. `open` means a process is listening on that port.

### D. Service and version detection

```bash
sudo nmap -sV 192.168.56.102
```

This attempts to connect to services and identify versions, e.g., `OpenSSH 8.2p1`, `nginx 1.18`.

### E. TCP SYN scan and timing

```bash
sudo nmap -sS -T4 -p 1-1000 192.168.56.102
```

* `-sS` → SYN stealth scan (faster, may require root).
* `-T4` → timing template (faster aggressive timing for LAN).
* `-p 1-1000` → scan ports 1–1000.

### F. Aggressive scan (caution)

```bash
sudo nmap -A 192.168.56.102
```

`-A` runs OS detection, version detection, script scanning, and traceroute. Useful in labs but noisy.

### G. Use Nmap scripts (NSE)

Nmap has many scripts for specific checks. Example: `http-title` shows page title.

```bash
nmap --script=http-title -p80 192.168.56.102
```

### H. Reading results & next steps

* **Open ports** → try connecting with the service (ssh, http).
* **Service versions** → search for known vulnerabilities for that version (only in lab).
* **OS hints** → Nmap may say `Linux 3.x` or `Windows`, which helps choose tools.

### I. Save scan to file

```bash
nmap -oN nmap-basic.txt 192.168.56.102
```

This saves readable output for later notes.

### J. Troubleshooting

* If all ports show `filtered`, a firewall is blocking probes — try scanning from another VM on the same host-only network.
* If Nmap requires root for some scans, use `sudo`.

---

## Lab 03 — Burp Suite Community: Intercepting & Modifying Web Requests

**Goal:** Configure Burp to intercept a browser’s traffic to a local web app, modify a request parameter, and observe the result. Learn about Repeater and basic workflow.

> **Warning:** Only intercept traffic for apps you control. Do not intercept third‑party traffic without permission.

### A. Install Burp Suite Community

1. Download the JAR or installer from PortSwigger: [https://portswigger.net/burp/releases](https://portswigger.net/burp/releases)
2. Run the jar: `java -jar burpsuite_community_vX.Y.Z.jar` (ensure Java is installed: `sudo apt install default-jre`).

### B. Configure Burp proxy and browser

1. In Burp → **Proxy → Options** note the proxy listener (default `127.0.0.1:8080`).
2. Configure your browser to use `127.0.0.1:8080` as an HTTP proxy (use a separate browser profile for lab work).
3. In Burp → **Proxy → Intercept** turn Intercept **on**.

### C. Install Burp CA certificate (for HTTPS)

1. In Burp, **Proxy → Intercept → Open Browser** or visit `http://burp` in your lab browser.
2. Click the link to download the CA cert.
3. In the browser, import the certificate as a trusted CA (in Firefox: Preferences → Privacy & Security → Certificates → View Certificates → Import).

### D. Start a local vulnerable test app (example)

You can use a small intentionally vulnerable app like `DVWA` or a simple Flask app that echoes a parameter. For simplicity, we’ll use a tiny Flask app:

```python
# save as app.py
from flask import Flask, request
app = Flask(__name__)

@app.route('/greet')
def greet():
    name = request.args.get('name', 'stranger')
    return f"<html><body><h1>Hello {name}</h1></body></html>"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Run it:

```bash
sudo apt install -y python3-venv
python3 -m venv venv && source venv/bin/activate
pip install flask
python app.py
```

Access `http://192.168.56.102:5000/greet?name=korvo` in your lab browser (through Burp proxy).

### E. Intercept and modify a request

1. With Intercept **On**, load the URL in your browser. Burp will capture the request.
2. In Burp's Intercept tab, change the parameter `name=korvo` to `name=<script>alert(1)</script>` (or `name=attacker` for safe testing), then forward the request.
3. Observe the server response in the browser — the app may render the changed name directly.

### F. Use Repeater for repeated tests

1. Right-click the intercepted request → **Send to Repeater**.
2. In Repeater, you can modify the request repeatedly and resend to observe responses without navigating the browser.

### G. Use Decoder / Comparer / Logger

* **Decoder** helps decode/encode payloads (URL-encoding, Base64).
* **Logger** records requests; **Comparer** compares two responses.

### H. Save your project and notes

Burp Community can save workspaces — save the project if you want to keep intercepted samples (be careful with sensitive data).

### I. Troubleshooting

* If browser shows certificate warnings, ensure the Burp CA is properly installed and trusted in the browser profile.
* If no traffic appears in Burp, confirm browser proxy settings and that the browser is using the lab profile through Burp.

---

## Lab CTF Overview — Getting Started with OverTheWire (Bandit)

**Goal:** Start with Bandit on OverTheWire to practice Linux, SSH, file permissions, and basic scripting in a gamified setting.

### Why OverTheWire Bandit?

* Designed for absolute beginners to learn command-line basics and basic security concepts.
* Levels are stepwise; each level teaches a specific skill.

### A. How to connect (Bandit example)

1. Open a terminal on your local machine (or lab VM).
2. Connect via SSH using the credentials given on the Bandit website. For Level 0:

```bash
ssh bandit.labs.overthewire.org -p 2220 -l bandit0
# It will prompt for a password (listed on the website for the starting level)
```

3. Once connected, read the README on the server (each level has instructions):

```bash
ls
cat readme
```

### B. Tips for solving levels

* Carefully read the instructions and any hint text in files.
* Use basic Linux commands: `ls`, `cat`, `file`, `less`, `grep`, `find`, `strings`, `chmod`, `ssh`, `scp`, `tar`.
* If you get stuck, research specific commands (e.g., "how to find files by size Linux"), but avoid spoilers if you want the learning experience.

### C. Example tiny walkthrough (Bandit Level 1 → Level 2)

* Level 1 instruction might say: "The password for the next level is stored in a file in the home directory."
  Commands to try:

```bash
# List files
ls -la
# Look for hidden files
ls -la | less
# Show file contents
cat filename
```

You will find the password and then SSH to the next level with `bandit1` user.

### D. Recording progress & notes

* Keep a notebook (or digital notes) with commands that worked — these are references for future levels.
* Don’t rush: the point is to learn commands and thinking patterns.

### E. Other practice platforms

* **picoCTF** — beginner-friendly, especially for students.
* **TryHackMe** / **HackTheBox** — interactive rooms and guided tracks (some free content).

---

## Lab verification checklist (how to know you succeeded)

* [ ] You created a VM and took a snapshot named `clean-install-YYYYMMDD`.
* [ ] You started a Python HTTP server and captured its HTTP GET in Wireshark/tcpdump.
* [ ] You discovered at least one live host and scanned its open ports with Nmap.
* [ ] You configured Burp, intercepted a local web request, and used Repeater to resend modified requests.
* [ ] You connected to OverTheWire Bandit and completed at least Level 0 → Level 1.

---

## Notes, tips & best practices

* Keep a lab notebook: record IPs, commands, and findings. This habit is invaluable.
* Use separate browser profiles for lab work and personal browsing (so you don't accidentally intercept personal traffic).
* Revert to snapshots often — break things freely in labs and restore clean states.
* Read `man` pages: `man nmap`, `man tcpdump`, `man ssh` — they contain authoritative options and examples.

