# 8. Information Gathering Categories & Pillaging

## 1. The Four Main Categories of Information Gathering

Every comprehensive penetration test covers four core reconnaissance categories:

1. **Open-Source Intelligence (OSINT):**
   * Collecting publicly accessible data from across the internet—including the target company's website, social media profiles, job postings, public **GitHub** repositories, and **StackOverflow** code snippets.
   * Developers occasionally leak **hardcoded passwords, API tokens, or private SSH keys** in public repositories. If a critical exposure is found during initial reconnaissance, follow the **Incident Handling** procedure defined in the Rules of Engagement (RoE) and notify the client immediately.

2. **Infrastructure Enumeration:**
   * Mapping the target organization's external and internal network footprint—identifying **DNS servers, mail servers, web servers, and cloud instances** along with their IP addresses, and cross-checking every asset against the authorized **Scope**.
   * Identifying perimeter security controls (**Firewalls, WAFs, IDS/IPS**) so testing traffic can be adapted for stealth and evasion when required.
   * Building an accurate list of internal hosts and user accounts to support targeted techniques such as **Password Spraying** (testing a single common password across multiple usernames).

3. **Service Enumeration:**
   * Identifying every network service listening on each host along with its **exact software version**.
   * Outdated service versions frequently contain known public vulnerabilities, as administrators often delay updates on production systems to avoid breaking critical operations.

4. **Host Enumeration:**
   * Inspecting individual servers and endpoints within scope—determining the **Operating System (OS)**, open ports, and each host's operational role in the network (such as an FTP server permitting anonymous access or an end-of-life OS).
   * **Internal Host Enumeration:** Once a machine is compromised, enumerating local files, scheduled scripts, and internal services—which feeds directly into **Post-Exploitation and Privilege Escalation**.

---

## 2. What Is Pillaging? (Key Concept)

* **Definition:** **Pillaging** is the process of systematically gathering sensitive local information—such as employee names, cached credentials, configuration files, SSH keys, and database records—from inside a compromised system after gaining initial access.
* **Key Fact:** Pillaging is not a standalone stage of the penetration testing lifecycle; rather, it is the local information-gathering activity performed during **Post-Exploitation** and **Privilege Escalation** that provides the credentials and intelligence required for **Lateral Movement** into other internal systems.
