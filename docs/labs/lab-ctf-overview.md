# Lab CTF: Capture The Flag Overview

## Overview

Capture The Flag (CTF) competitions are cybersecurity challenges that test various skills including cryptography, reverse engineering, web exploitation, forensics, and more. This lab introduces CTF concepts and provides practical exercises to develop competitive cybersecurity skills.

## Objectives

By the end of this lab, you will be able to:
- Understand different types of CTF challenges
- Apply systematic approaches to CTF problem-solving
- Use various tools and techniques for different challenge categories
- Develop methodical thinking for complex security problems
- Participate in online CTF competitions

## Prerequisites

- Completed previous labs (Setup, Wireshark, Nmap, Burp Suite)
- Basic understanding of common protocols and technologies
- Familiarity with Linux command line
- Basic programming knowledge (Python, JavaScript, etc.)

## Part 1: CTF Fundamentals

### Types of CTF Competitions

1. **Jeopardy-Style CTF**
   - Individual challenges in various categories
   - Point values based on difficulty
   - Teams solve challenges independently
   - Most common format for online competitions

2. **Attack-Defense CTF**
   - Teams maintain their own services
   - Attack other teams while defending own services
   - Real-time competition with scoring
   - More advanced format requiring infrastructure

3. **King of the Hill**
   - Single target system
   - Teams compete for control
   - Continuous scoring based on control time
   - Focus on system exploitation and persistence

### Common CTF Categories

1. **Web Exploitation**
   - SQL injection, XSS, CSRF
   - Authentication bypasses
   - File inclusion vulnerabilities
   - Server-side template injection

2. **Cryptography**
   - Classical ciphers (Caesar, Vigenère)
   - Modern cryptographic challenges
   - Hash function attacks
   - RSA and other public key attacks

3. **Reverse Engineering**
   - Binary analysis
   - Malware analysis
   - Protocol reverse engineering
   - Firmware analysis

4. **Forensics**
   - Memory dumps analysis
   - Network traffic analysis
   - File system analysis
   - Steganography

5. **Binary Exploitation (Pwn)**
   - Buffer overflows
   - Return-oriented programming
   - Format string vulnerabilities
   - Heap exploitation

6. **Miscellaneous**
   - OSINT (Open Source Intelligence)
   - Social engineering
   - Physical security
   - Programming challenges

## Part 2: Essential CTF Tools

### Multi-Purpose Tools

1. **Kali Linux Distribution**
   - Comprehensive security tool collection
   - Pre-configured for security testing
   - Regular updates with new tools
   - Excellent for CTF competitions

2. **Python and Scripting**
   ```python
   # Common Python libraries for CTF
   import requests     # HTTP requests
   import hashlib      # Hashing functions
   import base64       # Encoding/decoding
   import binascii     # Binary/ASCII conversion
   import socket       # Network communication
   import struct       # Binary data handling
   ```

### Category-Specific Tools

1. **Web Exploitation**
   ```bash
   # Burp Suite (already covered)
   burpsuite
   
   # OWASP ZAP
   zaproxy
   
   # SQLmap
   sqlmap -u "http://target/page?id=1"
   
   # Dirb/Dirbuster
   dirb http://target/ /usr/share/wordlists/dirb/common.txt
   ```

2. **Cryptography**
   ```bash
   # Hashcat for hash cracking
   hashcat -m 0 -a 0 hash.txt wordlist.txt
   
   # John the Ripper
   john --wordlist=wordlist.txt hash.txt
   
   # OpenSSL for crypto operations
   openssl enc -d -aes-256-cbc -in encrypted.txt
   ```

3. **Reverse Engineering**
   ```bash
   # Ghidra (NSA reverse engineering tool)
   ghidra
   
   # Radare2
   r2 binary_file
   
   # Strings command
   strings binary_file | grep -i flag
   
   # Objdump
   objdump -d binary_file
   ```

