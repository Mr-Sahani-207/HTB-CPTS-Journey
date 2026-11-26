# Networking Fundamentals — Complete Study Notes

## 1. OSI Model (Open Systems Interconnection)

* **Overview:** Developed by **ISO** in **1984** to standardize communication between devices from different vendors. It is divided into **7 layers**[cite: 1].

| Layer # | Layer Name | Protocol Data Unit (PDU) | Key Function & Addressing | Examples / Protocols / Devices |
| :--- | :--- | :--- | :--- | :--- |
| **7** | **Application Layer** | Data | Provides a user interface to access network services and resources | HTTP, HTTPS, DNS, TFTP, Telnet, SSH, SMTP, DHCP[cite: 1, 39] |
| **6** | **Presentation Layer** | Data | Data formatting/translation, encryption/decryption, and compression/decompression | File formats (`.doc`, `.txt`, `.png`), SSL/TLS[cite: 1, 2] |
| **5** | **Session Layer** | Data | Establishes, maintains, and terminates communication sessions (connections) between hosts | NetBIOS, RPC, SQL sessions[cite: 2] |
| **4** | **Transport Layer** | Segment (TCP) / Datagram (UDP) | End-to-end delivery, segmentation, flow control, and error control using **Port Numbers** | TCP (Protocol `6`), UDP (Protocol `17`)[cite: 2, 6] |
| **3** | **Network Layer** | Packet | Source-to-destination (host-to-host) delivery, logical addressing (**IP Address**), and **Routing** (shortest path selection) | IPv4, IPv6, ICMP, OSPF, EIGRP, Routers[cite: 6, 27, 28, 35, 37] |
| **2** | **Data Link Layer** | Frame | Node-to-node delivery on the local network using physical **MAC Addresses** | Ethernet, ARP, Switches[cite: 4, 26, 42] |
| **1** | **Physical Layer** | Bit / Byte | Converts frames into electrical/optical signals (`0`s and `1`s) for transmission over physical media | Hubs, Repeaters, Modems, Cables, Connectors *(unintelligent/stateless devices operate here)*[cite: 5] |

### Time to Live (TTL) — Layer 3 Loop Prevention
* **Purpose:** Prevents packets from looping endlessly across a Layer 3 network by setting an expiration hop limit[cite: 4, 6].
* **How It Works:** Every router decrements the packet's TTL value by `1` upon forwarding (egress/hop)[cite: 4].
  * If TTL reaches `0` **before** reaching the destination, the packet is dropped[cite: 4].
  * If TTL reaches `0` **at** the destination, the packet is accepted[cite: 4].
* **Default OS TTL Values (Useful for Passive OS Fingerprinting):**
  * **Linux / Unix:** `64`
  * **Windows:** `128`
  * **Cisco / Network Devices:** `255`[cite: 4]

---

## 2. TCP/IP Model

* **Overview:** Stands for **Transmission Control Protocol / Internet Protocol** (also known as the **DoD Model**). Developed by the U.S. **Department of Defense (DoD)** and **ARPA** for vendor-independent internetwork communication[cite: 9].
* **4 Layers of the TCP/IP Model:**
  1. **Application Layer** *(combines OSI Layers 5, 6, and 7)*
  2. **Transport Layer** *(maps to OSI Layer 4)*
  3. **Internet Layer** *(maps to OSI Layer 3)*
  4. **Network Access Layer** *(combines OSI Layers 1 and 2)*[cite: 9]

---

## 3. Addressing Fundamentals (IP & MAC Addresses)

### Types of Network Addresses
1. **IP Address (Internet Protocol):** A **logical address** that provides a unique logical identity to network devices for inter-network communication. Two versions exist: **IPv4** and **IPv6**[cite: 7].
2. **MAC Address (Media Access Control):** A **48-bit physical address** represented in **hexadecimal** format, permanently burned into the Network Interface Card (NIC) by the manufacturer[cite: 7].

### Public vs. Private IP Addresses
* **Private IP:** Assigned locally by a network administrator, unique only within a local network (LAN), and non-routable on the public internet[cite: 8].
* **Public IP:** Assigned by an Internet Service Provider (ISP), globally unique, and used for WAN/internet communication[cite: 8].

### Number Systems Used in Networking
* **Binary (`0`, `1`):** Base-2 system used internally by network hardware[cite: 13].
* **Decimal (`0–9`):** Base-10 system used to represent **IPv4** addresses and subnet masks[cite: 13].
* **Hexadecimal (`0–9`, `A–F`):** Base-16 system used in **IPv6** and **MAC** addresses (`A=10`, `B=11`, `C=12`, `D=13`, `E=14`, `F=15`)[cite: 13].

