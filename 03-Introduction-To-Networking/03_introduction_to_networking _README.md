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

## TCP 3-Way Handshake
1. **SYN:** Client sends a synchronize packet to initiate a connection.
2. **SYN-ACK:** Server responds with synchronize-acknowledge.
3. **ACK:** Client sends acknowledge, and data transfer begins.

## IPv4 Subnetting Cheat-Sheet
| CIDR | Subnet Mask | Total IPs | Usable Hosts |
| :--- | :--- | :--- | :--- |
| `/24` | `255.255.255.0` | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 14 |
| `/30` | `255.255.255.252` | 4 | 2 |

## Common Ports to Remember
- **21** — FTP (File Transfer Protocol)
- **22** — SSH (Secure Shell)
- **23** — Telnet (Unencrypted remote access)
- **25** — SMTP (Email transfer)
- **53** — DNS (Domain Name System - TCP/UDP)
- **80 / 443** — HTTP / HTTPS (Web traffic)
- **139 / 445** — SMB (Windows file sharing)
- **3306** — MySQL Database
- **3389** — RDP (Remote Desktop Protocol)
