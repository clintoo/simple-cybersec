# Lab 02: Nmap and Network Scanning

## Overview

This lab covers network reconnaissance using Nmap, one of the most important tools for network discovery and security auditing. You'll learn various scanning techniques, timing options, and how to interpret scan results.

## Objectives

By the end of this lab, you will be able to:
- Perform host discovery scans
- Conduct port scans using various techniques
- Identify services and operating systems
- Use Nmap Scripting Engine (NSE)
- Analyze and interpret scan results
- Understand scanning detection and evasion

## Prerequisites

- Completed [Lab 00: Environment Setup](lab-00-setup.md)
- Nmap installed on Kali Linux
- Target machines (Metasploitable, DVWA, etc.)
- Basic understanding of TCP/IP and ports

## Part 1: Nmap Basics

### Installation and Verification

1. **Check Nmap Installation**
   ```bash
   nmap --version
   which nmap
   
   # Install if needed
   sudo apt update
   sudo apt install nmap
   ```

2. **Basic Command Structure**
   ```bash
   nmap [Scan Type] [Options] {target specification}
   ```

### Target Specification

1. **Single Target**
   ```bash
   nmap 192.168.56.101
   nmap scanme.nmap.org
   ```

2. **Multiple Targets**
   ```bash
   nmap 192.168.56.101 192.168.56.102
   nmap 192.168.56.101-103
   nmap 192.168.56.0/24
   nmap 192.168.56.*
   ```

3. **Target Lists**
   ```bash
   # From file
   nmap -iL targets.txt
   
   # Exclude targets
   nmap 192.168.56.0/24 --exclude 192.168.56.1
   nmap 192.168.56.0/24 --excludefile exclude.txt
   ```

## Part 2: Host Discovery

### Ping Scans

1. **Basic Host Discovery**
   ```bash
   # Ping scan (default)
   nmap -sn 192.168.56.0/24
   
   # List scan (no ping)
   nmap -sL 192.168.56.0/24
   
   # No ping (assume all hosts up)
   nmap -Pn 192.168.56.101
   ```

2. **Ping Types**
   ```bash
   # ICMP ping
   nmap -PE 192.168.56.0/24
   
   # TCP SYN ping
   nmap -PS22,80,443 192.168.56.0/24
   
   # TCP ACK ping
   nmap -PA80,443 192.168.56.0/24
   
   # UDP ping
   nmap -PU53,67,68,123 192.168.56.0/24
   ```

### Advanced Host Discovery

1. **ARP Ping (Local Network)**
   ```bash
   # ARP ping (automatic for local subnet)
   nmap -PR 192.168.56.0/24
   
   # Verify with ARP table
   arp -a
   ```

2. **Combined Ping Types**
   ```bash
   # Multiple ping types
   nmap -PE -PS22,80,443 -PA80,443 192.168.56.0/24
   
   # Reason why hosts are up/down
   nmap -sn --reason 192.168.56.0/24
   ```

## Part 3: Port Scanning Techniques

### Basic Port Scans

1. **TCP Connect Scan**
   ```bash
   # Full TCP connection
   nmap -sT 192.168.56.101
   
   # Specific ports
   nmap -sT -p 22,80,443 192.168.56.101
   
   # Port ranges
   nmap -sT -p 1-1000 192.168.56.101
   ```

2. **SYN Scan (Default)**
   ```bash
   # SYN stealth scan
   nmap -sS 192.168.56.101
   
   # Requires root privileges
   sudo nmap -sS 192.168.56.101
   ```

### Advanced Scanning Techniques

1. **UDP Scan**
   ```bash
   # UDP port scan
   sudo nmap -sU 192.168.56.101
   
   # Common UDP ports
   sudo nmap -sU -p 53,67,68,123,161 192.168.56.101
   
   # Combined TCP/UDP scan
   sudo nmap -sS -sU -p T:80,443,U:53,161 192.168.56.101
   ```

2. **Specialized Scans**
   ```bash
   # FIN scan
   sudo nmap -sF 192.168.56.101
   
   # Null scan
   sudo nmap -sN 192.168.56.101
   
   # Xmas scan
   sudo nmap -sX 192.168.56.101
   
   # ACK scan (firewall detection)
   sudo nmap -sA 192.168.56.101
   ```

### Port Specifications

1. **Port Selection**
   ```bash
   # Top ports
   nmap --top-ports 100 192.168.56.101
   nmap --top-ports 1000 192.168.56.101
   
   # All ports
   nmap -p- 192.168.56.101
   
   # Specific protocols
   nmap -p T:80,443,U:53 192.168.56.101
   ```

2. **Port States**
   - **Open**: Service actively accepting connections
   - **Closed**: Port accessible but no service
   - **Filtered**: Packets filtered (firewall)
   - **Unfiltered**: Accessible but state unknown
   - **Open|Filtered**: Cannot determine if open or filtered
   - **Closed|Filtered**: Cannot determine if closed or filtered