4. **Forensics**
   ```bash
   # Wireshark (already covered)
   wireshark capture.pcap
   
   # Volatility for memory analysis
   volatility -f memory.dump --profile=Win7SP1x64 pslist
   
   # Binwalk for firmware analysis
   binwalk -e firmware.bin
   
   # Foremost for file carving
   foremost -i disk.img -o extracted/
   ```

### Online Tools and Resources

1. **Decoding/Encoding**
   - CyberChef (https://gchq.github.io/CyberChef/)
   - Online base64, hex, URL decoders
   - Hash identification tools
   - Character encoding converters

2. **Cryptography**
   - factordb.com for integer factorization
   - quipqiup.com for substitution ciphers
   - Cryptanalysis tools and calculators

## Part 3: CTF Problem-Solving Methodology

### General Approach

1. **Read Carefully**
   - Understand the challenge description
   - Note any hints or constraints
   - Identify the challenge category
   - Look for unusual phrasing or clues

2. **Reconnaissance**
   - Gather all available information
   - Examine provided files thoroughly
   - Test basic interactions
   - Document observations

3. **Hypothesis Formation**
   - Based on reconnaissance, form theories
   - Consider multiple attack vectors
   - Prioritize most likely approaches
   - Keep backup strategies

4. **Testing and Iteration**
   - Test hypotheses systematically
   - Document what works and what doesn't
   - Modify approach based on results
   - Don't get stuck on one method

### Category-Specific Strategies

1. **Web Exploitation Strategy**
   ```
   1. Map the application (spider/crawl)
   2. Test for common vulnerabilities
   3. Analyze source code if available
   4. Check for hidden files/directories
   5. Test input validation thoroughly
   6. Look for logic flaws
   ```

2. **Cryptography Strategy**
   ```
   1. Identify the cipher/encoding type
   2. Look for patterns or known plaintexts
   3. Try common keys or passwords
   4. Research known attacks for the cipher
   5. Consider frequency analysis
   6. Check for implementation flaws
   ```

3. **Reverse Engineering Strategy**
   ```
   1. Static analysis first (strings, disassembly)
   2. Dynamic analysis if needed (debugging)
   3. Look for obvious functions or strings
   4. Understand the program flow
   5. Identify key algorithms or checks
   6. Patch or bypass protections
   ```

## Part 4: Hands-On CTF Challenges

### Challenge 1: Basic Web Exploitation

1. **Challenge Setup**
   ```php
   # Simple PHP challenge
   <?php
   if ($_GET['page']) {
       include($_GET['page']);
   } else {
       echo "Welcome! Try ?page=home";
   }
   ?>
   ```

2. **Solution Approach**
   - Test for Local File Inclusion (LFI)
   - Try common payloads like `../../../etc/passwd`
   - Look for flag file in common locations
   - Use PHP wrappers for code execution

### Challenge 2: Basic Cryptography

1. **Challenge Setup**
   ```
   Encrypted message: WKLV LV D VLPSOH FDHVDU FLSKHU
   Hint: Julius would be proud
   ```

2. **Solution Approach**
   - Recognize Caesar cipher from hint
   - Try different shift values
   - Look for readable English text
   - Common shift values: 13 (ROT13), 3 (classic Caesar)

### Challenge 3: Basic Forensics

1. **Challenge Setup**
   - Provide a suspicious network capture file
   - Hidden flag in the traffic

2. **Solution Approach**
   ```bash
   # Analyze with Wireshark
   wireshark capture.pcap
   
   # Look for suspicious protocols
   # Check HTTP traffic for flags
   # Examine DNS queries
   # Look for data exfiltration
   ```

### Challenge 4: Basic Reverse Engineering

1. **Challenge Setup**
   - Simple binary that checks for correct input
   - Flag revealed with correct password

2. **Solution Approach**
   ```bash
   # Run strings to look for obvious text
   strings challenge_binary
   
   # Disassemble to understand logic
   objdump -d challenge_binary
   
   # Use debugging tools if needed
   gdb challenge_binary
   ```

## Part 5: Advanced CTF Techniques

### Web Exploitation Advanced

1. **SQL Injection Techniques**
   ```sql
   # Boolean-based blind injection
   1' AND (SELECT COUNT(*) FROM users) > 0 --
   
   # Time-based blind injection
   1'; WAITFOR DELAY '00:00:05' --
   
   # Union-based injection
   1' UNION SELECT 1,2,3,database() --
   ```

2. **Server-Side Template Injection**
   ```python
   # Jinja2 template injection
   {{ ''.__class__.__mro__[2].__subclasses__() }}
   
   # Command execution through SSTI
   {{ config.__class__.__init__.__globals__['os'].popen('ls').read() }}
   ```

### Cryptography Advanced

1. **RSA Attacks**
   ```python
   # Factoring small RSA moduli
   def factor_rsa(n):
       for p in range(2, int(n**0.5) + 1):
           if n % p == 0:
               return p, n // p
   
   # Common modulus attack
   def common_modulus_attack(c1, c2, e1, e2, n):
       # Implementation of extended GCD attack
       pass
   ```

2. **Hash Extension Attacks**
   ```python
   # Length extension attack on MD5/SHA1
   import hashpumpy
   
   original_data = "admin=False"
   original_hash = "5d41402abc4b2a76b9719d911017c592"
   append_data = "&admin=True"
   
   new_hash, new_data = hashpumpy.hashpump(
       original_hash, original_data, append_data, len(original_data)
   )
   ```

### Binary Exploitation Basics

1. **Buffer Overflow Detection**
   ```python
   # Simple buffer overflow test
   import subprocess
   
   for i in range(1, 1000):
       payload = "A" * i
       try:
           result = subprocess.run(["./vulnerable_program", payload], 
                                 capture_output=True, timeout=5)
           if result.returncode != 0:
               print(f"Crash at {i} bytes")
               break
       except subprocess.TimeoutExpired:
           print(f"Timeout at {i} bytes")
   ```

## Part 6: Popular CTF Platforms

### Beginner-Friendly Platforms

1. **PicoCTF**
   - Educational CTF platform
   - Yearly competition with practice problems
   - Good for beginners
   - Comprehensive learning resources

2. **OverTheWire**
   - Progressive difficulty levels
   - Focus on Linux and command line
   - Excellent for learning basics
   - Bandit series for beginners

3. **HackTheBox**
   - Retired machines for practice
   - Active machines for subscribed users
   - Real-world scenarios
   - Community writeups available

### Competition Platforms

1. **CTFtime.org**
   - Comprehensive CTF event listing
   - Team rankings and statistics
   - Writeup collection
   - Calendar of upcoming events

2. **Major Annual CTFs**
   - DEFCON CTF Finals
   - PlaidCTF
   - Google CTF
   - CSAW CTF

## Part 7: Building CTF Skills

### Practice Recommendations

1. **Daily Practice**
   - Solve at least one challenge daily
   - Focus on weaker categories
   - Review writeups after solving
   - Keep notes on techniques learned

2. **Team Formation**
   - Join or form a CTF team
   - Collaborate on difficult challenges
   - Learn from teammates' expertise
   - Participate in team competitions

### Skill Development Areas

1. **Programming Skills**
   ```python
   # Essential Python for CTF
   - String manipulation
   - Binary operations
   - Network programming
   - Automation scripts
   - Cryptographic operations
   ```

2. **System Knowledge**
   - Linux system administration
   - Network protocols
   - Web technologies
   - Cryptographic concepts
   - Assembly language basics

## Part 8: Creating Your Own CTF Challenges

### Challenge Design Principles

1. **Educational Value**
   - Teach specific security concepts
   - Provide appropriate difficulty progression
   - Include realistic scenarios
   - Offer multiple solution paths

2. **Technical Quality**
   - Test thoroughly before deployment
   - Provide clear, unambiguous flags
   - Ensure stable infrastructure
   - Include appropriate hints

### Challenge Categories to Create

1. **Web Challenges**
   ```php
   # Example: Authentication bypass
   if ($_POST['username'] == 'admin' && 
       md5($_POST['password']) == '5d41402abc4b2a76b9719d911017c592') {
       echo "Flag: CTF{md5_is_not_secure}";
   }
   ```

2. **Crypto Challenges**
   ```python
   # Example: Weak random number generator
   import random
   random.seed(1337)  # Weak seed
   key = random.randint(1000, 9999)
   # Use key to encrypt flag
   ```

## Part 9: Competition Strategies

### Time Management

1. **Challenge Prioritization**
   - Start with easier challenges
   - Focus on your strongest categories
   - Switch challenges when stuck
   - Save time for high-point challenges

2. **Team Coordination**
   - Assign challenges based on expertise
   - Share information and tools
   - Avoid duplicate work
   - Communicate discoveries quickly

### Documentation and Writeups

1. **During Competition**
   - Document solution steps
   - Save important commands/scripts
   - Screenshot key findings
   - Note failed attempts and reasons

2. **Post-Competition**
   - Write detailed writeups
   - Share interesting techniques
   - Learn from others' solutions
   - Contribute to community knowledge

## Lab Exercise: Mini-CTF

### Exercise Setup

Create a mini-CTF with challenges covering:
1. Basic web exploitation
2. Simple cryptography
3. Network forensics
4. Basic reverse engineering

### Sample Challenges

1. **Web: Hidden Flag**
   - Create simple web page with flag in HTML comments
   - Add robots.txt with interesting directories
   - Include basic form with SQL injection

2. **Crypto: Multi-layer Encoding**
   - Base64 encode a ROT13 encoded flag
   - Provide hint about multiple layers
   - Include unusual character patterns

3. **Forensics: Network Traffic**
   - Create PCAP with flag in HTTP POST data
   - Include decoy traffic
   - Hide flag in unusual protocol field

4. **Reverse: String Comparison**
   - Simple binary comparing input to flag
   - Use strings command to find flag
   - Include obfuscation if advanced

## Lab Report Requirements

### CTF Experience Documentation

1. **Challenge Solutions**
   - Step-by-step solution process
   - Tools and techniques used
   - Obstacles encountered
   - Alternative approaches considered

2. **Skills Assessment**
   - Strengths and weaknesses identified
   - New techniques learned
   - Areas for improvement
   - Future learning goals

3. **Tool Evaluation**
   - Most useful tools for each category
   - Tool limitations encountered
   - Recommended tool combinations
   - Custom scripts developed

### Deliverables

- Solutions to provided CTF challenges
- Documentation of solution methodology
- Custom challenge creation (optional)
- Reflection on CTF experience
- Plan for continued CTF participation

## Next Steps and Continued Learning

### Advanced Topics

1. **Specialized Areas**
   - Hardware hacking
   - IoT security
   - Automotive security
   - Industrial control systems

2. **Advanced Techniques**
   - Exploit development
   - Malware analysis
   - Mobile security
   - Cloud security

### Community Involvement

1. **Contributing Back**
   - Create and share challenges
   - Write detailed writeups
   - Mentor beginners
   - Organize local competitions

2. **Professional Development**
   - Build portfolio with CTF achievements
   - Network with security professionals
   - Develop specialized expertise
   - Consider cybersecurity career paths

## Additional Resources

- CTFtime.org for competition listings
- CTF writeup repositories on GitHub
- Security conference presentations
- Academic papers on security topics
- Professional security training courses
- Local security meetups and conferences

## Conclusion

Capture The Flag competitions provide an excellent way to develop practical cybersecurity skills in a challenging and engaging environment. The skills learned through CTF participation directly translate to real-world security assessment and defense capabilities. Regular practice and competition participation will significantly enhance your cybersecurity expertise and problem-solving abilities.