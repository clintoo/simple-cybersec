# Lab 03: Burp Suite Introduction

## Overview

This lab introduces Burp Suite, a powerful web application security testing platform. You'll learn to intercept, analyze, and modify HTTP traffic, perform automated scans, and identify common web application vulnerabilities.

## Objectives

By the end of this lab, you will be able to:
- Configure and use Burp Suite proxy
- Intercept and modify HTTP requests and responses
- Use Burp's various tools (Spider, Intruder, Repeater)
- Perform automated vulnerability scans
- Analyze web application security issues
- Generate security assessment reports

## Prerequisites

- Completed [Lab 00: Environment Setup](lab-00-setup.md)
- Burp Suite installed (Community or Professional)
- DVWA or other vulnerable web application
- Web browser configured to work with Burp
- Basic understanding of HTTP protocol

## Part 1: Burp Suite Setup and Configuration

### Installation and Initial Setup

1. **Start Burp Suite**
   ```bash
   # From command line (Kali Linux)
   burpsuite
   
   # Or from desktop menu
   # Applications > Web Application Analysis > Burp Suite
   ```

2. **Project Configuration**
   - Choose "Temporary project" for practice
   - Select "Use Burp defaults" for configuration
   - Wait for Burp to fully load

### Proxy Configuration

1. **Burp Proxy Settings**
   - Navigate to Proxy > Options
   - Verify proxy listener on 127.0.0.1:8080
   - Note the interface and port configuration

2. **Browser Proxy Configuration**
   
   **Firefox Configuration:**
   ```
   Settings > Network Settings > Manual proxy configuration
   HTTP Proxy: 127.0.0.1
   Port: 8080
   Use this proxy server for all protocols: Yes
   ```

   **Chrome/Chromium:**
   ```bash
   # Start Chrome with proxy
   google-chrome --proxy-server=127.0.0.1:8080
   ```

3. **SSL Certificate Installation**
   - Browse to http://burp while proxy is running
   - Download CA certificate
   - Install in browser's certificate store
   - Verify HTTPS interception works

### Initial Testing

1. **Test Proxy Interception**
   - Enable interception: Proxy > Intercept > Intercept is on
   - Browse to any website
   - Observe intercepted requests in Burp
   - Forward or drop requests as needed

2. **HTTP History**
   - Navigate to Proxy > HTTP history
   - Review all captured requests
   - Filter by host, file extension, or status code
   - Examine request and response details

## Part 2: Target Application Setup

### DVWA Configuration

1. **Access DVWA**
   ```bash
   # Ensure DVWA is running
   sudo systemctl start apache2
   sudo systemctl start mysql
   
   # Navigate to DVWA
   http://192.168.56.101/DVWA
   ```

2. **DVWA Initial Setup**
   - Login with admin/password
   - Navigate to Setup page
   - Click "Create / Reset Database"
   - Set security level to "Low" initially

3. **Add Target to Burp**
   - Navigate to Target > Site map
   - Browse DVWA through proxy
   - Observe site map population
   - Add to scope if desired

### Alternative Targets

1. **WebGoat**
   ```bash
   # Download and start WebGoat
   wget https://github.com/WebGoat/WebGoat/releases/download/v8.2.2/webgoat-server-8.2.2.jar
   java -jar webgoat-server-8.2.2.jar
   ```

2. **Online Practice Sites**
   - http://testphp.vulnweb.com/
   - https://httpbin.org/
   - https://reqres.in/

## Part 3: Burp Suite Tools Overview

### Proxy Tool

1. **Interception Features**
   - Request/response modification
   - Match and replace rules
   - SSL pass through
   - Response modification

2. **Useful Proxy Functions**
   ```
   # Common proxy actions
   - Forward: Send request to server
   - Drop: Discard the request
   - Action > Send to tool: Forward to other Burp tools
   - Action > Do intercept > Response: Intercept server response
   ```

### Target Tool

1. **Site Map**
   - Hierarchical view of application
   - Color coding for different content types
   - Request/response viewer
   - Issue annotations

2. **Scope Definition**
   - Include/exclude URLs from scope
   - Focus scanning and spidering
   - Reduce noise in other tools

### Spider Tool

1. **Automated Crawling**
   ```
   # Spider configuration
   Spider > Options
   - Set crawl depth and limits
   - Configure form submission
   - Set maximum number of pages
   - Handle JavaScript links
   ```

