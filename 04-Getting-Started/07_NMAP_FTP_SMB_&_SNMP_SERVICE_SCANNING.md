# 7. Nmap Service Scanning, FTP, SMB & SNMP Enumeration

## Point 1: Searching Outdated Software Versions Using Nmap

Identifying whether a port is open or closed is not enough. Determining **which specific software and exact version** is running behind that port is called **Service Scanning / Version Detection**. A vulnerability on a target does not exist because of the port number itself, but because of an **outdated software version** running on that port.

**Step 1: Core Command**
```bash
nmap -sV -sC -p- -Pn <TARGET_IP>
```
* **`-sV`:** **Version Detection** — Checks the exact service name and software version (e.g., `Apache 2.4.41` or `OpenSSH 8.2p1`).
* **`-sC`:** **Default Scripts** — Runs Nmap's built-in NSE scripts for banner grabbing and basic enumeration.
* **`-p-`:** **All Ports** — Scans all `65,535` ports (hidden admin portals often run on non-standard ports).
* **`-Pn`:** **No Ping** — Skips ICMP ping discovery and scans the target directly so firewalls do not block the scan.

**Step 2: What a Pentester Looks for in the Output**
```text
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
80/tcp open  http    Apache httpd 2.4.49
```
A beginner only notices that FTP and HTTP are open, whereas a pentester immediately extracts **Software + Version**:
1. `vsftpd 2.3.4` $\rightarrow$ Backdoor vulnerability.
2. `Apache 2.4.49` $\rightarrow$ Path Traversal / Remote Code Execution (RCE).

**Step 3: Vulnerability Search**
```bash
searchsploit vsftpd 2.3.4
searchsploit apache 2.4.49
```
1. Identify the **software and version** (e.g., `Apache 2.4.49`).
2. Search `searchsploit` or Google for known public vulnerabilities affecting that exact version.
3. If an exploit exists, it becomes your **Initial Foothold** into the target system.

## Point 2: FTP (File Transfer Protocol) — Port 21

FTP is used to upload and download files between a client and server. In pentesting and CPTS labs, FTP rarely gives direct shell access—instead, it exposes **sensitive files** (configuration files, passwords, backups, or SSH keys).

**Step 1: First Move — Anonymous Login**
```bash
ftp <TARGET_IP>
```
* **Name / Username:** `anonymous`
* **Password:** Press `Enter` (or type `anonymous`)
* If the server responds with **`230 Login successful`**, you are logged in without credentials.

**Step 2: Essential FTP Commands**

| FTP Command | Meaning | Function |
| :--- | :--- | :--- |
| `ls` / `dir` | List files | Lists available files and directories on the server |
| `cd <folder>` | Change directory | Navigates inside a remote directory |
| `binary` | Set transfer mode | Switches transfer mode to Binary to prevent corrupted downloads |
| `get <filename>` | Download | Downloads a file from the server to your Kali machine |
| `mget *` | Multi-download | Downloads all files at once (disable confirmation prompts using `prompt`) |
| `put <filename>` | Upload | Uploads a local file (e.g., a reverse shell) to the server if write access is permitted |
| `exit` / `bye` | Quit | Exits the FTP session |

**Step 3: Quick Check with Nmap Scripts**
```bash
nmap -p 21 --script ftp-anon,ftp-syst <TARGET_IP>
```
* If anonymous login is enabled, Nmap prints the directory listing directly in the scan output.

**Step 4: High-Value Files to Look for on FTP**
1. **`.txt`, `.pdf`, `.docx` files:** Often contain admin notes, employee lists, or plaintext passwords.
2. **Backup files (`backup.zip`, `.bak`, `.sql`):** Frequently leak database credentials and source code.
3. **Web Directory Mapping:** Sometimes the FTP root points directly to the web server directory (`/var/www/html`). If **upload permissions** are enabled, uploading a PHP reverse shell and triggering it via a web browser gives **direct shell access**.

## Point 3: SMB & SNMP Enumeration Cheatsheet

**1. SMB (Ports 139, 445)** — *Target: OS Detection, Shared Folders, Sensitive Credential Files.*
```bash
# Detect target OS and computer name
nmap --script smb-os-discovery.nse -p445 <TARGET_IP>

# 1. List shares (Anonymous / No Password)
smbclient -N -L //<TARGET_IP>

# 2. Connect to a share (Guest / No Password)
smbclient -N //<TARGET_IP>/<SHARE_NAME>

# 3. Connect to a share (With Credentials)
smbclient -U <USERNAME> //<TARGET_IP>/<SHARE_NAME>
```
* *Tip:* If Windows 7 or a legacy Windows version is detected, check for the **MS17-010 (EternalBlue)** exploit.
* *Interactive Commands:* `ls` (list directory contents), `cd <folder>` (navigate into folder), `get <file>` (download file to Kali Linux).

**2. SNMP (Port 161 UDP)** — *Target: Default Community Strings, System Info, Command-Line Passwords.*
* SNMP v1/v2c operates without traditional passwords and authenticates solely via a **Community String**. Default strings to test first: `public` (Read-Only) and `private` (Read/Write).
```bash
# Extract hostname / basic system info (v2c + public string)
snmpwalk -v 2c -c public <TARGET_IP> 1.3.6.1.2.1.1.5.0

# Dump the full MIB tree (running processes, network interfaces, installed software versions)
snmpwalk -v 2c -c public <TARGET_IP>
```