## Part 4: Service and OS Detection

### Service Version Detection

1. **Basic Service Detection**
   ```bash
   # Version detection
   nmap -sV 192.168.56.101
   
   # Intensity levels (0-9)
   nmap -sV --version-intensity 5 192.168.56.101
   
   # Light version detection
   nmap -sV --version-light 192.168.56.101
   
   # Aggressive version detection
   nmap -sV --version-all 192.168.56.101
   ```

2. **Service Information**
   - Service name and version
   - Extra service info
   - Operating system details
   - Device type information

### OS Detection

1. **Operating System Detection**
   ```bash
   # OS detection
   sudo nmap -O 192.168.56.101
   
   # Aggressive OS detection
   sudo nmap -O --osscan-guess 192.168.56.101
   
   # OS detection with version
   sudo nmap -O -sV 192.168.56.101
   ```

2. **OS Detection Methods**
   - TCP/IP fingerprinting
   - TCP sequence prediction
   - TCP timestamp analysis
   - IP ID sequence generation
   - TCP window size analysis

## Part 5: Nmap Scripting Engine (NSE)

### Script Categories

1. **Default Scripts**
   ```bash
   # Default script scan
   nmap -sC 192.168.56.101
   
   # Equivalent to
   nmap --script=default 192.168.56.101
   ```

2. **Script Categories**
   ```bash
   # Vulnerability scripts
   nmap --script vuln 192.168.56.101
   
   # Authentication scripts
   nmap --script auth 192.168.56.101
   
   # Brute force scripts
   nmap --script brute 192.168.56.101
   
   # Discovery scripts
   nmap --script discovery 192.168.56.101
   ```

### Specific Scripts

1. **Common Useful Scripts**
   ```bash
   # HTTP information
   nmap --script http-title,http-server-header -p 80 192.168.56.101
   
   # SMB enumeration
   nmap --script smb-enum-shares,smb-enum-users -p 445 192.168.56.101
   
   # DNS information
   nmap --script dns-brute domain.com
   
   # SSL certificate info
   nmap --script ssl-cert -p 443 192.168.56.101
   ```

2. **Script Arguments**
   ```bash
   # HTTP methods
   nmap --script http-methods --script-args http-methods.url-path='/admin' -p 80 192.168.56.101
   
   # Brute force with wordlist
   nmap --script ssh-brute --script-args userdb=users.txt,passdb=passwords.txt -p 22 192.168.56.101
   ```

### Script Management

1. **Update Scripts**
   ```bash
   # Update script database
   sudo nmap --script-updatedb
   
   # List available scripts
   nmap --script-help=all | grep -E "^[a-z-]+" | sort
   
   # Script information
   nmap --script-help http-title
   ```

## Part 6: Timing and Performance

### Timing Templates

1. **Predefined Templates**
   ```bash
   # Timing templates (0-5)
   nmap -T0 192.168.56.101  # Paranoid (very slow)
   nmap -T1 192.168.56.101  # Sneaky (slow)
   nmap -T2 192.168.56.101  # Polite (slower)
   nmap -T3 192.168.56.101  # Normal (default)
   nmap -T4 192.168.56.101  # Aggressive (faster)
   nmap -T5 192.168.56.101  # Insane (very fast)
   ```

2. **Custom Timing**
   ```bash
   # Parallel host scan groups
   nmap --min-hostgroup 50 --max-hostgroup 100 192.168.56.0/24
   
   # Parallel port scan
   nmap --min-parallelism 100 --max-parallelism 256 192.168.56.101
   
   # Timeout values
   nmap --host-timeout 300s --min-rtt-timeout 100ms 192.168.56.101
   ```

### Performance Optimization

1. **Fast Scanning**
   ```bash
   # Fast scan
   nmap -F 192.168.56.101
   
   # Aggressive scan
   nmap -A 192.168.56.101
   
   # Quick scan
   nmap -T4 -F 192.168.56.0/24
   ```

2. **Bandwidth Control**
   ```bash
   # Limit scan rate
   nmap --max-rate 100 192.168.56.101
   
   # Minimum scan rate
   nmap --min-rate 50 192.168.56.101
   ```

## Part 7: Output and Reporting

### Output Formats

1. **Standard Output**
   ```bash
   # Normal output
   nmap 192.168.56.101
   
   # Verbose output
   nmap -v 192.168.56.101
   nmap -vv 192.168.56.101  # Very verbose
   
   # Debugging
   nmap -d 192.168.56.101
   ```

2. **File Output**
   ```bash
   # Normal output to file
   nmap -oN scan_results.txt 192.168.56.101
   
   # XML output
   nmap -oX scan_results.xml 192.168.56.101
   
   # Greppable output
   nmap -oG scan_results.gnmap 192.168.56.101
   
   # All formats
   nmap -oA scan_results 192.168.56.101
   ```

