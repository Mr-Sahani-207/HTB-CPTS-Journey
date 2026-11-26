# 13. Assessment Methodologies & Phased Nmap Scanning Strategy

## 1. Penetration Testing Assessment Approaches

* **Black-Box Testing:** The tester starts with zero internal knowledge of the target environment (typically provided only with an organization name or a single IP address), simulating an external attacker performing reconnaissance from scratch.
* **Grey-Box Testing:** The tester is provided with partial prior knowledge (such as the underlying OS being Linux, a low-privilege user account, or general application context). For example, the *Nibbles* lab is categorized as Grey-Box because the target OS (Linux) and primary web attack surface are known upfront.
* **White-Box Testing:** The tester is granted full internal access, including administrative credentials, network architecture diagrams, configuration files, and application source code.

---

## 2. Systematic Phased Nmap Scanning Strategy

Rather than running noisy, unorganized scans, a structured 5-step Nmap methodology ensures thorough port coverage while maintaining clean documentation:

1. **Step 1: Quick Top 1,000 Ports & Service Version Scan**
   ```bash
   nmap -sV --open -oA initial_scan <TARGET_IP>
   ```
   * **Objective:** Rapidly identify open ports among the top 1,000 most common ports and fingerprint running services.
   * **Findings:**
     * **Port 22 (`TCP`):** OpenSSH (Ubuntu)
     * **Port 80 (`TCP`):** Apache HTTP Server

2. **Step 2: Evidence Logging & Output Archiving (`-oA`)**
   * Using `-oA <filename>` saves scan results simultaneously in all three Nmap formats (`.nmap`, `.gnmap`, and `.xml`), preserving verifiable evidence for reporting and note-taking.

3. **Step 3: Background Full TCP Port Scan (`-p-`)**
   ```bash
   nmap -p- --open -oA full_tcp_scan <TARGET_IP>
   ```
   * **Objective:** Scan all remaining ports (`1–65535`) in the background (e.g., inside a `tmux` pane) so no non-standard or hidden services are missed while initial web enumeration begins.
   * **Findings:** Confirmed no additional open TCP ports beyond `22` and `80`.

4. **Step 4: Targeted Default Script Scan (`-sC`)**
   ```bash
   nmap -sC -p 22,80 -oA script_scan <TARGET_IP>
   ```
   * **Objective:** Once the exact open ports (`22` and `80`) are confirmed, run Nmap's default NSE scripts exclusively against those ports to avoid generating unnecessary traffic across closed ports.

5. **Step 5: Automated Web Enumeration Script (`--script=http-enum`)**
   ```bash
   nmap -p 80 --script=http-enum -oA http_enum_scan <TARGET_IP>
   ```
   * **Objective:** Leverage Nmap's built-in web directory enumeration script to check for common paths before moving to dedicated fuzzers like `gobuster`.

---

## 3. Attack Path Summary & Next Steps

Based on the completed Nmap reconnaissance, the engagement narrows down to two clear paths:
1. **Port 22 (SSH):** Requires valid credentials or a discovered SSH private key before direct authentication can be attempted.
2. **Port 80 (HTTP):** Serves as the primary attack surface for deeper web enumeration and gaining an initial foothold.
