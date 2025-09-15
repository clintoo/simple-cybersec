# Lab 01: Wireshark Basics

## Overview

This lab introduces network protocol analysis using Wireshark. You'll learn to capture, filter, and analyze network traffic to understand how protocols work and identify potential security issues.

## Objectives

By the end of this lab, you will be able to:
- Capture network traffic using Wireshark
- Apply display filters to analyze specific traffic
- Understand common network protocols
- Identify suspicious network activity
- Export and analyze packet captures

## Prerequisites

- Completed [Lab 00: Environment Setup](lab-00-setup.md)
- Wireshark installed on Kali Linux
- Basic understanding of networking concepts
- Target machines for traffic generation

## Part 1: Wireshark Interface Overview

### Starting Wireshark

1. **Launch Wireshark**
   ```bash
   # Command line
   sudo wireshark
   
   # Or from desktop menu
   # Applications > Sniffing & Spoofing > Wireshark
   ```

2. **Interface Selection**
   - Select appropriate network interface
   - Usually `eth0` for wired, `wlan0` for wireless
   - Look for interfaces with traffic activity

### Understanding the Interface

1. **Main Window Components**
   - **Packet List**: Summary of captured packets
   - **Packet Details**: Protocol layers of selected packet
   - **Packet Bytes**: Raw hexadecimal and ASCII data
   - **Display Filter**: Current filter expression
   - **Status Bar**: Capture statistics and information

2. **Toolbar Functions**
   - Start/Stop capture
   - Save captures
   - Apply display filters
   - Navigate packets
   - Colorize traffic

## Part 2: Basic Packet Capture

### Simple Traffic Capture

1. **Start Capture**
   ```bash
   # Select interface and start capture
   # Generate some traffic:
   ping google.com
   curl http://example.com
   ssh user@target-machine
   ```

2. **Stop and Analyze**
   - Stop capture after generating traffic
   - Observe different protocol types
   - Notice packet timing and sizes

### Understanding Packet Information

1. **Packet List Columns**
   - **No.**: Packet sequence number
   - **Time**: Timestamp relative to capture start
   - **Source**: Source IP address
   - **Destination**: Destination IP address
   - **Protocol**: Highest layer protocol
   - **Length**: Packet size in bytes
   - **Info**: Protocol-specific information

2. **Protocol Layers**
   - Physical Layer (not shown in Wireshark)
   - Data Link Layer (Ethernet)
   - Network Layer (IP)
   - Transport Layer (TCP/UDP)
   - Application Layer (HTTP/DNS/etc.)

## Part 3: Display Filters

### Basic Filter Syntax

1. **Protocol Filters**
   ```
   # Filter by protocol
   http
   tcp
   udp
   dns
   icmp
   arp
   ```

2. **IP Address Filters**
   ```
   # Filter by IP address
   ip.addr == 192.168.1.100
   ip.src == 192.168.1.100
   ip.dst == 192.168.1.100
   
   # Network range
   ip.addr == 192.168.1.0/24
   ```

3. **Port Filters**
   ```
   # Filter by port
   tcp.port == 80
   udp.port == 53
   tcp.srcport == 443
   tcp.dstport == 22
   ```

### Advanced Filtering

1. **Logical Operators**
   ```
   # AND operator
   tcp and port 80
   ip.addr == 192.168.1.100 and http
   
   # OR operator
   tcp.port == 80 or tcp.port == 443
   
   # NOT operator
   not arp
   not tcp.port == 22
   ```

2. **Comparison Operators**
   ```
   # Greater than
   frame.len > 1000
   tcp.window_size > 8000
   
   # Contains
   http.request.uri contains "login"
   tcp contains "password"
   ```

### Common Filter Examples

```
# Web traffic
http or https or tcp.port == 80 or tcp.port == 443

# Email traffic
tcp.port == 25 or tcp.port == 110 or tcp.port == 143 or tcp.port == 993

# DNS queries
dns and (dns.flags.response == 0)

# Failed TCP connections
tcp.flags.reset == 1

# Large packets
frame.len > 1500
```

## Part 4: Protocol Analysis

### HTTP Traffic Analysis

1. **Capture HTTP Traffic**
   ```bash
   # Generate HTTP traffic
   curl -v http://httpbin.org/get
   wget http://example.com
   ```

2. **Analyze HTTP Packets**
   - Apply filter: `http`
   - Examine HTTP request methods
   - Look at headers and user agents
   - Observe response codes

3. **Follow HTTP Stream**
   - Right-click on HTTP packet
   - Select "Follow > HTTP Stream"
   - View complete conversation
   - Notice unencrypted data

### DNS Traffic Analysis

1. **Capture DNS Queries**
   ```bash
   # Generate DNS traffic
   nslookup google.com
   dig example.com
   host -t mx gmail.com
   ```

2. **Analyze DNS Packets**
   - Apply filter: `dns`
   - Examine query types (A, AAAA, MX, etc.)
   - Look at response codes
   - Identify authoritative servers

### TCP Connection Analysis

1. **TCP Three-Way Handshake**
   ```bash
   # Generate TCP connection
   telnet towel.blinkenlights.nl 23
   ```

