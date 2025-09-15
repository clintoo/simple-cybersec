# Lab 00: Environment Setup

## Overview

This lab will guide you through setting up a cybersecurity learning environment. You'll configure virtual machines, install essential tools, and create a safe testing environment for subsequent laboratories.

## Objectives

By the end of this lab, you will have:
- Installed and configured a virtualization platform
- Set up Kali Linux for penetration testing
- Installed vulnerable applications for practice
- Configured network isolation for safe testing
- Verified tool installations and connectivity

## Prerequisites

- Computer with at least 8GB RAM (16GB recommended)
- 50GB free disk space
- Administrative access to install software
- Stable internet connection for downloads

## Part 1: Virtualization Platform Setup

### Option A: VirtualBox (Free)

1. **Download VirtualBox**
   - Visit: https://www.virtualbox.org/
   - Download latest version for your host OS
   - Download VirtualBox Extension Pack

2. **Install VirtualBox**
   ```bash
   # Linux (Ubuntu/Debian)
   sudo apt update
   sudo apt install virtualbox virtualbox-ext-pack
   
   # Windows: Run installer as administrator
   # macOS: Open .dmg file and follow instructions
   ```

3. **Configure VirtualBox**
   - Enable hardware virtualization in BIOS/UEFI
   - Increase default memory allocation
   - Configure host-only networking

### Option B: VMware Workstation (Commercial)

1. **Download VMware Workstation**
   - VMware Workstation Pro (Windows/Linux)
   - VMware Fusion (macOS)
   - Students may qualify for educational discounts

2. **Install and License**
   - Follow installation wizard
   - Enter license key or start trial
   - Configure virtual network adapters

## Part 2: Kali Linux Setup

### Download and Installation

1. **Download Kali Linux**
   ```bash
   # Download pre-built VM image (recommended)
   wget https://cdimage.kali.org/kali-2023.3/kali-linux-2023.3-vmware-amd64.7z
   
   # Or download ISO for custom installation
   wget https://cdimage.kali.org/kali-2023.3/kali-linux-2023.3-installer-amd64.iso
   ```

2. **Import/Create Virtual Machine**
   - **For pre-built image**: Extract and import VM
   - **For ISO installation**: Create new VM with 4GB RAM, 40GB disk
   - Configure VM settings:
     - Enable virtualization features
     - Allocate sufficient resources
     - Configure network adapter

3. **Initial Configuration**
   ```bash
   # Update system
   sudo apt update && sudo apt full-upgrade -y
   
   # Install additional tools
   sudo apt install -y curl wget git vim
   
   # Configure user account
   sudo passwd kali  # Set strong password
   ```

### Essential Tool Verification

1. **Network Tools**
   ```bash
   # Test network connectivity
   ping -c 4 8.8.8.8
   
   # Verify Nmap installation
   nmap --version
   
   # Test Wireshark installation
   wireshark --version
   ```

2. **Web Application Tools**
   ```bash
   # Verify Burp Suite
   which burpsuite
   
   # Test OWASP ZAP
   zaproxy --version
   
   # Verify SQLmap
   sqlmap --version
   ```

## Part 3: Vulnerable Applications Setup

### DVWA (Damn Vulnerable Web Application)

1. **Download and Setup**
   ```bash
   # Download DVWA
   git clone https://github.com/digininja/DVWA.git
   cd DVWA
   
   # Start Apache and MySQL
   sudo systemctl start apache2
   sudo systemctl start mysql
   
   # Configure database
   sudo mysql -u root -p
   CREATE DATABASE dvwa;
   CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'password';
   GRANT ALL ON dvwa.* TO 'dvwa'@'localhost';
   FLUSH PRIVILEGES;
   exit
   ```

2. **Web Configuration**
   - Copy DVWA to `/var/www/html/`
   - Configure `config/config.inc.php`
   - Access via `http://localhost/DVWA`
   - Run setup to create database tables

### Metasploitable 2

1. **Download VM**
   ```bash
   wget https://sourceforge.net/projects/metasploitable/files/Metasploitable2/metasploitable-linux-2.0.0.zip
   ```

2. **Import and Configure**
   - Extract and import VM
   - Configure host-only networking
   - Default credentials: msfadmin/msfadmin
   - Verify services are running

## Part 4: Network Configuration

### Network Isolation

