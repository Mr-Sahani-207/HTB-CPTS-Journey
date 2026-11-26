# 9. Banner Grabbing

Banner grabbing is performed to extract software names and exact version details across network services (such as HTTP, FTP, SSH, and SMTP) using standard networking and reconnaissance utilities:

### 1. `curl` (HTTP / Web Services)
The most common and lightweight command-line utility for inspecting HTTP response headers from web servers.
* **Purpose:** Retrieving the `Server` header (e.g., `Server: Apache/2.4.18`), backend runtime headers (`X-Powered-By`), and session cookies.
* **Syntax:**
  ```bash
  curl -I http://<target-ip>
  ```
  *(The `-I` flag sends an HTTP `HEAD` request to fetch only the response headers.)*

### 2. `netcat` (`nc`) (Raw TCP/UDP Services)
Netcat connects directly to an open port and reads the initial welcome banner returned by the listening service.
* **Purpose:** Probing FTP (port `21`), SSH (port `22`), SMTP (port `25`), or custom TCP services.
* **Syntax:**
  ```bash
  nc -vn <target-ip> 21
  ```

### 3. `telnet`
Similar to Netcat, Telnet establishes an interactive connection over a raw TCP socket.
* **Purpose:** Manually interacting with mail servers (SMTP/POP3/IMAP) or requesting HTTP headers directly.
* **Syntax:**
  ```bash
  telnet <target-ip> 80
  ```

### 4. `nmap` (Network Scanner & Scripting Engine)
Nmap is the industry-standard tool for automated service fingerprinting and banner enumeration.
* **Version Detection (`-sV`):** Identifies the exact software and version running on open ports.
  ```bash
  nmap -sV -p 80,22,21 <target-ip>
  ```
* **NSE Banner Script (`--script banner`):** Uses the Nmap Scripting Engine to explicitly grab service banners across ports.
  ```bash
  nmap -sC --script banner -p- <target-ip>
  ```

### 5. `WhatWeb` (Web Application Fingerprinting)
Goes beyond basic HTTP server headers to identify embedded web technologies, Content Management Systems (CMS), JavaScript frameworks, and analytics widgets.
* **Purpose:** Fingerprinting web servers, CMS versions (WordPress, Drupal, Joomla), and backend libraries.
* **Syntax:**
  ```bash
  whatweb http://<target-ip>
  ```

### 6. `openssl s_client` (Encrypted / SSL-TLS Services)
Plaintext tools like `nc` or `telnet` cannot read raw text over SSL/TLS-encrypted ports such as HTTPS (`443`) or SMTPS (`465`/`587`).
* **Purpose:** Inspecting SSL/TLS certificates and reading service banners after completing the TLS handshake.
* **Syntax:**
  ```bash
  openssl s_client -connect <target-ip>:443
  ```

### 7. `nikto` (Web Server Reconnaissance)
Automatically scans web servers to identify server banners, misconfigurations, dangerous files, and missing security headers.
* **Syntax:**
  ```bash
  nikto -h http://<target-ip>
  ```

### Quick Comparison

| Tool | Primary Use Case | Output Type |
| :--- | :--- | :--- |
| **`curl`** | HTTP/HTTPS headers | Clean HTTP response headers |
| **`netcat` / `telnet`** | Raw TCP services (FTP, SSH, SMTP) | Direct welcome/service banner |
| **`nmap -sV`** | Port scan + service fingerprinting | Comprehensive service and version list |
| **`WhatWeb`** | Web applications & CMS | Rich metadata (CMS, plugins, server stack) |
| **`openssl s_client`** | Encrypted TLS/SSL ports | SSL certificate details + decrypted headers |
