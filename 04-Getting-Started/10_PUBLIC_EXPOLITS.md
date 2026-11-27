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

