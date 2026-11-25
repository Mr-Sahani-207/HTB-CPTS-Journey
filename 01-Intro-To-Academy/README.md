# Intro To Academy
Basic setup notes for connecting to Hack The Box Academy labs from my local Kali VM.
Lab Connection Setup
OpenVPN Connection: Download the `.ovpn` connection pack from the HTB Academy dashboard and connect via terminal:
```bash
  sudo openvpn academy-regular.ovpn
  ```
Verify VPN Interface: Check if the `tun0` interface is up and assigned an internal HTB IP (`10.10.x.x`):
```bash
  ip a show tun0
  ```
Target Connectivity Check: After spawning a target machine, verify connectivity using ping:
```bash
  ping -c 4 <TARGET_IP>
  ```
Quick Notes
Keep the OpenVPN terminal tab running in the background while working on labs.
If a target stops responding or resets connections, check `tun0` status first before restarting the target instance.
