# 4. Network Ports & High-Yield Protocols

In networking, a **Port** is a virtual or logical endpoint (entry point) that determines which specific software or application on a computer should receive incoming network traffic.

* **Analogy:**
  * **IP Address:** The street address of a large apartment building (e.g., Target Server).
  * **Port Number:** Individual apartment numbers inside that building (e.g., Apartment 80, 22, 443).
  * Just as a courier needs the apartment number after reaching the building to deliver a package to the right recipient, network packets require a port number to reach the correct service.

---

## Part 1: Port Numbers and Ranges

There are **65,536** total ports (`0` to `65535`), divided into three standard ranges:

* **Well-Known Ports (`0 – 1023`):** Reserved for core system services (typically require `root` or administrative privileges to bind/listen on).
* **Registered Ports (`1024 – 49151`):** Assigned to specific vendor applications and services (e.g., port `3306` for MySQL).
* **Dynamic / Private / Ephemeral Ports (`49152 – 65535`):** Temporarily assigned by the operating system as source ports when a client (such as a web browser) initiates outbound internet connections.

---

## Part 2: Transport Layer Port Types

Logical ports operate over two primary transport protocols:

* **TCP Ports (Transmission Control Protocol):** Connection-oriented ports that establish reliability via the 3-Way Handshake ($\text{SYN} \rightarrow \text{SYN-ACK} \rightarrow \text{ACK}$). Every packet is acknowledged to guarantee ordered delivery.
  * **Examples:** SSH (`22`), HTTP (`80`), HTTPS (`443`).
* **UDP Ports (User Datagram Protocol):** Connectionless ports that transmit datagrams directly without a handshake—faster, but without delivery guarantees.
  * **Examples:** DNS (`53`), DHCP (`67/68`), TFTP (`69`), SNMP (`161`).

---

## Part 3: High-Yield Application Protocols & Ports (Layer 4/7)

In HTB labs and real-world penetration testing, these 16 ports are high-priority targets for enumeration:

| Port | Protocol | Layer 4 Type | Plaintext / Encrypted | Pentest / Red Team Importance |
| :--- | :--- | :--- | :--- | :--- |
| **21** | **FTP** | TCP | Plaintext | Anonymous login (`anonymous:anonymous`), unencrypted file leaks |
| **22** | **SSH** | TCP | Encrypted | Remote administration, credential brute-forcing, private key (`id_rsa`) abuse |
| **23** | **Telnet** | TCP | Plaintext | Legacy insecure remote shell, cleartext credential sniffing |
| **25** | **SMTP** | TCP | Plaintext / TLS | User enumeration (`VRFY`, `EXPN`), open mail relay testing |
| **53** | **DNS** | TCP / UDP | Plaintext | Subdomain enumeration, DNS Zone Transfer attacks (`AXFR`) |
| **67 / 68** | **DHCP** | UDP | Plaintext | Rogue DHCP server attacks, network traffic spoofing |
| **69** | **TFTP** | UDP | Plaintext | Unauthenticated file transfers (router/switch configuration leaks) |
| **80** | **HTTP** | TCP | Plaintext | Web vulnerabilities (SQLi, XSS, LFI/RFI, Command Injection, Web Shells) |
| **88** | **Kerberos** | TCP / UDP | Encrypted | Active Directory attacks (Kerberoasting, AS-REP Roasting) |
| **110** | **POP3** | TCP | Plaintext | Cleartext email retrieval and credential sniffing |
| **139 / 445** | **SMB** | TCP | Mixed | Windows file share enumeration, EternalBlue, Pass-the-Hash |
| **143** | **IMAP** | TCP | Plaintext | Cleartext email retrieval |
| **161 / 162** | **SNMP** | UDP | Plaintext (v1/v2c) | Extracting full network/asset inventory via default community strings (`public`) |
| **389 / 636** | **LDAP / LDAPS** | TCP | Plaintext / TLS | Active Directory user, group, and policy querying |
| **443** | **HTTPS** | TCP | Encrypted (TLS) | Secure web applications and modern web attack vectors |
| **3389** | **RDP** | TCP | Encrypted | Windows Remote Desktop access, credential stuffing, BlueKeep |

---

## Why Full Port Scanning Matters for Pentesters

By default, Nmap only scans the **top 1,000 most common ports**:

* Running a basic `nmap 10.129.x.x` command will completely miss services running on non-standard ports (such as an administrative web panel on port `8888` or SSH moved to port `2222`).
* Therefore, standard penetration testing methodology requires running a full port scan using **`-p-`** (`nmap -p- 10.129.x.x`) to enumerate all `65,535` ports and uncover hidden initial footholds.
