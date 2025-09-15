# Threats and Vulnerabilities

## Overview

Understanding the threat landscape and common vulnerabilities is essential for effective cybersecurity defense. This module explores various attack vectors, threat actors, and vulnerability types.

## Learning Objectives

By the end of this module, you will be able to:
- Identify different types of threats and threat actors
- Understand common vulnerability categories
- Recognize attack vectors and techniques
- Apply threat modeling concepts

## Threat Actors

### 1. Types of Threat Actors

#### Nation-State Actors
- Government-sponsored or affiliated groups
- Advanced Persistent Threats (APTs)
- Targets: Critical infrastructure, intellectual property, political intelligence
- Examples: APT1, Lazarus Group, Fancy Bear

#### Cybercriminals
- Financially motivated individuals or groups
- Ransomware operators, credit card fraudsters
- Targets: Financial gain through data theft, extortion
- Examples: Ransomware-as-a-Service (RaaS) groups

#### Hacktivists
- Politically or socially motivated groups
- Use cyberattacks to promote causes or ideologies
- Targets: Government agencies, corporations with opposing views
- Examples: Anonymous, various environmental or social justice groups

#### Insider Threats
- Current or former employees, contractors, or business partners
- May be malicious or unintentional
- Access to internal systems and sensitive data
- Types: Malicious insiders, negligent users, compromised accounts

#### Script Kiddies
- Inexperienced hackers using existing tools
- Limited technical knowledge
- Often motivated by curiosity or recognition
- Generally less sophisticated attacks

## Common Attack Vectors

### 1. Social Engineering
- **Phishing**: Fraudulent emails to steal credentials
- **Spear Phishing**: Targeted phishing attacks
- **Pretexting**: Creating false scenarios to extract information
- **Baiting**: Offering something enticing to trigger downloads
- **Tailgating**: Following authorized personnel into secure areas

### 2. Malware
- **Viruses**: Self-replicating programs
- **Worms**: Network-spreading malware
- **Trojans**: Disguised malicious programs
- **Ransomware**: Encryption-based extortion
- **Spyware**: Information-gathering software
- **Rootkits**: System-level hiding mechanisms

### 3. Network Attacks
- **Man-in-the-Middle (MitM)**: Intercepting communications
- **Denial of Service (DoS/DDoS)**: Service disruption
- **Packet Sniffing**: Network traffic interception
- **ARP Spoofing**: MAC address impersonation
- **DNS Poisoning**: Corrupting DNS records

### 4. Web Application Attacks
- **SQL Injection**: Database manipulation through input
- **Cross-Site Scripting (XSS)**: Client-side code injection
- **Cross-Site Request Forgery (CSRF)**: Unauthorized actions
- **Directory Traversal**: Unauthorized file access
- **Session Hijacking**: Stealing user sessions

## Vulnerability Categories

### 1. OWASP Top 10
1. Injection
2. Broken Authentication
3. Sensitive Data Exposure
4. XML External Entities (XXE)
5. Broken Access Control
6. Security Misconfiguration
7. Cross-Site Scripting (XSS)
8. Insecure Deserialization
9. Using Components with Known Vulnerabilities
10. Insufficient Logging & Monitoring

### 2. Common Vulnerability Scoring System (CVSS)
- Standardized method for rating vulnerability severity
- Base score (0.0-10.0) based on exploitability and impact
- Temporal and environmental metrics for context

### 3. Vulnerability Lifecycle
1. **Discovery**: Identification of vulnerability
2. **Disclosure**: Responsible disclosure to vendor
3. **Patching**: Vendor develops and releases fix
4. **Deployment**: Organizations apply patches
5. **Exploitation**: Potential misuse by threat actors

## Attack Kill Chain

### Lockheed Martin Kill Chain
1. **Reconnaissance**: Target research and information gathering
2. **Weaponization**: Creating malicious payload
3. **Delivery**: Transmitting weapon to target
4. **Exploitation**: Triggering vulnerability
5. **Installation**: Installing persistent access
6. **Command and Control**: Establishing communication channel
7. **Actions on Objectives**: Achieving intended goal

### MITRE ATT&CK Framework
- Comprehensive matrix of tactics and techniques
- Based on real-world observations
- Used for threat modeling and detection

## Threat Intelligence

### Types of Intelligence
- **Strategic**: High-level trends and patterns
- **Tactical**: Specific techniques and procedures
- **Operational**: Campaign and actor information
- **Technical**: Indicators of Compromise (IoCs)

### Threat Modeling
- Systematic approach to identifying threats
- STRIDE methodology (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege)
- Asset-based, attacker-based, or software-based approaches

## Next Steps

Continue to [Tools and Labs](05-tools-and-labs.md) to learn about security tools and hands-on practice.