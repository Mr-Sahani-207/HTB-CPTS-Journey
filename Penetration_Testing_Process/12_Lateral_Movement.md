# 12. Lateral Movement — Core Steps & Attack Techniques

## 1. Six Core Steps of Lateral Movement

1. **Pivoting & Tunneling (Setting Up a Proxy):**
   Compromised target machines rarely have penetration testing tools (such as Nmap) installed. Instead, we configure the compromised host as a **proxy or tunnel** so that scans and exploits launched from our Kali Linux attack machine can route through it to reach internal servers that are inaccessible directly from the internet.

2. **Evasive Testing:**
   Moving quietly through the internal network while bypassing defenses such as **micro-segmentation, IDS/IPS, and EDR** so the Blue Team is not alerted to your presence.

3. **Internal Information Gathering:**
   Enumerating the internal network from the compromised host's perspective to discover connected hosts, internal subnets, routing paths, and backend servers.

4. **Internal Vulnerability Assessment:**
   Evaluating internal hosts for weaknesses—internal systems frequently have looser security controls and broader permissions than perimeter servers, such as open network shares or privileged **user group memberships** (e.g., a compromised user belonging to a `Developers` group may already have access to internal source code repositories and staging/production servers).

5. **(Privilege) Exploitation & Credential Reuse:**
   * Reusing or cracking plaintext passwords and password hashes harvested during pillaging to authenticate to additional internal systems.
   * **Responder & Pass-the-Hash:** Even without cracking a password, tools like **Responder** can capture **NTLMv2 hashes** on the local network, and **Pass-the-Hash** techniques allow authenticating directly to other Windows servers as an administrator using captured hashes.

6. **Repeat Post-Exploitation:**
   On every newly compromised server, cycle back through local enumeration and evidence collection (strictly following contractual data-handling rules) before advancing to the **Proof-of-Concept (PoC)** stage.

---

## Key Takeaway

Even if you cannot immediately escalate to `root` or `Administrator` on your initial foothold machine, **Lateral Movement** enables you to pivot across internal systems where weaker permissions, misconfigurations, or exposed administrative hashes provide a path to higher privileges.
