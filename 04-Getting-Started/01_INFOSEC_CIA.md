# 1. InfoSec CIA Triad & Risk Management Framework

## Part 1: The CIA Triad

### 1. Confidentiality (Privacy)
Confidentiality ensures that sensitive data is accessible only to authorized individuals and remains protected from unauthorized disclosure.

* **Common Threats:** Man-in-the-Middle (MITM) attacks, credential theft, packet sniffing, and unauthorized data leaks.
* **Defenses & Controls:**
  * **Encryption:** Encrypting data in transit (TLS/HTTPS) and data at rest (AES).
  * **Access Control:** Implementing Role-Based Access Control (RBAC) and the Principle of Least Privilege.
  * **Authentication:** Enforcing Multi-Factor Authentication (MFA).

### 2. Integrity (Data Accuracy & Trustworthiness)
Integrity guarantees that data arrives at its destination exactly as it was generated, preventing unauthorized tampering or modification during storage or transit.

* **Common Threats:** Database modification via SQL Injection, MITM packet tampering, and altering or deleting log files.
* **Defenses & Controls:**
  * **Hashing:** Verifying SHA-256 and MD5 checksums to detect unauthorized file changes.
  * **Digital Signatures & Certificates:** Validating the authenticity of both the sender and the data.
  * **Version Control & Audit Logs:** Maintaining a record of every modification.

### 3. Availability (Uptime & Accessibility)
Availability ensures that systems and data remain operational and accessible 24/7 whenever authorized users require them. Regardless of how confidential or accurate data is, it holds no value if it cannot be accessed.

* **Common Threats:** Denial of Service (DoS/DDoS) attacks, hardware failures, power outages, and ransomware lockouts.
* **Defenses & Controls:**
  * **Redundancy & Failover:** Utilizing backup servers, load balancers, and redundant network links.
  * **DDoS Mitigation:** Deploying anti-DDoS firewalls, rate limiting, and services like Cloudflare.
  * **Disaster Recovery:** Maintaining regular automated backups and incident response plans.

---

## Part 2: Risk Management Framework (5 Core Steps)

1. **Identify the Risk**
   * Spot risks across every business exposure category: legal, environmental, market, regulatory, operational, and cyber threats.
   * **Objective:** Map organizational weak points and external exposure surfaces.

2. **Analyze the Risk**
   * Calculate the **Impact** and **Probability (Likelihood)** of each identified risk.
   * Map risks against company policies, procedures, and business workflows to determine exact exposure.

3. **Evaluate the Risk**
   * Rank and prioritize risks based on severity (High, Medium, Low).
   * Select one of the **4 Core Risk Treatment Strategies**:
     * **Accept:** Acknowledge unavoidable risks necessary for business operations.
     * **Avoid:** Eliminate the threat by modifying plans or discontinuing the risky process.
     * **Control (Mitigate):** Deploy security controls to reduce the likelihood or potential damage.
     * **Transfer:** Shift financial or operational liability via cyber insurance or third-party vendors.

4. **Treat / Deal with the Risk**
   * Eliminate or contain the risk within acceptable limits.
   * Coordinate directly with system and process stakeholders to deploy effective countermeasures.

5. **Monitor the Risk**
   * Conduct continuous audits and ongoing tracking.
   * Monitor situational changes (such as newly discovered exploits or network topology updates) that could escalate a risk from Low to Medium or High impact.