2. **Manual Spidering**
   - Right-click on target > Spider this host
   - Monitor progress in Spider > Control
   - Review discovered content
   - Check for missed endpoints

## Part 4: HTTP Request Manipulation

### Using Repeater Tool

1. **Send Request to Repeater**
   - Right-click any request > Send to Repeater
   - Navigate to Repeater tab
   - Modify request parameters
   - Send modified requests

2. **Request Modification Examples**
   ```http
   # Original request
   GET /DVWA/vulnerabilities/sqli/?id=1&Submit=Submit HTTP/1.1
   Host: 192.168.56.101
   
   # Modified request
   GET /DVWA/vulnerabilities/sqli/?id=1'&Submit=Submit HTTP/1.1
   Host: 192.168.56.101
   ```

3. **Response Analysis**
   - Compare original vs. modified responses
   - Look for error messages
   - Note response time differences
   - Identify behavioral changes

### Using Intruder Tool

1. **Attack Types**
   - **Sniper**: Single payload set, one position
   - **Battering ram**: Single payload set, all positions
   - **Pitchfork**: Multiple payload sets, synchronized
   - **Cluster bomb**: Multiple payload sets, all combinations

2. **Payload Configuration**
   ```
   # Common payload types
   - Simple list: Predefined wordlists
   - Runtime file: Load from file
   - Numbers: Sequential or random numbers
   - Brute forcer: Character-based brute force
   ```

3. **Attack Example: SQL Injection**
   ```http
   # Base request
   GET /DVWA/vulnerabilities/sqli/?id=§1§&Submit=Submit HTTP/1.1
   
   # Payload list
   1
   1'
   1"
   1' OR '1'='1
   1' UNION SELECT 1,2--
   ```

## Part 5: Vulnerability Testing

### SQL Injection Testing

1. **Manual Testing with Repeater**
   ```http
   # Test basic injection
   GET /DVWA/vulnerabilities/sqli/?id=1' OR '1'='1&Submit=Submit
   
   # Test UNION injection
   GET /DVWA/vulnerabilities/sqli/?id=1' UNION SELECT 1,2--&Submit=Submit
   
   # Extract database information
   GET /DVWA/vulnerabilities/sqli/?id=1' UNION SELECT user(),database()--&Submit=Submit
   ```

2. **Automated Testing with Intruder**
   - Set up Intruder attack on SQL injection point
   - Load SQL injection payloads
   - Analyze results for successful injections
   - Look for error messages or timing differences

### Cross-Site Scripting (XSS) Testing

1. **Reflected XSS**
   ```javascript
   # Test payloads
   <script>alert('XSS')</script>
   <img src=x onerror=alert('XSS')>
   javascript:alert('XSS')
   "><script>alert('XSS')</script>
   ```

2. **Stored XSS**
   - Submit XSS payloads through forms
   - Check if payload persists
   - Test different contexts (HTML, attributes, JavaScript)

### Authentication Testing

1. **Brute Force Testing**
   ```
   # Set up Intruder on login form
   POST /DVWA/login.php HTTP/1.1
   Content-Type: application/x-www-form-urlencoded
   
   username=admin&password=§password§&Login=Login
   
   # Load common passwords
   - password
   - admin
   - 123456
   - password123
   ```

2. **Session Management Testing**
   - Analyze session tokens
   - Test session fixation
   - Check session timeout
   - Test concurrent sessions

## Part 6: Automated Scanning

### Burp Scanner (Professional Only)

1. **Active Scanning**
   - Right-click target > Actively scan this host
   - Configure scan options
   - Monitor scan progress
   - Review identified issues

2. **Passive Scanning**
   - Automatically runs during browsing
   - No additional requests sent
   - Identifies issues in observed traffic
   - Lower risk of application disruption

### Community Edition Limitations

1. **Manual Testing Focus**
   - Use manual testing techniques
   - Leverage free extensions
   - Combine with other tools
   - Document findings manually

2. **Extension Integration**
   - Install useful extensions from BApp Store
   - Use extensions for specialized testing
   - Integrate with external tools
   - Enhance capabilities

## Part 7: Advanced Features

### Match and Replace Rules

1. **Automatic Modifications**
   ```
   # Example rules
   Type: Request header
   Match: User-Agent: .*
   Replace: User-Agent: CustomUserAgent
   
   Type: Request parameter
   Match: sessionid
   Replace: [different session ID]
   ```

### Macro and Session Handling

