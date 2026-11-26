# 13. Proof-of-Concept (PoC), Attack Chains & Root Cause Remediation

## 1. What Is a Proof-of-Concept (PoC) and Why Is It Essential?

* **Purpose:**
  Demonstrating to system administrators and developers that a vulnerability is genuinely exploitable—enabling them to reproduce the issue, understand its real-world business impact, and verify that their patch works after remediation.

* **Classic Example:**
  Launching Windows Calculator (`calc.exe`) to safely prove Remote Code Execution (RCE) on a target host without causing any damage or service disruption.

* **Two Forms of a PoC:**
  1. **Step-by-Step Documentation:** Clear written reproduction steps supported by commands and terminal screenshots.
  2. **Automated Exploit Script / Code:** A custom script (e.g., in Python) that safely triggers and demonstrates the flaw on demand.

---

## 2. The Biggest Pitfall of PoC Scripts (Important Warning)

* **"Fighting Against the Script":**
  When administrators receive an automated PoC script, they sometimes implement superficial changes just to make **that specific script fail** rather than fixing the underlying security flaw.

* **The Penetration Tester's Responsibility:**
  During the report walkthrough meeting, clearly explain to the client that a PoC script represents only **one method** of exploitation—if the underlying root cause is not remediated, an attacker will simply modify their payload or use an alternative technique to achieve the same compromise.

---

## 3. Attack Chains & Root Cause Remediation (Key Concept)

* **Attack Chain Walkthrough:**
  Including a visual flow diagram or structured walkthrough in the report showing how 3–4 seemingly minor misconfigurations were chained together to achieve full domain compromise. While breaking a single link stops that specific chain, every remaining weakness still poses a risk and must be remediated.

* **Root Cause Example (`Password123`):**
  If a Domain Admin account is compromised because its password was set to `Password123`, merely resetting that single administrator's password does not solve the problem—the true vulnerability is a **weak domain Password Policy**. Until a strong password policy is enforced globally, users and administrators across the network will continue creating weak, guessable passwords.
