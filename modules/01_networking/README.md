# Module 1: Networking

Networking is the backbone of cybersecurity.  
Before you can defend a system, you need to understand how computers communicate.

---

## 1. What is Networking?

Networking is the practice of connecting computers and devices so they can share information.  
The Internet itself is the largest network in existence.

---

## 2. Key Concepts

- **IP Address** → A unique identifier for a device on a network.  
  - Example: `192.168.1.1` (IPv4), `2001:db8::1` (IPv6).  
- **MAC Address** → Hardware identifier assigned to a network card.  
- **Ports** → Virtual “doors” where services communicate.  
  - Example: Port 80 → HTTP, Port 22 → SSH.  
- **Protocols** → Rules that govern communication.  
  - Example: TCP, UDP, HTTP, HTTPS, DNS.  

---

## 3. The OSI Model

The **OSI (Open Systems Interconnection)** model describes how data moves through a network.  
It has **7 layers**, each with a specific role:

1. **Physical** – Cables, Wi-Fi, hardware.  
2. **Data Link** – MAC addresses, switches.  
3. **Network** – IP addresses, routers.  
4. **Transport** – TCP/UDP, reliable delivery.  
5. **Session** – Connections between apps.  
6. **Presentation** – Data format (encryption, compression).  
7. **Application** – User-facing apps (HTTP, DNS, SMTP).  

👉 In practice, we often simplify this into the **TCP/IP model (4 layers)**:  
- Application → (HTTP, DNS, etc.)  
- Transport → (TCP/UDP)  
- Internet → (IP addresses, routing)  
- Network Access → (Ethernet, Wi-Fi)  

---

## 4. Common Protocols

- **HTTP/HTTPS** → Web traffic.  
- **DNS** → Converts domain names into IP addresses.  
- **SSH** → Secure remote login.  
- **FTP/SFTP** → File transfer.  
- **SMTP/IMAP/POP3** → Email communication.  

---

## 5. Networking Commands (Linux/macOS/Windows)

Try these commands to explore your network:

```bash
# Check your IP address
ip addr         # Linux
ifconfig        # macOS (or older Linux)
ipconfig        # Windows

# Test connectivity
ping google.com

# Trace the route packets take
traceroute google.com   # Linux/macOS
tracert google.com      # Windows

# View open network connections
netstat -tuln           # Linux/macOS
netstat -an             # Windows
````

---

## 6. Tools to Know

* **Wireshark** – Packet capture and analysis.
* **tcpdump** – Command-line packet capture.
* **Nmap** – Network scanner (find hosts, open ports, services).

---

## 7. Setup Instructions

### Install Wireshark

* **Linux (Debian/Ubuntu):**

  ```bash
  sudo apt update
  sudo apt install wireshark
  ```
* **macOS:**

  ```bash
  brew install --cask wireshark
  ```
* **Windows:**

  * Download from [wireshark.org](https://www.wireshark.org/download.html).

### Install Nmap

* **Linux:**

  ```bash
  sudo apt install nmap
  ```
* **macOS:**

  ```bash
  brew install nmap
  ```
* **Windows:**

  * Download installer from [nmap.org/download.html](https://nmap.org/download.html).

---

## 8. Try It Yourself (Mini Labs)

### 🔍 Scan your own system with Nmap

```bash
# Scan localhost for open ports
nmap 127.0.0.1
```

### 📡 Capture packets with tcpdump

```bash
# Capture first 10 packets on interface eth0
sudo tcpdump -i eth0 -c 10
```

---

✅ Next up: **Operating Systems (Module 2)** — learning how Windows, Linux, and macOS differ in cybersecurity.

