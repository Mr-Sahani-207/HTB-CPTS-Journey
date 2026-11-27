### Lab 1 — Web Service & Version Enumeration

* **Objective:** Identify the web service running on a non-standard HTTP port (`8080`) using Nmap service version detection.

* **Steps & Commands:**
  1. **Targeted Service Version Scan:**
     ```bash
     nmap -sV -p 8080 <TARGET_IP>
     ```
     * **Finding:** Discovered `8080/tcp` open and identified the running web server (`[REDACTED]`), answering the lab challenge.

* **Takeaway:** Web servers and application containers frequently listen on alternate ports such as `8080` or `8443`; running targeted version scans (`-sV`) quickly identifies the underlying web technology for further directory and vulnerability enumeration.

* ### Lab 2 — Full TCP Port Enumeration

* **Objective:** Identify a service running on a non-standard port by performing a full TCP port scan across all 65,535 ports.
* **Steps & Commands:**
  1. **Full Port & Service Scan:**
     ```bash
     nmap -sV -p- <TARGET_IP>
     ```
     * **Finding:** Identified an open non-standard TCP port (`[REDACTED]/tcp`) running the target service, answering the lab challenge.
* **Takeaway:** Default Nmap scans only check the top 1,000 common ports; always run a full port scan (`-p-`) to uncover services hidden on high or non-standard ports.

* ### Lab 3 — Authenticated SMB Share Enumeration & File Retrieval

* **Objective:** Identify exposed SMB services via full-port scanning, authenticate using provided credentials (`bob`), and extract sensitive files from restricted share directories.

* **Steps & Commands:**
  1. **Full TCP Port Scan:**
     ```bash
     nmap -sV -p- <TARGET_IP>
     ```
     * **Finding:** Discovered port `445/tcp` (`SMB`) open on the target host.

  2. **SMB Share Listing:**
     ```bash
     smbclient -N -L //<TARGET_IP>
     ```
     * **Finding:** Identified the `users` disk share (anonymous access denied).

  3. **Authenticated SMB Access & File Extraction:**
     ```bash
     smbclient -U bob //<TARGET_IP>/users
     ```
     * Authenticated as **`bob`** (password: `[REDACTED]`) and discovered two directories inside the share: `bob` and `flag`.
     * Navigated into both directories (`cd bob` and `cd flag`) and downloaded `password.txt` and `flag.txt` using `get` to retrieve the final flag (`HTB{REDACTED}`).

* **Takeaway:** Always inspect all accessible subdirectories within an SMB share once authenticated—sensitive credential files and flags are frequently stored in separate user or administrative folders.