2. **Analyze TCP Flow**
   - Filter: `tcp.flags.syn == 1`
   - Observe SYN, SYN-ACK, ACK sequence
   - Note sequence and acknowledgment numbers
   - Watch window scaling and options

## Part 5: Security-Focused Analysis

### Identifying Suspicious Activity

1. **Port Scanning Detection**
   ```bash
   # Generate port scan from another VM
   nmap -sS target-ip
   ```
   
   - Filter: `tcp.flags.syn == 1 and tcp.flags.ack == 0`
   - Look for many connections to different ports
   - Notice lack of established connections

2. **ARP Spoofing Detection**
   - Filter: `arp`
   - Look for duplicate IP announcements
   - Check for unusual ARP patterns
   - Verify MAC address consistency

3. **Unusual DNS Activity**
   - Filter: `dns and not dns.flags.response == 1`
   - Look for unusual domain queries
   - Check for DNS over unusual ports
   - Identify potential DNS tunneling

### Traffic Patterns

1. **Baseline Normal Traffic**
   - Capture during normal operations
   - Note typical protocols and volumes
   - Identify regular communication patterns
   - Document normal service behaviors

2. **Anomaly Detection**
   - Compare against baseline
   - Look for unusual protocols
   - Identify unexpected destinations
   - Monitor traffic volume spikes

## Part 6: Advanced Features

### Statistics and Analysis

1. **Protocol Hierarchy**
   - Statistics > Protocol Hierarchy
   - View protocol distribution
   - Identify dominant traffic types

2. **Conversations**
   - Statistics > Conversations
   - View top talkers
   - Identify traffic patterns
   - Sort by packets, bytes, duration

3. **Endpoints**
   - Statistics > Endpoints
   - View traffic by IP address
   - Identify heavy users
   - Check geographical distribution

### Exporting Data

1. **Save Captures**
   ```bash
   # Save complete capture
   File > Save As > capture.pcapng
   
   # Save filtered packets
   File > Export Specified Packets
   ```

2. **Export Objects**
   - File > Export Objects > HTTP
   - Extract files from network traffic
   - Useful for malware analysis
   - Examine downloaded content

## Part 7: Hands-On Exercises

### Exercise 1: HTTP Analysis

1. **Generate HTTP Traffic**
   - Visit several websites using HTTP
   - Include form submissions
   - Perform file downloads

2. **Analysis Tasks**
   - Identify all HTTP methods used
   - Extract user-agent strings
   - Find any credentials transmitted
   - Locate cookies and session tokens

### Exercise 2: Network Reconnaissance

1. **Capture Reconnaissance Traffic**
   ```bash
   # From attacking machine
   nmap -sn 192.168.56.0/24
   nmap -sS -p 1-1000 target-ip
   ```

2. **Detection Analysis**
   - Identify scanning patterns
   - Count connection attempts
   - Note timing between packets
   - Document target responses

### Exercise 3: Protocol Investigation

1. **Multi-Protocol Capture**
   - Generate traffic using various protocols
   - Include SSH, FTP, SMTP, etc.
   - Capture both encrypted and unencrypted

2. **Comparative Analysis**
   - Compare encrypted vs. unencrypted protocols
   - Identify protocol-specific patterns
   - Note security implications

## Part 8: Troubleshooting and Tips

### Common Issues

1. **Permission Problems**
   ```bash
   # Add user to wireshark group
   sudo usermod -a -G wireshark $USER
   
   # Or run with sudo
   sudo wireshark
   ```

2. **Interface Selection**
   - Choose correct network interface
   - Verify interface is up and active
   - Check for promiscuous mode support

3. **Capture Problems**
   - Ensure sufficient disk space
   - Monitor memory usage during capture
   - Use capture filters for large volumes

### Performance Tips

1. **Capture Filters**
   ```bash
   # Reduce capture size with filters
   # Capture only TCP traffic to port 80
   tcp port 80
   
   # Capture specific host traffic
   host 192.168.1.100
   ```

2. **Ring Buffer**
   - Use ring buffer for continuous capture
   - Set size and file rotation limits
   - Prevent disk space exhaustion

## Lab Report Requirements

### Analysis Documentation

1. **Traffic Capture Summary**
   - Types of protocols observed
   - Volume and timing statistics
   - Notable patterns or anomalies

2. **Security Observations**
   - Unencrypted sensitive data
   - Potential security vulnerabilities
   - Suspicious network behavior

3. **Filter Examples**
   - Useful filters developed
   - Analysis techniques learned
   - Troubleshooting experiences

### Deliverables

- Packet capture files (.pcapng)
- Screenshots of key findings
- Written analysis report
- Filter reference sheet

## Next Steps

Continue with:
- **[Lab 02: Nmap and Scanning](lab-02-nmap-and-scanning.md)** - Network reconnaissance
- **[Lab 03: Burp Suite Introduction](lab-03-burp-intro.md)** - Web application security

## Additional Resources

- Wireshark User's Guide
- Network protocol RFCs
- Wireshark Display Filter Reference
- Malware-Traffic-Analysis.net
- Wireshark sample captures