1. **Complex Authentication**
   - Record login sequence as macro
   - Configure session handling rules
   - Maintain authenticated state
   - Handle CSRF tokens automatically

### Collaborator (Professional)

1. **Out-of-Band Testing**
   - Blind SQL injection detection
   - SSRF vulnerability testing
   - XXE exploitation
   - DNS and HTTP interactions

## Part 8: Reporting and Documentation

### Issue Documentation

1. **Manual Documentation**
   - Vulnerability description
   - Proof of concept requests
   - Impact assessment
   - Remediation recommendations

2. **Screenshot Documentation**
   - Capture request/response pairs
   - Show vulnerability exploitation
   - Document configuration settings
   - Provide visual evidence

### Report Generation

1. **Burp Professional Reports**
   - Generate HTML/XML reports
   - Customize report content
   - Include executive summary
   - Technical details for developers

2. **Manual Report Structure**
   ```
   1. Executive Summary
   2. Testing Methodology
   3. Findings Summary
   4. Detailed Findings
   5. Risk Assessment
   6. Recommendations
   7. Appendices
   ```

## Part 9: Practical Exercises

### Exercise 1: SQL Injection Exploitation

1. **Objective**
   - Identify SQL injection in DVWA
   - Extract database information
   - Demonstrate data exfiltration

2. **Steps**
   ```
   1. Navigate to SQL Injection page in DVWA
   2. Intercept requests with Burp
   3. Test for injection points
   4. Extract database schema
   5. Retrieve user data
   6. Document the attack
   ```

### Exercise 2: Session Management Testing

1. **Objective**
   - Analyze session token security
   - Test session fixation
   - Check concurrent sessions

2. **Steps**
   ```
   1. Login to DVWA
   2. Analyze session tokens
   3. Test token predictability
   4. Check session timeout
   5. Test multiple sessions
   6. Document findings
   ```

### Exercise 3: Cross-Site Scripting Discovery

1. **Objective**
   - Find XSS vulnerabilities
   - Test different payload types
   - Demonstrate impact

2. **Steps**
   ```
   1. Navigate to XSS pages in DVWA
   2. Test reflected XSS payloads
   3. Test stored XSS payloads
   4. Bypass any filters
   5. Create proof of concept
   6. Document remediation
   ```

## Part 10: Best Practices and Ethics

### Testing Ethics

1. **Authorized Testing Only**
   - Only test applications you own
   - Obtain explicit permission
   - Follow responsible disclosure
   - Respect privacy and data

2. **Scope Limitations**
   - Stay within defined scope
   - Avoid causing damage
   - Minimize system impact
   - Document all activities

### Tool Configuration Best Practices

1. **Proxy Settings**
   - Use separate browser profile
   - Configure SSL handling properly
   - Set appropriate timeouts
   - Monitor resource usage

2. **Project Organization**
   - Use descriptive project names
   - Organize by target or date
   - Save important configurations
   - Backup critical findings

## Lab Report Requirements

### Testing Documentation

1. **Methodology Description**
   - Tools and techniques used
   - Testing approach and scope
   - Configuration details
   - Timeline of activities

2. **Findings Documentation**
   - Vulnerability descriptions
   - Proof of concept requests
   - Impact assessment
   - Risk ratings

3. **Evidence Collection**
   - Screenshots of key findings
   - Request/response examples
   - Configuration settings
   - Error messages or outputs

### Deliverables

- Burp project files (if saved)
- Screenshots of vulnerabilities found
- Detailed findings report
- Remediation recommendations
- Lessons learned summary

## Troubleshooting Common Issues

### Proxy Problems

1. **Browser Not Using Proxy**
   - Verify proxy settings
   - Check if proxy is listening
   - Test with simple HTTP site
   - Clear browser cache

2. **SSL Certificate Issues**
   - Reinstall Burp CA certificate
   - Check certificate store
   - Verify SSL interception settings
   - Test with HTTPS sites

### Performance Issues

1. **Slow Response Times**
   - Increase timeout values
   - Reduce concurrent requests
   - Check system resources
   - Disable unnecessary features

## Next Steps

Continue with:
- **[Lab CTF Overview](lab-ctf-overview.md)** - Capture The Flag challenges
- Advanced web application testing techniques
- Automated scanning with other tools

## Additional Resources

- Burp Suite Documentation
- PortSwigger Web Security Academy
- OWASP Testing Guide
- Web application security standards
- Burp Suite extensions and plugins