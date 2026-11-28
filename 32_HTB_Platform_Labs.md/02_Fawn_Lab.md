# Hack The Box — Starting Point: Fawn (Write-up)

- **Target Name:** Fawn
- **Platform:** Hack The Box (Starting Point — Tier 0)
- **OS:** Linux (Ubuntu)
- **Difficulty:** Very Easy
- **Vulnerability:** Anonymous FTP Login Misconfiguration

---

## 1. Executive Summary
During the security assessment of the target machine **Fawn**, an exposed **FTP (File Transfer Protocol)** service (`vsftpd 3.0.3`) was identified running on TCP port `21`. Further enumeration revealed that **Anonymous FTP authentication** was enabled, allowing unauthorized access to retrieve sensitive files (`flag.txt`) without valid credentials.

---

## 2. Reconnaissance & Enumeration

### Step 1: Connectivity Verification
Verified active VPN tunnel connectivity to the target IP (`10.129.127.22`):
```bash
ping -c 3 10.129.127.22
```

### Step 2: Port & Service Scanning (Nmap)
Performed a service version and default script scan using Nmap:
```bash
nmap -sV -sC 10.129.127.22
```

**Scan Results:**
| Port | State | Service | Version |
| :--- | :--- | :--- | :--- |
| 21/tcp | open | ftp | vsftpd 3.0.3 |

---

## 3. Exploitation

Since **FTP** on port `21` often permits unauthenticated access when misconfigured, connected to the FTP server using the `anonymous` user (or retrieved the file directly via `curl` over FTP):

### Method 1: Interactive FTP Client
```bash
ftp 10.129.127.22
```
- **Username:** `anonymous`
- **Password:** *(Blank / Enter)*
- **Server Response:** `230 Login successful.`

Listed directory contents and downloaded the flag:
```text
ls
get flag.txt
bye
```

### Method 2: Non-Interactive Retrieval via `curl`
```bash
curl ftp://anonymous:@10.129.127.22/flag.txt -o flag.txt && cat flag.txt
```

---

## 4. Post-Exploitation (Flag Retrieval)
Read the downloaded `flag.txt` file on the local machine:
```bash
cat flag.txt
```
- **Root Flag:** `<REDACTED>`

---

## 5. Remediation & Security Recommendations
1. **Disable Anonymous FTP Access:** Set `anonymous_enable=NO` inside `/etc/vsftpd.conf` and restart the `vsftpd` service.
2. **Migrate to SFTP:** Replace plaintext FTP (port 21) with encrypted **SFTP** over SSH (port 22) to protect credentials and file transfers in transit.
3. **Restrict Network Exposure:** Use firewall rules to limit file transfer access strictly to authorized IP addresses.
