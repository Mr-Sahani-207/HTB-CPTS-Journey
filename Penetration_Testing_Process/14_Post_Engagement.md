# 14. Post-Engagement Phase — Cleanup, Reporting & Retesting

## 1. Six Essential Steps of Post-Engagement

1. **Cleanup:**
   Removing all uploaded tools, shells, scripts, and temporary administrative accounts created on target servers during testing. If any artifact cannot be deleted safely, alert the client immediately and document the exact file path in the report's **Appendices**.

2. **Documentation & Reporting:**
   Saving all screenshots, command outputs, and activity logs before disconnecting from the client network (while ensuring no client PII or sensitive production data is retained locally). A professional report includes an **Executive Summary** (for leadership), **Attack Chain**, **Detailed Findings** (with risk ratings and remediation guidance), and **Appendices**.

3. **Draft Report $\rightarrow$ Review Meeting $\rightarrow$ Final Report:**
   * Deliver a **DRAFT** report first so the client can review the findings and provide feedback.
   * Conduct a **Report Review Meeting** to walk through the findings, clarify technical questions, and incorporate feedback before issuing the **FINAL** report.

4. **Post-Remediation Testing (Retest):**
   Once the client implements fixes, perform a targeted verification scan/test to confirm whether each vulnerability has been resolved—documenting the results in a clear **`Remediated` vs. `Not Remediated`** status table.

5. **The Penetration Tester's Role in Remediation:**
   Because pentesters serve as independent auditors, we do **not** rewrite client application code or apply patches directly on production servers—instead, we provide high-level remediation recommendations (such as *"sanitize user input"* or *"enforce least-privilege permissions"*).

6. **Data Retention & Project Close-Out:**
   Upon engagement closure, securely wipe all client data from testers' local machines, archive engagement evidence in the firm's encrypted storage according to contractual retention policies, and issue the final project invoice.