---

## 4. IPv4 Addressing & Classes

* **Structure:** A **32-bit** logical address written in **dotted-decimal notation**, divided into **4 octets** (8 bits each) separated by dots[cite: 8].
* **IPv4 Address Classes:**

| Class | First Octet Range | Default Structure (`N` = Network, `H` = Host) | Primary Purpose |
| :--- | :--- | :--- | :--- |
| **Class A** | `0 – 127` | `N.H.H.H` (`/8`) | Unicast & broadcast (large networks) |
| **Class B** | `128 – 191` | `N.N.H.H` (`/16`) | Unicast & broadcast (medium networks) |
| **Class C** | `192 – 223` | `N.N.N.H` (`/24`) | Unicast & broadcast (small networks) |
| **Class D** | `224 – 239` | N/A | Reserved for **Multicasting** |
| **Class E** | `240 – 255` | N/A | Reserved for **Research & Development**[cite: 10] |

* **Reserved Special Addresses:**
  * `0.0.0.0`: Reserved for **default routing**[cite: 12].
  * `127.0.0.0/8` (e.g., `127.0.0.1`): Reserved for **loopback / NIC card self-testing**[cite: 12].
* **Private IPv4 Ranges (RFC 1918):**
  * **Class A:** `10.0.0.0` – `10.255.255.255` (`/8`)
  * **Class B:** `172.16.0.0` – `172.31.255.255` (`/12`)
  * **Class C:** `192.168.0.0` – `192.168.255.255` (`/16`)[cite: 10]
* **APIPA (Automatic Private IP Addressing):**
  * **Range:** `169.254.0.0` – `169.254.255.255` (`/16`)[cite: 10, 39]
  * **Behavior:** If a client sends a DHCP Discover message and receives no IP address from a DHCP server within ~30 seconds, the OS automatically self-assigns an APIPA address for local link communication[cite: 39].

---

## 5. IPv6 Addressing

* **Structure:** A **128-bit** hexadecimal address divided into **8 blocks** separated by colons (`:`)[cite: 3, 13].
* **Key IPv6 Address Ranges:**
  * **Global Unicast:** `2000::/3` (publicly routable internet addresses)
  * **Unique Local:** `FC00::/7` (private internal network addresses)
  * **Link-Local:** `FE80::/10` (auto-configured local segment communication)
  * **Loopback:** `::1/128` (equivalent to `127.0.0.1` in IPv4)
  * **Multicast:** `FF02::/8` (link-local multicast traffic)[cite: 13]

---

## 6. Subnetting, CIDR, FLSM & Route Summarization

### Core Concepts
* **Subnet:** A smaller logical subdivision of a larger IP network[cite: 3].
* **Subnetting:** Dividing a large network into multiple smaller sub-networks to reduce broadcast domain size and minimize IP address wastage[cite: 12].
* **Subnet Mask:** A 32-bit dotted-decimal value that distinguishes the **Network bits** (`1`s, counted from left to right) from the **Host bits** (`0`s, counted from right to left)[cite: 3, 7, 8].
* **Wildcard Mask:** The inverse of a subnet mask—`0` bits indicate bits that **must match**, and `1` bits indicate bits to **ignore** (used in ACLs, OSPF, and EIGRP)[cite: 3].
* **CIDR (Classless Inter-Domain Routing):** Represents subnet masks using slash notation (`/1` to `/32`), indicating the total number of network (`1`) bits[cite: 17].

### Binary & Bit-Value Reference Table (Per Octet)
* Bit positional weights: `128 | 64 | 32 | 16 | 8 | 4 | 2 | 1`[cite: 11]
* Cumulative subnet mask values for `1` to `8` active network bits:
  * `1 bit = 128` | `2 bits = 192` | `3 bits = 224` | `4 bits = 240`
  * `5 bits = 248` | `6 bits = 252` | `7 bits = 254` | `8 bits = 255`[cite: 11, 12]
* **Classful Subnetting CIDR Ranges:**
  * Class A: `/8` to `/15` | Class B: `/16` to `/23` | Class C: `/24` to `/31`[cite: 11]

