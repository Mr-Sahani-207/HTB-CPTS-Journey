# 7. Service Scanning & Enumeration: Nmap, FTP, SMB, and SNMP

## Part 1: Detecting Outdated Software Versions with Nmap

Identifying whether a port is open or closed is only the first step. Determining **which exact software and version** is listening behind that port is known as **Service Scanning / Version Detection**.

In real-world penetration testing, vulnerabilities rarely stem from the port number itself—they exist due to **outdated or misconfigured software versions** running on those ports.

### Step 1: Core Nmap Enumeration Command
```bash
nmap -sV -sC -p- -Pn <TARGET_IP>
