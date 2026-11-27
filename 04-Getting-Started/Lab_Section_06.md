### Section 6 — Service Enumeration & Banner Grabbing

* **Objective:** Identify the exact service and software version running on a non-standard port (`41374`) via banner grabbing.
* **Command Used:**
  ```bash
  nmap -sV -p 41374 <TARGET_IP>
  ```
* **Key Findings:**
  * **Open Port:** `41374/tcp` (`ssh`)
  * **Service Version:** `OpenSSH 8.2p1` (Ubuntu Linux)
  * **Raw SSH Banner (Exercise Answer):** `[REDACTED]`
* **Takeaway:** Services often run on high non-standard ports to evade basic default scans—always run full-port or targeted service version scans (`-sV`) or connect via `nc -nv <IP> <PORT>` to grab raw banners.
