# Security Tools and Labs

## Overview

This module introduces essential cybersecurity tools and provides guidance for hands-on laboratory exercises. Practical experience with these tools is crucial for developing real-world security skills.

## Learning Objectives

By the end of this module, you will be able to:
- Identify and use common security tools
- Set up a cybersecurity lab environment
- Perform basic security assessments
- Understand tool capabilities and limitations

## Categories of Security Tools

### 1. Network Security Tools

#### Network Scanners
- **Nmap**: Network discovery and port scanning
- **Masscan**: High-speed port scanner
- **Zmap**: Internet-wide network scanner
- **Angry IP Scanner**: GUI-based network scanner

#### Network Analysis
- **Wireshark**: Network protocol analyzer
- **tcpdump**: Command-line packet capture
- **NetworkMiner**: Network forensic analysis
- **Snort**: Network intrusion detection system

#### Wireless Security
- **Aircrack-ng**: Wi-Fi security auditing suite
- **Kismet**: Wireless network detector and analyzer
- **Wifite**: Automated wireless attack tool
- **Reaver**: WPS PIN recovery tool

### 2. Web Application Security Tools

#### Web Scanners
- **OWASP ZAP**: Web application security scanner
- **Burp Suite**: Web vulnerability scanner and proxy
- **Nikto**: Web server scanner
- **Dirb/Dirbuster**: Directory and file brute-forcer

#### Specialized Web Tools
- **SQLmap**: Automatic SQL injection exploitation
- **XSSer**: Cross-site scripting detection
- **Commix**: Command injection exploitation
- **W3af**: Web application attack and audit framework

### 3. Vulnerability Assessment Tools

#### Vulnerability Scanners
- **Nessus**: Comprehensive vulnerability scanner
- **OpenVAS**: Open-source vulnerability scanner
- **Qualys VMDR**: Cloud-based vulnerability management
- **Rapid7 Nexpose**: Vulnerability management solution

#### Operating System Tools
- **Lynis**: System hardening and compliance scanning
- **AIDE**: Advanced Intrusion Detection Environment
- **Chkrootkit**: Rootkit detection
- **RKill**: Malware termination utility

### 4. Penetration Testing Frameworks

#### Comprehensive Frameworks
- **Metasploit**: Exploitation framework
- **Cobalt Strike**: Adversary simulation platform
- **Empire**: PowerShell post-exploitation framework
- **Armitage**: Graphical cyber attack management

#### Specialized Frameworks
- **SET**: Social Engineering Toolkit
- **BeEF**: Browser Exploitation Framework
- **Veil**: Payload generation framework
- **TheFatRat**: Backdoor generation tool

### 5. Forensics and Incident Response

#### Digital Forensics
- **Autopsy**: Digital forensics platform
- **Volatility**: Memory analysis framework
- **YARA**: Malware identification and classification
- **Binwalk**: Firmware analysis tool

#### Incident Response
- **OSSEC**: Host-based intrusion detection
- **TheHive**: Incident response platform
- **GRR**: Remote live forensics framework
- **Velociraptor**: Endpoint visibility and response

## Lab Environment Setup

### 1. Virtualization Platforms
- **VMware Workstation/Player**: Professional virtualization
- **VirtualBox**: Free virtualization platform
- **QEMU/KVM**: Linux-based virtualization
- **Hyper-V**: Windows native virtualization

### 2. Security-Focused Distributions

#### Penetration Testing
- **Kali Linux**: Debian-based penetration testing distribution
- **Parrot Security OS**: Multi-purpose security platform
- **BlackArch Linux**: Arch-based penetration testing distribution
- **Pentoo**: Gentoo-based security testing platform

#### Defensive Security
- **Security Onion**: Network security monitoring platform
- **SELKS**: Suricata/ELK-based IDS
- **SANS SIFT**: Digital forensics and incident response
- **REMnux**: Malware analysis toolkit

### 3. Vulnerable Applications and Systems

#### Web Applications
- **DVWA**: Damn Vulnerable Web Application
- **WebGoat**: OWASP teaching application
- **Mutillidae II**: Web application security training
- **bWAPP**: Buggy Web Application

#### Operating Systems
- **Metasploitable**: Intentionally vulnerable Linux
- **VulnHub VMs**: Collection of vulnerable machines
- **HackTheBox**: Online penetration testing platform
- **TryHackMe**: Beginner-friendly security challenges

## Tool Selection Guidelines

### Considerations for Tool Selection
- **Purpose**: Specific security testing requirements
- **Skill Level**: User expertise and learning curve
- **License**: Commercial vs. open-source considerations
- **Integration**: Compatibility with existing tools
- **Support**: Community and vendor support

### Legal and Ethical Considerations
- Only test systems you own or have explicit permission to test
- Understand applicable laws and regulations
- Follow responsible disclosure practices
- Maintain professional ethics and integrity

## Laboratory Exercises

This module includes the following hands-on laboratories:

1. **[Lab 00: Setup](labs/lab-00-setup.md)** - Environment configuration
2. **[Lab 01: Wireshark Basics](labs/lab-01-wireshark-basics.md)** - Network analysis
3. **[Lab 02: Nmap and Scanning](labs/lab-02-nmap-and-scanning.md)** - Network reconnaissance
4. **[Lab 03: Burp Suite Introduction](labs/lab-03-burp-intro.md)** - Web application testing
5. **[Lab CTF Overview](labs/lab-ctf-overview.md)** - Capture The Flag introduction

## Tool Documentation and Resources

### Official Documentation
- Always reference official tool documentation
- Follow vendor best practices and guidelines
- Subscribe to security advisories and updates

### Community Resources
- GitHub repositories and projects
- Security conferences and presentations
- Online training platforms and courses
- Professional security communities

## Next Steps

Continue to [Security Mindset](06-security-mindset.md) to develop critical thinking skills for cybersecurity.