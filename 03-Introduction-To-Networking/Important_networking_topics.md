# Introduction To Networking

Quick reference notes on networking models, IP addressing, subnetting, and common ports.

## OSI vs TCP/IP Model
| OSI Layer | TCP/IP Layer | Key Protocols / Examples |
| :--- | :--- | :--- |
| 7. Application | Application | HTTP, HTTPS, FTP, SSH, DNS, SMB |
| 6. Presentation | Application | SSL/TLS, ASCII, JPEG |
| 5. Session | Application | NetBIOS, RPC |
| 4. Transport | Transport | TCP, UDP |
| 3. Network | Internet | IP (IPv4/IPv6), ICMP, ARP |
| 2. Data Link | Network Access | Ethernet, MAC Address, Switches |
| 1. Physical | Network Access | Cables, Fiber, Wi-Fi Signals |

# TCP 3-Way Handshake — Complete Technical Guide

---

## 1. Diagrammatic Overview

Refer to the diagram below to visualize the packet flow and state transitions during connection establishment:

![TCP 3-Way Handshake Flow](image_44.png)

---

## 2. What Is the TCP 3-Way Handshake?

* **Definition:**
  * **TCP (Transmission Control Protocol)** is a connection-oriented protocol that establishes a reliable, verified session between a **Client** and a **Server** before transmitting any application data.
  * This three-step connection establishment sequence is known as the **TCP 3-Way Handshake**.

* **Objective:**
  * Verifies that both endpoints are active and reachable on the network, while synchronizing (`SYN`) and acknowledging (`ACK`) initial sequence numbers to ensure ordered, lossless packet delivery.

---

## 3. How the 3 Steps Work (`SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`)

#### Step 1 — `SYN` (Synchronize)

* The **Client** initiates the connection by sending a TCP segment with the **`SYN`** flag set and chooses an Initial Sequence Number (e.g., `Seq = 100`).
* **Plain English:** *"Hello Server, I would like to connect with you. Let's synchronize our sequence numbers."*

#### Step 2 — `SYN-ACK` (Synchronize-Acknowledgment)

* Upon receiving the `SYN` packet, the **Server** responds with both the **`SYN`** and **`ACK`** flags set—confirming receipt of the client's packet (`Ack = 101`, indicating the next expected byte) and advertising its own Initial Sequence Number (e.g., `Seq = 300`).
* **Plain English:** *"Hello Client, I received your request! Here is my sequence number—can you hear me?"*

#### Step 3 — `ACK` (Acknowledgment)

* The **Client** sends a final segment with the **`ACK`** flag set (`Ack = 301`) to confirm receipt of the server's sequence number, completing the handshake.
* **Plain English:** *"Yes Server, I received your acknowledgment. Everything is synchronized—let's begin data transfer."*

---

## 4. TCP Connection State Transitions

* **Client Side:** `CLOSED` $\rightarrow$ `SYN-SENT` $\rightarrow$ *(receives `SYN-ACK` & sends `ACK`)* $\rightarrow$ `ESTABLISHED`
* **Server Side:** `LISTEN` $\rightarrow$ `SYN-RECEIVED` $\rightarrow$ *(receives `ACK`)* $\rightarrow$ `ESTABLISHED`

*Note:* Once both endpoints reach the **`ESTABLISHED`** state, actual payload data (such as HTTP requests or file transfers) begins flowing across the connection.

---

## 5. Penetration Testing & Security Significance

* **Nmap Stealth SYN Scan (`-sS`):**
  * To scan targets quickly without completing full application-level sessions, Nmap sends only **Step 1 (`SYN`)**.
  * If the target port responds with **`SYN-ACK`**, Nmap marks the port as **`open`** and immediately sends an **`RST` (Reset)** packet to tear down the connection before Step 3—making the scan faster and less noisy in application logs.

* **SYN Flood Attack (Denial of Service):**
  * An attacker overwhelms a target server by sending a high volume of **`SYN`** packets from spoofed IP addresses and intentionally withholding the final **`ACK`** in Step 3.
  * Because the server keeps each connection in the `SYN-RECEIVED` state while waiting for an acknowledgment, its half-open connection queue fills up, preventing legitimate users from connecting.

## IPv4 Subnetting Cheat-Sheet
| CIDR | Subnet Mask | Total IPs | Usable Hosts |
| :--- | :--- | :--- | :--- |
| `/24` | `255.255.255.0` | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 14 |
| `/30` | `255.255.255.252` | 4 | 2 |

# Essential Network Protocols & Port Reference (Penetration Testing Guide)

---

## 1. Core Protocols & Security Angles