1. **Create Isolated Network**
   - VirtualBox: Create Host-Only Adapter
   - VMware: Create Custom/Private Network
   - Configure IP range (e.g., 192.168.56.0/24)

2. **Configure VM Networking**
   ```bash
   # Configure static IP (example)
   sudo nano /etc/netplan/01-network-manager-all.yaml
   
   network:
     version: 2
     renderer: networkd
     ethernets:
       eth0:
         dhcp4: no
         addresses:
           - 192.168.56.10/24
         gateway4: 192.168.56.1
   
   sudo netplan apply
   ```

### Firewall Configuration

1. **Host Firewall**
   ```bash
   # Linux (UFW)
   sudo ufw enable
   sudo ufw allow from 192.168.56.0/24
   
   # Windows Firewall
   # Allow VirtualBox/VMware through firewall
   
   # macOS Firewall
   # Configure in System Preferences > Security & Privacy
   ```

## Part 5: Documentation and Backup

### Lab Documentation

1. **Create Lab Notebook**
   - Document VM configurations
   - Record network settings
   - Note installed tools and versions
   - Keep track of credentials

2. **Screenshot Important Configurations**
   - VM settings
   - Network configurations
   - Tool installations
   - Working vulnerable applications

### Backup Strategy

1. **Create VM Snapshots**
   ```bash
   # VirtualBox snapshot
   VBoxManage snapshot "Kali Linux" take "Clean Install"
   
   # VMware snapshot via GUI or CLI
   vmrun snapshot "path/to/vm.vmx" "Clean Install"
   ```

2. **Export VM Images**
   - Create OVA/OVF files for portability
   - Document export settings
   - Store in secure location

## Part 6: Verification and Testing

### Connectivity Tests

1. **Internal Network Tests**
   ```bash
   # Test host-only network connectivity
   ping 192.168.56.1  # Host machine
   ping 192.168.56.X  # Other VMs
   
   # Test service connectivity
   nmap -sT 192.168.56.X -p 80,443,3389
   ```

2. **Tool Functionality Tests**
   ```bash
   # Network scanning
   nmap -sn 192.168.56.0/24
   
   # Web application scanning
   nikto -h http://192.168.56.X/DVWA
   
   # Packet capture test
   sudo tcpdump -i eth0 -c 10
   ```

### Security Verification

1. **Isolation Verification**
   - Confirm VMs cannot reach external internet (if desired)
   - Verify host system protection
   - Test firewall rules

2. **Tool Permissions**
   ```bash
   # Verify sudo access for security tools
   sudo nmap --version
   sudo wireshark --version
   sudo tcpdump --version
   ```

## Troubleshooting

### Common Issues

1. **Virtualization Problems**
   - Enable VT-x/AMD-V in BIOS
   - Disable Hyper-V on Windows
   - Check available system resources

2. **Network Issues**
   - Verify network adapter settings
   - Check DHCP configuration
   - Confirm firewall settings

3. **Tool Installation Issues**
   ```bash
   # Update package sources
   sudo apt update
   
   # Fix broken dependencies
   sudo apt --fix-broken install
   
   # Reinstall specific tools
   sudo apt remove --purge tool-name
   sudo apt install tool-name
   ```

## Lab Submission

### Required Deliverables

1. **Environment Documentation**
   - VM specifications and configurations
   - Network topology diagram
   - Installed tools list

2. **Verification Screenshots**
   - Running Kali Linux desktop
   - Successful tool execution
   - Vulnerable application access
   - Network connectivity tests

3. **Lab Report**
   - Setup process documentation
   - Issues encountered and solutions
   - Lessons learned and improvements

### Security Considerations

- Never connect vulnerable VMs to production networks
- Use strong passwords for all accounts
- Keep host system updated and protected
- Regularly backup and update lab environment
- Follow responsible disclosure if vulnerabilities are found

## Next Steps

With your environment configured, you're ready for:
- **[Lab 01: Wireshark Basics](lab-01-wireshark-basics.md)** - Network traffic analysis
- **[Lab 02: Nmap and Scanning](lab-02-nmap-and-scanning.md)** - Network reconnaissance
- **[Lab 03: Burp Suite Introduction](lab-03-burp-intro.md)** - Web application testing

## Additional Resources

- VirtualBox User Manual
- Kali Linux Documentation
- VMware Documentation
- Cybersecurity lab setup guides
- Virtualization security best practices