# 6. The Penetration Testing Lifecycle — Iterative Workflow & Stage Breakdown

## 1. Penetration Testing Is a Flexible & Iterative Process

* **No Rigid Recipe:** Every organization has a unique infrastructure and technology stack, so penetration testing cannot follow a fixed, step-by-step checklist—you must continuously adapt your approach based on what you uncover.
* **Deterministic Yet Iterative:** Each action depends directly on the findings of the previous step, and core phases (such as Information Gathering and Post-Exploitation) loop repeatedly whenever new assets or privileges are discovered.

---

## 2. Detailed Breakdown of the 8 Engagement Stages

| # | Stage | Core Activities & Objectives |
| :--- | :--- | :--- |
| **1** | **Pre-Engagement** | Conducting kickoff meetings with the client to finalize and sign the **NDA**, **Goals**, **Scope**, **Time Estimation**, and **Rules of Engagement (RoE)**. |
| **2** | **Information Gathering** | Mapping the target website or network's technologies, software versions, and architecture to identify potential entry points. |
| **3** | **Vulnerability Assessment** | Analyzing gathered reconnaissance data using automated scanners and manual inspection to identify known flaws and suspicious functionality. |
| **4** | **Exploitation** | Preparing or customizing exploit code/tools, validating them against the target, and securing an **initial foothold (access)**. |
| **5** | **Post-Exploitation** | Performing local enumeration (**pillaging**) inside the compromised host, harvesting credentials, and executing **Privilege Escalation** (`root` / `Administrator`). |
| **6** | **Lateral Movement** | Leveraging harvested credentials, tokens, or SSH keys from a compromised host to pivot deeper into internal systems (such as backend database servers). |
| **7** | **Proof-of-Concept (PoC)** | Writing clear step-by-step reproduction documentation (or an automation script) demonstrating how chained vulnerabilities led to compromise so administrators can verify and patch them. |
| **8** | **Post-Engagement** | Removing all uploaded tools, shells, and artifacts (**cleanup**), delivering executive and technical reports, conducting a walkthrough call, and securely archiving engagement data for future **retesting**. |
