# Hack The Box — Starting Point: Meow (Write-up)

- **Target Name:** Meow
- **Platform:** Hack The Box (Starting Point — Tier 0)
- **OS:** Linux (Ubuntu)
- **Difficulty:** Very Easy
- **Vulnerability:** Unauthenticated Telnet Access (Misconfigured Root Login)

---

## 1. Executive Summary
During the security assessment of the target machine **Meow**, an exposed **Telnet** service was identified running on TCP port `23`. Further enumeration revealed that the `root` account allowed passwordless authentication, resulting in full administrative (`root`) compromise of the system.

---

## 2. Reconnaissance & Enumeration

### Step 1: Connectivity Verification
Verified active VPN tunnel connectivity to the target IP (`10.129.155.199`):
```bash
ping -c 3 10.129.155.199
```

### Step 2: Port & Service Scanning (Nmap)
Performed a service version and default script scan using Nmap:
```bash
nmap -sV -sC 10.129.155.199
```

**Scan Results:**
| Port | State | Service | Version |
| :--- | :--- | :--- | :--- |
| 23/tcp | open | telnet | Linux telnetd |

---

## 3. Exploitation

Since **Telnet** transmits data in plaintext and is often prone to weak authentication configurations, a connection attempt was initiated using the `telnet` client:
```bash
telnet 10.129.155.199
```

At the `Meow login:` prompt, tested default administrative usernames (`admin`, `administrator`, `root`). Logging in as **`root`** required no password and immediately granted a root shell:
```text
Meow login: root
Last login: Sat Nov 28 06:16:01 UTC 2026 on pts/0
root@Meow:~#
```

---

## 4. Post-Exploitation (Flag Retrieval)
Listed the files inside `/root` and read `flag.txt`:
```bash
ls
cat flag.txt
```
- **Root Flag:** `<REDACTED>`

---

## 5. Remediation & Security Recommendations
1. **Disable Telnet:** Replace Telnet (port 23) with encrypted **SSH** (port 22) for remote administration.
2. **Disable Direct Root Login:** Restrict direct `root` login over remote protocols and enforce standard user authentication with `sudo` privileges.
3. **Enforce Strong Authentication:** Require strong passwords or SSH key-based authentication for all system accounts.
