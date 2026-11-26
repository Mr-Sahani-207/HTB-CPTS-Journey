# 4. Assessment Types, Perspectives & Risk Management

## 1. Vulnerability Assessment (VA) vs. Penetration Test (PT) vs. Red Team

* **Vulnerability Assessment (VA):**
  Running automated scanners (such as Nessus, Qualys, or OpenVAS) to compile a prioritized list of known weaknesses without actively exploiting the target systems.
* **Penetration Test (PT):**
  A combination of automated scanning and hands-on manual exploitation to verify vulnerabilities and demonstrate real-world business impact. *Objective:* Identify **all** exploitable flaws within the defined scope.
* **Red Team Assessment:**
  A goal-oriented, stealthy attack simulation (such as compromising a specific executive email account or reaching a critical internal server) that leverages only the specific attack paths needed to achieve the objective while evading detection.
* **The Penetration Tester's Role & Assessment Boundaries:**
  Pentesters deliver actionable reports, reproduction steps, and remediation guidance—we do **not** modify client code or apply patches directly on production systems. Additionally, a penetration test represents a **point-in-time snapshot** of an organization's security posture rather than continuous 24/7 monitoring.

---

## 2. External vs. Internal Penetration Testing

* **External Penetration Test:**
  Conducted from outside the organization's perimeter over the internet—simulating an external attacker—to evaluate perimeter defenses, public-facing servers, and web applications. Depending on client requirements, testing may be **noisy** (comprehensive) or **stealthy** (evading IDS/IPS alerts and firewall blocks).
* **Internal Penetration Test:**
  Conducted from inside the corporate network—either following a simulated perimeter breach or under an **Assumed Breach** model—to evaluate lateral movement, internal segmentation, and privilege escalation paths.

---

## 3. Engagement Types (Based on Information Provided)

| Type | Information Provided | Testing Approach |
| :--- | :--- | :--- |
| **Blackbox** | Minimal (only target IP addresses or domain names) | Simulates an unauthenticated external attacker; requires the most reconnaissance time |
| **Greybox** | Extended (target URLs, subnets, network diagrams, or standard user credentials) | Most common engagement model; balances reconnaissance speed with technical depth |
| **Whitebox** | Maximum (full source code, architecture diagrams, admin credentials, and configurations) | Deepest security audit; effective for uncovering code-level and business logic flaws |
| **Red-Teaming** | Stealthy, objective-driven attack (may include social engineering or physical testing) | Evaluates the Blue Team's detection, alerting, and incident response capabilities |
| **Purple-Teaming** | Collaborative exercise between Red and Blue teams | Executes attacks transparently to tune defender alerts, SIEM rules, and detection logic in real time |

---

## 4. Risk Management & Legal Considerations

* **Four Core Risk Management Strategies:**
  Organizations handle identified risks by choosing to **Accept** (tolerate the risk), **Transfer** (shift financial impact via cyber insurance or third-party contracts), **Avoid** (decommission the vulnerable service), or **Mitigate** (implement security controls and patches).
* **Third-Party & Cloud Hosting Policies:**
  If target infrastructure is hosted on cloud providers (such as AWS, Azure, or GCP) or managed third-party platforms, testers must verify the provider's Acceptable Use Policy and authorization rules prior to testing.
* **Data Privacy & Confidentiality:**
  Any sensitive information encountered during testing (such as employee payroll records, credit card data, or Personally Identifiable Information) must be handled securely, minimized in evidence screenshots, and kept strictly confidential.
