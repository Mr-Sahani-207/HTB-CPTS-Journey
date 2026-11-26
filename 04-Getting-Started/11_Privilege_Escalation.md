# 11. Privilege Escalation (Linux PrivEsc)

**Privilege Escalation (PrivEsc)** is the post-exploitation phase of a penetration test where you have already gained a low-privilege foothold on a target machine (such as `www-data` or a standard user) and aim to elevate your access to the highest administrative level—`root` on Linux or `Administrator` / `NT AUTHORITY\SYSTEM` on Windows.

---

## Part 1: Types of Privilege Escalation

* **Vertical Escalation (Privilege Elevation):** Elevating access directly from a low-privilege user account to a high-privilege administrative account (`root` or `NT AUTHORITY\SYSTEM`).
* **Horizontal Escalation (Lateral Movement):** Compromising another user account at the same privilege tier (e.g., pivoting from `user1` to `user2`) that possesses better sudo permissions, group memberships, or file access.

---

## Part 2: Five Core Pillars of Linux Privilege Escalation

### 1. Sudo Misconfigurations (`sudo -l`)
* Always the first manual check after gaining a shell—identifies commands the current user is permitted to execute as `root` or another user without a password (`NOPASSWD`).
* If a standard binary (such as `vim`, `find`, `nmap`, or `php`) is permitted via `sudo`, consult **GTFOBins** to abuse its built-in functionality and spawn an elevated shell.

### 2. SUID / SGID Binaries
* Files configured with the SUID bit execute with the privileges of the file owner (frequently `root`) regardless of who runs them:
  ```bash
  find / -perm -4000 -type f 2>/dev/null
  ```
* Custom scripts or non-standard binaries with the SUID bit set can often be exploited via environment variables, path manipulation, or command arguments.

### 3. Scheduled Tasks (Cron Jobs)
* Automated background jobs scheduled to run periodically with `root` privileges:
  ```bash
  cat /etc/crontab
  ls -la /etc/cron.*
  ```
* If a script executed by a root cron job is world-writable or located in a writable directory, modifying it grants root code execution.

### 4. Kernel Exploits
* Vulnerabilities in outdated Linux kernel versions (e.g., Dirty COW, PwnKit):
  ```bash
  uname -a
  ```
* *Operational Note:* In real-world assessments, kernel exploits carry a high risk of system instability or kernel panic (crash), so they are only considered after exhausting configuration-based vectors.

### 5. Internal Services & Hardcoded Credentials
* Enumerate internal services bound exclusively to localhost (`127.0.0.1`):
  ```bash
  ss -tulpn
  ```
* Inspect configuration files (`wp-config.php`, `.env`), backup files, and shell history files (`.bash_history`) for cleartext passwords or API keys.

---

## Part 3: Practical Walkthrough — Multi-Stage Escalation (`user1` $\rightarrow$ `user2` $\rightarrow$ `root`)

### Step 1: Lateral Movement (`user1` $\rightarrow$ `user2`)
* **Enumeration:** Checked sudo privileges while logged in as `user1`:
  ```bash
  sudo -l
  ```
* **Vulnerability Identified:** The output revealed `(user2 : user2) NOPASSWD: /bin/bash`, allowing `user1` to spawn a Bash shell as `user2` without a password.
* **Execution:** Switched user context to `user2`:
  ```bash
  sudo -u user2 /bin/bash
  ```
* **Milestone:** Gained access as `user2` and verified read access to `user2`'s home directory (`/home/user2/flag.txt` — `<REDACTED>`).

### Step 2: Post-Lateral Enumeration (`user2` Context)
* **Sudo Vector Exhausted:** Running `sudo -l` as `user2` prompted for a password, ruling out direct sudo abuse.
* **SUID Enumeration:** Scanned the filesystem for SUID binaries:
  ```bash
  find / -perm -4000 -type f 2>/dev/null
  ```
  Only standard system binaries were returned, with no exploitable custom SUID executables.
* **Misconfigured File Permissions:** Further filesystem enumeration revealed weak directory permissions on `/root/.ssh/`, allowing `user2` to read root's private SSH key.

### Step 3: Vertical Privilege Escalation (`user2` $\rightarrow$ `root`)
* **Private Key Extraction:** Read the contents of root's private SSH key:
  ```bash
  cat /root/.ssh/id_rsa
  ```
* **Key Staging on Attack Box (Kali):** Saved the extracted private key locally:
  ```bash
  nano /tmp/root_key
  ```
* **Permission Hardening:** Restricted file permissions to owner-only (`600`), as required by the SSH client:
  ```bash
  chmod 600 /tmp/root_key
  ```
* **Root Login via SSH Identity File:** Authenticated directly as `root` using the `-i` identity flag:
  ```bash
  ssh -i /tmp/root_key root@<TARGET_IP> -p <PORT>
  ```
* **Milestone:** Achieved full `root` access and verified `/root/flag.txt` (`<REDACTED>`).
