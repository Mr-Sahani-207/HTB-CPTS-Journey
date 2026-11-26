# 6. Core Pentesting Utilities: Netcat, SSH, Tmux, and Vim

## Part 1: Tool Roles in IT vs. Penetration Testing

### 1. Netcat (`nc` — The TCP/IP Swiss Army Knife)
* **Primary IT Purpose:** Network debugging, connectivity testing, and raw socket communication.
* **Offensive / Pentesting Use Cases:**
  * Setting up reverse shell listeners (`nc -lvnp 4444`).
  * Performing manual banner grabbing against open ports (`nc -vn 10.10.10.10 21`).
  * Transferring files directly between machines without requiring HTTP/FTP services.

### 2. SSH (Secure Shell)
* **Primary IT Purpose:** Encrypted remote command-line administration.
* **Offensive / Pentesting Use Cases:**
  * Gaining stable, interactive shell access once valid credentials (`user:pass`) or private SSH keys (`id_rsa`) are discovered.
  * **SSH Local & Dynamic Port Forwarding (Pivoting):** Tunneling traffic through a compromised host to reach internal-only ports blocked from the outside (`ssh -L 8080:127.0.0.1:80 user@target`).

### 3. Tmux (Terminal Multiplexer)
* **Primary IT Purpose:** Terminal pane splitting and persistent background session management.
* **Offensive / Pentesting Use Cases:**
  * Running multiple parallel workflows inside a single terminal window (e.g., OpenVPN in one pane, Nmap scan in another, and a Netcat listener in a third).
  * **Session Persistence:** Long-running tasks (such as directory brute-forcing or password cracking) continue running in the background even if the terminal window closes or the SSH/VPN connection drops.

### 4. Vim (Terminal Text Editor)
* **Primary IT Purpose:** Lightweight, keyboard-driven command-line text editor.
* **Offensive / Pentesting Use Cases:**
  * Compromised target shells rarely have graphical editors (such as VS Code or Notepad); editing must be done directly inside a CLI shell.
  * Modifying public exploits, updating IP addresses/ports in reverse shell payloads, and editing configuration files on the fly.

### Summary Comparison Table

| Tool | Standard IT Role | Pentesting Role | Why It Must Be Mastered |
| :--- | :--- | :--- | :--- |
| **Netcat** | Network connection tester | Catching reverse shells & port probing | Primary tool for receiving shell connections after exploitation |
| **SSH** | Remote administration | Post-exploitation access & pivoting/tunnels | Essential for tunneling into segmented internal networks |
| **Tmux** | Terminal workspace manager | Multi-tasking & persistent background jobs | Prevents losing hours of scan progress if a connection drops |
| **Vim** | CLI text editing | Modifying scripts & exploits on the fly | Standard editor available across headless Linux targets |

---

## Part 2: Banner Grabbing

**Banner Grabbing** is a reconnaissance technique used to identify the **service name and exact software version** running on an open port.

* When a service (such as Apache, OpenSSH, or vsftpd) accepts an incoming connection, it frequently returns a welcome message known as a **"Banner."**
* Capturing this banner enables penetration testers to map the running software version to known CVEs and public exploits.

---

## Part 3: Netcat (`nc`) Command Reference

| Flag | Name | Purpose in Pentesting | Example |
| :--- | :--- | :--- | :--- |
| **`-l`** | **Listen** | Listens for incoming connections (mandatory for catching reverse shells) | `nc -l 4444` |
| **`-v`** | **Verbose** | Displays detailed connection status, errors, and output (`-vv` for extra verbosity) | `nc -v 10.10.10.5 80` |
| **`-n`** | **Numeric** | Disables DNS resolution for faster connections and avoids generating DNS logs | `nc -nv 10.10.10.5 445` |
| **`-p`** | **Port** | Specifies the local listening port (used alongside `-l`) | `nc -lp 4444` |
| **`-u`** | **UDP** | Switches from default TCP to UDP mode for services like SNMP (`161`), DNS (`53`), and TFTP (`69`) | `nc -unv 10.10.10.5 53` |
| **`-k`** | **Keep-Alive** | Forces the listener to stay open even after a client disconnects | `nc -lkvnp 443` |
| **`-w`** | **Timeout** | Sets a connection timeout in seconds so scans or banner grabs do not hang | `nc -nv -w 2 10.10.10.5 21` |

### Two Essential Netcat Command Combos

1. **Reverse Shell Listener Combo:**
   ```bash
   nc -lvnp <port>