| Protocol | Port / Protocol # | Layer | Core Function | Penetration Testing / Security Angle |
| :--- | :--- | :--- | :--- | :--- |
| **DNS** | `53` (UDP/TCP) | Layer 7 | Resolves human-readable domain names (`google.com`) into IP addresses | Subdomain enumeration, Zone Transfer (`AXFR`), DNS tunneling |
| **HTTP / HTTPS** | `80` / `443` (TCP) | Layer 7 | Serves unencrypted (`HTTP`) and encrypted (`HTTPS`) web applications | Web application attacks (SQLi, XSS, LFI/RFI, IDOR, SSRF) |
| **SMB** | `445` (TCP) | Layer 7 | File and printer sharing across Windows networks | Null sessions, exposed shares, EternalBlue, Responder NTLMv2 relay |
| **SSH** | `22` (TCP) | Layer 7 | Encrypted remote terminal login and file transfer on Linux/network devices | Brute-force, stolen private keys (`id_rsa`), SSH tunneling and pivoting |
| **RDP** | `3389` (TCP/UDP) | Layer 7 | Remote graphical desktop access for Windows systems | Password spraying, credential reuse, BlueKeep |
| **FTP** | `21` (TCP) | Layer 7 | Unencrypted plaintext file transfer between client and server | Anonymous login, credential sniffing, malicious file upload |
| **SMTP / POP3 / IMAP** | `25` / `110` / `143` (TCP) | Layer 7 | Sending (`SMTP`) and retrieving (`POP3`/`IMAP`) emails | User enumeration (`VRFY`), phishing, internal email pillaging |
| **SNMP** | `161` / `162` (UDP) | Layer 7 | Monitoring and managing routers, switches, and servers | Default community strings (`public`/`private`) leaking system configuration |
| **LDAP / Kerberos** | `389` / `88` (TCP/UDP) | Layer 7 | Windows Active Directory directory queries (`LDAP`) and authentication (`Kerberos`) | AD enumeration, Kerberoasting, AS-REP Roasting, Pass-the-Ticket |
| **DHCP** | `67` / `68` (UDP) | Layer 7 | Automatically assigns IP configurations to hosts (DORA process) | Rogue DHCP server, DHCP starvation, Man-in-the-Middle (MITM) attacks |
| **TCP / UDP** | Protocol `6` / `17` | Layer 4 | **TCP:** Reliable connection-oriented delivery (3-Way Handshake); **UDP:** Fast connectionless delivery | SYN flood, port scanning (`-sS` vs `-sU`), reverse shells |
| **ICMP** | Protocol `1` | Layer 3 | Connectivity diagnostics (`ping` — Type `8` Request, Type `0` Reply) and error reporting | Host discovery (ping sweep), ICMP tunneling, firewall evasion |
| **ARP** | Layer 2 (No Port) | Layer 2 | Resolves known IPv4 addresses to physical **MAC addresses** on a local subnet | **ARP Spoofing / Poisoning** (MITM sniffing) |

---

## 2. Top 5 Priority Protocols to Master First

1. **DNS (`53`):** Reconnaissance and subdomain enumeration almost always begin here.
2. **HTTP / HTTPS (`80` / `443`):** Over 70% of initial footholds in external assessments come through web applications.
3. **SMB (`445`):** The primary target for misconfigurations, credential harvesting, and lateral movement inside Windows/Active Directory networks.
4. **SSH (`22`) & RDP (`3389`):** Essential for remote interactive access and pivoting once valid credentials or keys are obtained.
5. **ARP & ICMP:** Foundational protocols for local network host discovery and Man-in-the-Middle traffic interception.

---

## 3. Must-Know 24 Ports + 3 Protocol Numbers (Cheat Sheet)

### Core Network & Infrastructure (5)

* **FTP** — `21` (TCP) — File transfer (plaintext)
* **SSH** — `22` (TCP) — Secure remote terminal and tunneling
* **Telnet** — `23` (TCP) — Unencrypted remote login (legacy devices)
* **DNS** — `53` (UDP/TCP) — Domain name to IP resolution
* **DHCP** — `67 / 68` (UDP) — Automatic IP assignment (DORA)

### Web & File Transfer (4)

* **TFTP** — `69` (UDP) — Trivial FTP (unauthenticated small configuration transfers)
* **HTTP** — `80` (TCP) — Unencrypted web traffic
* **HTTPS** — `443` (TCP) — Encrypted web traffic (TLS/SSL)
* **HTTP Alt / Web Apps** — `8080 / 8443` (TCP) — Apache Tomcat, proxies, and development web servers

### Email Protocols (3)

* **SMTP** — `25` (TCP) — Outgoing email transmission (`465` / `587` secure)
* **POP3** — `110` (TCP) — Email retrieval/download (`995` secure)
* **IMAP** — `143` (TCP) — Server-synchronized email access (`993` secure)

### Windows & Active Directory (6)

* **Kerberos** — `88` (TCP/UDP) — Active Directory authentication
* **RPC** — `135` (TCP) — Windows Remote Procedure Call
* **NetBIOS** — `137–139` (UDP/TCP) — Legacy Windows name resolution and file sharing
* **SMB** — `445` (TCP) — Windows file/printer sharing and lateral movement
* **LDAP / LDAPS** — `389 / 636` (TCP/UDP) — Active Directory directory queries
* **RDP** — `3389` (TCP/UDP) — Windows Remote Desktop

### Network Management & Linux Shares (3)

* **NTP** — `123` (UDP) — Network clock synchronization (critical for Kerberos tickets)
* **SNMP** — `161 / 162` (UDP) — Network device monitoring
* **NFS** — `2049` (TCP/UDP) — Linux/Unix network file sharing

### Databases (3)

* **MS SQL** — `1433` (TCP) — Microsoft SQL Server
* **MySQL / MariaDB** — `3306` (TCP) — Open-source relational database
* **PostgreSQL** — `5432` (TCP) — Enterprise relational database

### Layer 3 & Layer 4 Protocol Numbers (No Ports)

* **ICMP** — Protocol `1` (`ping`, traceroute)
* **TCP** — Protocol `6` (3-Way Handshake: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`)
* **UDP** — Protocol `17` (Fast, connectionless)