### The 8 Key Subnetting Formulas
1. **Subnet Mask:** Convert CIDR (`/` bits) into octets of `8 + 8 + 8 + extra bits`[cite: 12, 17].
2. **Number of Subnets (Networks):** $\text{Networks} = 2^n$ (where $n$ = newly borrowed network bits in the interesting octet)[cite: 12, 15].
3. **Number of Valid Hosts:** $\text{Usable Hosts} = 2^H - 2$ (where $H$ = remaining host `0` bits; subtract `2` for Network ID and Broadcast ID)[cite: 12, 15].
4. **Block Size:** $\text{Block Size} = 256 - \text{Last Non-Zero Subnet Mask Octet}$[cite: 12, 15, 17].
5. **Network ID (NID):** Copy octets where the mask is `255` unchanged; find the multiple of the Block Size $\le$ the IP's interesting octet; set remaining host octets to `0`[cite: 12, 20].
6. **First Valid Host (FVH):** $\text{Network ID} + 1$[cite: 12, 20].
7. **Broadcast ID (BID):** $\text{Next Network ID} - 1$[cite: 12, 18].
8. **Last Valid Host (LVH):** $\text{Broadcast ID} - 1$[cite: 12, 18].

### Worked Subnetting Examples
* **Example 1 (`192.168.70.35/27`):**
  * **Subnet Mask:** `255.255.255.224` (`24 + 3` bits; `3 bits = 224`)
  * **Block Size:** $256 - 224 = 32$
  * **Usable Hosts:** $2^5 - 2 = 30$
  * **Subnets:** $2^3 = 8$
  * **Network ID:** `192.168.70.0` | **FVH:** `192.168.70.1`[cite: 16, 20]
* **Example 2 (`192.168.75.20/29`):**
  * **Subnet Mask:** `255.255.255.248` (`24 + 5` bits; `5 bits = 248`)
  * **Block Size:** $256 - 248 = 8$
  * **Usable Hosts:** $2^3 - 2 = 6$ | **Subnets:** $2^5 = 32$
  * **Network ID:** `192.168.75.0` | **FVH:** `192.168.75.1` | **LVH:** `192.168.75.6` | **Broadcast ID:** `192.168.75.7`[cite: 18]
* **CIDR from Host Requirement (`10.1.1.5/?` needing `50` hosts):**
  * Nearest power of 2: $2^6 - 2 = 62 \ge 50 \implies H = 6$ host bits.
  * $\text{CIDR} = 32 - 6 = \mathbf{/26}$ (`10.1.1.5/26`)[cite: 19].

### FLSM vs. Route Summarization (Supernetting)
* **FLSM (Fixed-Length Subnet Mask):** Uses the exact same subnet mask across all subnets in a network. Because it cannot dynamically scale up or down per department without re-addressing, it leads to IP wastage[cite: 21].
* **Route Summarization (Supernetting):** Combines multiple contiguous small networks into a single larger summary route to reduce routing table size[cite: 22, 30].
  * **Method:** Align the networks, keep identical leading octets as-is, convert the first differing octet into binary, count all matching left-to-right bits to find the new **CIDR**, and sum those matching bit weights for the summarized octet[cite: 22, 23, 30].
  * **Example:** `192.168.180.0/24`, `192.168.185.0/23`, `192.168.187.0/22`, `192.168.189.0/26` $\rightarrow$ First 20 bits (`8 + 8 + 4`) match (`180–189` share `1011xxxx`, where `128+32+16 = 176`) $\rightarrow$ Summarized Route: **`192.168.176.0/20`**[cite: 23].

---

## 7. Core Network Protocols (ARP, ICMP, DHCP, NAT)

### Address Resolution Protocol (ARP)
* **Overview:** A **Layer 2** protocol used in IPv4 Ethernet networks to resolve a known **IPv4 address** to its corresponding **MAC address** and detect IP address conflicts[cite: 26].
* **Packet Behavior:** An **ARP Request** is **Broadcast**, while an **ARP Reply** is **Unicast**[cite: 26].
* **ARP Cache & Commands:** Default ARP table aging time is **4 hours**[cite: 26]. View the ARP table with `arp -a`; clear it with `arp -d`[cite: 26, 29].
* **Three Types of ARP:**
  1. **Simple (Standard) ARP:** Host A broadcasts an ARP request asking who owns Host B's IP on the local subnet; Host B replies directly with its MAC address[cite: 25, 26].
  2. **Proxy ARP:** When a host lacks a default gateway or sends an ARP request for a remote subnet, the router acts as a middleman and replies with its own MAC address so it can forward the packet[cite: 25, 29].
  3. **Gratuitous ARP:** An unsolicited ARP packet broadcast automatically when a device boots or configures an IP address—used to announce its IP-to-MAC mapping to all hosts and check for duplicate IP conflicts[cite: 29].