### Results Analysis

1. **Parsing Output**
   ```bash
   # Extract open ports from gnmap
   grep "open" scan_results.gnmap
   
   # Extract IP addresses with open ports
   grep "Up" scan_results.gnmap | cut -d' ' -f2
   
   # Convert XML to HTML
   xsltproc nmap_results.xml -o nmap_results.html
   ```

## Part 8: Stealth and Evasion

### Stealth Techniques

1. **Fragmentation**
   ```bash
   # Fragment packets
   nmap -f 192.168.56.101
   
   # Specify fragment size
   nmap --mtu 16 192.168.56.101
   ```

2. **Decoys**
   ```bash
   # Use decoy addresses
   nmap -D 192.168.56.50,192.168.56.51,ME 192.168.56.101
   
   # Random decoys
   nmap -D RND:5 192.168.56.101
   ```

3. **Source Port Spoofing**
   ```bash
   # Spoof source port
   nmap --source-port 53 192.168.56.101
   nmap -g 53 192.168.56.101
   ```

### IDS/IPS Evasion

1. **Timing Evasion**
   ```bash
   # Slow scanning
   nmap -T1 192.168.56.101
   
   # Random delays
   nmap --scan-delay 5s 192.168.56.101
   nmap --max-scan-delay 10s 192.168.56.101
   ```

2. **Protocol Evasion**
   ```bash
   # Idle scan (zombie host required)
   nmap -sI zombie_host:113 target_host
   
   # FTP bounce scan
   nmap -b ftp_server target_host
   ```

## Part 9: Practical Exercises

### Exercise 1: Network Discovery

1. **Host Discovery**
   ```bash
   # Discover live hosts in lab network
   nmap -sn 192.168.56.0/24
   
   # Try different ping types
   nmap -PE -PS -PA -PU 192.168.56.0/24
   ```

2. **Analysis Tasks**
   - Identify all live hosts
   - Note different response patterns
   - Document discovery techniques that work
   - Compare timing of different methods

### Exercise 2: Service Enumeration

1. **Port and Service Scan**
   ```bash
   # Comprehensive service scan
   sudo nmap -sS -sV -O -sC target_ip
   
   # Focus on specific services
   nmap -sV -p 21,22,23,25,53,80,110,111,135,139,143,443,993,995 target_ip
   ```

2. **Analysis Tasks**
   - Identify all running services
   - Note service versions and potential vulnerabilities
   - Determine operating system
   - Document interesting findings

### Exercise 3: Vulnerability Assessment

1. **Vulnerability Scanning**
   ```bash
   # Run vulnerability scripts
   nmap --script vuln target_ip
   
   # Specific vulnerability checks
   nmap --script smb-vuln-* -p 445 target_ip
   nmap --script http-vuln-* -p 80,443 target_ip
   ```

2. **Analysis Tasks**
   - Identify potential vulnerabilities
   - Prioritize findings by severity
   - Research identified vulnerabilities
   - Recommend remediation steps

## Part 10: Detection and Defense

### Scan Detection

1. **Monitoring for Scans**
   - Log analysis for connection patterns
   - Port scan detection with tools like PortSentry
   - Network IDS signatures for scan detection
   - Behavioral analysis of connection attempts

2. **Scan Signatures**
   - Multiple connections to different ports
   - Connections to uncommon ports
   - Failed connection attempts
   - Unusual timing patterns

### Defense Strategies

1. **Network Segmentation**
   - Isolate critical systems
   - Implement network access controls
   - Use VLANs for separation
   - Deploy network monitoring

2. **Firewall Configuration**
   - Block unnecessary ports
   - Implement port knocking
   - Use stateful inspection
   - Deploy intrusion prevention systems

## Lab Report Requirements

### Scan Documentation

1. **Discovery Results**
   - Network topology discovered
   - Live host inventory
   - Response time analysis
   - Discovery method effectiveness

2. **Service Enumeration**
   - Complete port/service inventory
   - Version information collected
   - Operating system identification
   - Banner information gathered

3. **Vulnerability Assessment**
   - Vulnerabilities identified
   - Risk assessment and prioritization
   - Recommended remediation steps
   - False positive analysis

### Deliverables

- Nmap scan output files (all formats)
- Network diagram of discovered topology
- Service inventory spreadsheet
- Vulnerability assessment report
- Detection and defense recommendations

## Next Steps

Continue with:
- **[Lab 03: Burp Suite Introduction](lab-03-burp-intro.md)** - Web application security testing
- **[Lab CTF Overview](lab-ctf-overview.md)** - Capture The Flag challenges

## Additional Resources

- Nmap Official Documentation
- Nmap Network Scanning book
- NSE Script Documentation
- Network reconnaissance methodologies
- IDS/IPS evasion techniques