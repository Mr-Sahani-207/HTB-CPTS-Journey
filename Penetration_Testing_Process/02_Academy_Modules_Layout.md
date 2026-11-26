# 2. The 8 Stages of Penetration Testing & Core Rules

## 1. The 8 Stages of Penetration Testing (Sequential Workflow)

1. **Pre-Engagement:**
   Signing a written **Scope of Work (SoW)**, legal contracts, and Rules of Engagement before starting any testing—explicitly defining authorized target IPs/domains and permitted testing activities.

2. **Information Gathering (Reconnaissance):**
   Identifying target systems, subdomains, open ports, and running services. *Rule: Complete thorough reconnaissance first—never jump straight into exploitation blindly.*

3. **Vulnerability Assessment:**
   Analyzing gathered reconnaissance data to identify security weaknesses using a combination of automated scanners and manual verification.

4. **Exploitation:**
   Leveraging discovered vulnerabilities to gain an initial foothold on the target system or web application.

5. **Post-Exploitation (Pillaging & Privilege Escalation):**
   Extracting sensitive local information from the compromised host (pillaging) and elevating privileges from a standard low-privilege user to **`root` / `Administrator`**.

6. **Lateral Movement:**
   Using an initially compromised host as a pivot point to reach and compromise additional systems deeper inside the internal network via pivoting or tunneling.

7. **Proof-of-Concept (PoC):**
   Documenting clear, reproducible steps or writing a clean automation script (e.g., in Python) to prove that a vulnerability genuinely exists and help system administrators verify and patch it.

8. **Post-Engagement (Cleanup & Reporting):**
   Removing all uploaded payloads, shells, and temporary tools from target systems (**cleanup**) to leave the environment secure, followed by delivering a clear, detailed final penetration test report to the client.

---

## 2. Four Golden Rules to Always Remember

* **Penetration Testing Is a Loop, Not a Straight Line:**
  Every time you compromise a new host (after Exploitation or Lateral Movement), you must cycle back through **Local Information Gathering $\rightarrow$ Vulnerability Assessment** for that new environment.

* **Pillaging Is Mandatory:**
  Before attempting privilege escalation, thoroughly enumerating the compromised system (local users, configuration files, cron jobs, internal services, and credentials) is essential—not optional.

* **Never Forget Cleanup:**
  Leaving a bind shell, web shell, or backdoor file on a client server exposes them to real-world attackers. The final report must also include an inventory of all system modifications and uploaded artifacts.

* **Adopt a "Question Everything" Mindset:**
  Do not rely blindly on automated tools. Understanding how underlying technologies work under the hood (Networking, Linux, Windows, and Web internals) is what separates strong penetration testers from tool operators.