### Internet Control Message Protocol (ICMP)
* **Overview:** A **Layer 3** protocol (**Protocol Number `1`**) used for network diagnostics, error reporting, and verifying end-to-end connectivity (e.g., via `ping`)[cite: 27].
* **Key ICMP Message Types:**
  * **Type `0`:** Echo Reply
  * **Type `8`:** Echo Request[cite: 27]

### Dynamic Host Configuration Protocol (DHCP)
* **Overview:** A **Layer 7** open-standard protocol running over **UDP Ports `67` (Server) and `68` (Client)** that dynamically assigns IP configurations to hosts. Can be configured on routers or Layer 3 switches[cite: 39].
* **DORA Process (4 DHCP Messages):**
  1. **D**iscover (Client broadcasts to find a DHCP server)
  2. **O**ffer (Server proposes an available IP lease)
  3. **R**equest (Client formally requests the offered IP)
  4. **A**cknowledgment (Server confirms and finalizes the lease)[cite: 39]

### Network Address Translation (NAT)
* **Overview:** Translates private internal LAN IP addresses into public WAN IP addresses to enable internet access[cite: 41].
* **Three Types of NAT:**
  1. **Static NAT (One-to-One):** Maps one private IP permanently to one public IP in the NAT table (primarily used for internal servers accessible from the internet)[cite: 41].
  2. **Dynamic NAT (Many-to-Many):** Maps internal LAN users dynamically to a pool of available public IPs using temporary NAT table entries[cite: 41].
  3. **PAT / NAT Overload (One-to-All / Many-to-One):** Translates multiple private LAN hosts simultaneously onto a **single public IP address** by tracking unique source port numbers in temporary NAT table entries[cite: 41, 42].

---

## 8. Routing Concepts & Protocols

### Routing Fundamentals
* **Routing:** The process a router uses to forward packets from one network to another by selecting the optimal path[cite: 28].
* **Routing Protocol vs. Routed Protocol:**
  * **Routing Protocol:** Rules used *between routers* to exchange network topology information (e.g., RIP, EIGRP, OSPF, BGP)[cite: 28, 31].
  * **Routed Protocol:** Protocols that carry actual *user traffic* from source to destination (e.g., **IPv4**, **IPv6**)[cite: 28].
* **Symmetric vs. Asymmetric Routing:**
  * **Symmetric Routing:** Outgoing and returning packets follow the exact same path[cite: 40].
  * **Asymmetric Routing:** Outgoing and returning packets take different paths[cite: 40].
* **Recursive Lookup:** Occurs when a static route is configured using only a **next-hop IP** (without an exit interface), forcing the router to perform two routing table lookups for a single destination and increasing CPU usage[cite: 40].
* **Route Redistribution:** Translates and exchanges routing information between two different routing protocols[cite: 36].

### Route Precedence (Path Selection Order)
1. **Longest Prefix Match:** The router always prefers the most specific route (highest CIDR slash value, e.g., `/28` over `/24`)[cite: 24, 30].
2. **Administrative Distance (AD Value):** Measures route trustworthiness (`0–255`); the **lowest AD** is always preferred[cite: 24, 38].
3. **Metric:** Used as a tie-breaker when multiple routes come from the same routing protocol[cite: 30].

| Route Source / Type | Default AD Value |
| :--- | :--- |
| **Directly Connected** | `0` |
| **Static Route** | `1` |
| **EIGRP Summary Route** | `5` |
| **External BGP (eBGP)** | `20` |
| **Internal EIGRP** | `90` |
| **OSPF** | `110` |
| **RIP** | `120` |
| **External EIGRP** | `170` |
| **Internal BGP (iBGP)** | `200`[cite: 31, 37, 38] |

### Static & Default Routing (Cisco IOS)
* **Static Routing:** Manually configured by administrators; low CPU overhead and ideal for small, stable networks[cite: 30].
  * Syntax: `ip route <Network-ID> <Subnet-Mask> <Self-Exit-Interface> <Next-Hop-IP>`[cite: 30]
* **Floating Static Route:** A backup static route configured with a higher AD value than the primary route, automatically entering the routing table if the primary path fails[cite: 38].
* **Default Routing ("Gateway of Last Resort"):** Matches all unknown destinations (`0.0.0.0 0.0.0.0`); typically configured on edge routers pointing toward the ISP[cite: 33, 40].
  * Syntax: `ip route 0.0.0.0 0.0.0.0 <Exit-Interface / Next-Hop>`[cite: 33]
