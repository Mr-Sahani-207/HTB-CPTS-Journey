# 10. Public Exploits & Exploit Research

## Point 1: Understanding Public Exploits

**Public Exploits** are ready-made exploit scripts or Proof-of-Concept (PoC) code shared publicly by security researchers, ethical hackers, or developers to demonstrate, verify, or test a known security vulnerability (CVE).

When a flaw is discovered in a software product, researchers often publish PoC exploit methods after responsible disclosure so administrators can verify exposure and apply patches.

### Popular Sources for Public Exploits
* **Exploit-DB (Offensive Security):** One of the largest public archives of verified exploit scripts (`.py`, `.rb`, `.c`, `.sh`).
* **Searchsploit:** The offline command-line archive of Exploit-DB pre-installed on Kali Linux.
* **GitHub:** Researchers frequently publish PoC repositories for newly disclosed CVEs here first.
* **Packet Storm Security:** A public portal hosting exploits, security advisories, and penetration testing utilities.
* **Rapid7 Vulnerability Database:** A curated repository of Metasploit modules and vulnerability advisories.

### Basic Exploit Research Workflow
1. **Enumerate Target:** Identify the exact software name and version running on the target (e.g., `Simple Backup 2.7.10`).
2. **Search Database:** Query offline or online exploit databases using the software name and version:
   ```bash
   searchsploit "<software_name> <version>"
   ```
3. **Analyze Code:** Inspect the exploit source code to understand its logic, dependencies, and required inputs before executing it:
   ```bash
   searchsploit -x <EDB-ID>
   ```
4. **Execute & Verify:** Supply the required arguments (target URL, payload, path, or parameters) to verify whether the target is vulnerable.

---

## Point 2: Practical Walkthrough — Exploiting a Vulnerable Web Plugin

### Step 1: Target Enumeration
* Navigated to `http://<TARGET_IP>:<PORT>` in a web browser.
* Inspecting the site content revealed the exact installed plugin and version: **`Simple Backup Plugin 2.7.10`**.

### Step 2: Searching for a Public Exploit
* Queried the local Exploit-DB archive in Kali Linux:
  ```bash
  searchsploit -t "simple backup"
  ```
* Identified a matching Path Traversal exploit script:
  * **File Path:** `/usr/share/exploitdb/exploits/php/webapps/51937.txt`

### Step 3: Preparing the Exploit Script
* Copied the exploit file into the working directory as an executable Python script:
  ```bash
  cp /usr/share/exploitdb/exploits/php/webapps/51937.txt exploit.py
  ```

### Step 4: Checking Script Usage & Arguments
* Executed the script without arguments to inspect its expected CLI syntax:
  ```bash
  python3 exploit.py
  ```
* The usage instructions required three arguments: `<target_url>`, `<file_name>`, and `<depth>`.

### Step 5: Troubleshooting & Arbitrary File Read Verification
* Passing a traversal string (`/../../../../`) as the third argument triggered a `ValueError` because the script expected an integer (`int(sys.argv[3])`) specifying directory traversal depth.
* Re-running the exploit with the proper integer depth (`5`) successfully traversed the directory structure and retrieved the target file:
  ```bash
  python3 exploit.py http://<TARGET_IP>:<PORT> <TARGET_FILE> 5
  ```
* **Result:** Target file contents retrieved (`<REDACTED>`).