* **IPv6 Routing Commands:**
  * Enable IPv6 routing: `ipv6 unicast-routing`
  * Configure interface IPv6: `ipv6 address 12:1:1:1::1/64` and `no shutdown`
  * Verify IPv6 interfaces/routes: `show ipv6 interface brief` and `show ipv6 route`[cite: 33]

### Dynamic Routing Protocols Comparison

| Feature | RIP (v1 / v2) | EIGRP | OSPF |
| :--- | :--- | :--- | :--- |
| **Type** | Distance-Vector (Open Standard) | Advanced Distance-Vector / Hybrid (IGP) | Link-State (Open Standard IGP) |
| **Transport / Protocol #** | **UDP Port `520`** | **IP Protocol `88`** | **IP Protocol `89`** |
| **Multicast Address** | `224.0.0.9` (RIPv2) | **`224.0.0.10`** | **`224.0.0.5`** & **`224.0.0.6`** |
| **Default AD Value** | `120` | `90` (Internal), `170` (External), `5` (Summary) | `110` |
| **Metric** | **Hop Count** (Max `15`; `16` = unreachable) | Composite (`K1` Bandwidth, `K2` Load, `K3` Delay, `K4` Reliability, `K5` MTU) | **Cost** ($\frac{\text{Reference Bandwidth}}{\text{Interface Bandwidth}}$) |
| **Limitations / Key Notes** | Sends full routing table updates every `30s`; RIPv1 is classful | Requires matching **Autonomous System (AS)** number; default max hops `100` (max `255`) | Maintains **Neighbor**, **Database (LSDB)**, and **Routing** tables; default equal-cost max paths = `4` |
| **Timers & Messages** | 30s periodic updates | Hello/Hold timers | Timers: **Hello `10s`**, **Hold/Dead `40s`**, **Wait `40s`**; Messages: **Hello, DBD, LSR, LSU, LSAck**[cite: 31, 32, 34, 35, 36, 37] |

* **Key Cisco Verification & Config Commands:**
  * **RIP:** `router rip` $\rightarrow$ `version 2` $\rightarrow$ `network <net>` | Verify: `show ip protocols`, `show ip route rip`[cite: 32]
  * **EIGRP:** `router eigrp <AS>` $\rightarrow$ `network <net> <wildcard>` | Verify: `show ip eigrp neighbors`, `show ip protocols`[cite: 34, 37]

---

## 9. Switching, VLANs & Access Control Lists (ACLs)

### Layer 2 Switch Operation
* **Overview:** An intelligent, manageable **Layer 2** centralized device that connects multiple end hosts and forwards frames based on **MAC addresses**[cite: 42].
* **CAM / MAC Address Table:**
  * Dynamically learns source MAC addresses and maps them to switch ports[cite: 42].
  * Stores both **Static** and **Dynamic** entries; dynamic entries age out after **300 seconds (5 minutes)**[cite: 42].

### Virtual Local Area Networks (VLANs)
* **Overview:** Logically segments a physical switch to **break up Layer 2 broadcast domains**[cite: 44].
* **VLAN Storage & Defaults:**
  * Stored in flash memory as **`vlan.dat`**[cite: 44].
  * **VLAN 1** is present by default on all switches, acts as the default management VLAN, and initially owns all switch ports[cite: 44].
* **VLAN ID Ranges (`0–4095`):**
  * **Standard VLANs (`1 – 1005`):** Used in small-to-medium networks[cite: 44].
  * **Extended VLANs (`1006 – 4094`):** Used in large enterprise networks[cite: 44].
* **Switch Port Modes:**
  * **Switchport (Layer 2 Port):** Forwards frames based on MAC addresses[cite: 44].
  * **Routed Port (Layer 3 Port):** Configured with `no switchport`; disables Layer 2 switching on that interface so an IP address can be assigned directly[cite: 43].
  * **Access Port:** Connects end devices (PCs, servers, printers) and carries traffic for **only one VLAN**[cite: 43].
  * **Trunk Port:** Connects switches together, carries traffic for **multiple active VLANs**, and inserts a **VLAN tag (802.1Q)** into forwarded frames[cite: 43].

### Access Control Lists (ACLs)
* **Overview:** Filters incoming (`in`) and outgoing (`out`) network traffic on router interfaces[cite: 40].
* **Two Main Types of ACLs:**
  1. **Standard ACL (Numbered `1–99` or Named):** Filters traffic strictly based on **Source IP Address** only; should be applied **closest to the destination**[cite: 40].
  2. **Extended ACL (Numbered `100–199` or Named):** Filters traffic based on Source IP, Destination IP, Protocol, and Port numbers; should be applied **closest to the source**[cite: 40].
