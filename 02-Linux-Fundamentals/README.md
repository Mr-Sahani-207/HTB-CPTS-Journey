Linux Fundamentals
Core Linux CLI commands and system administration notes for pentesting labs.
File & Directory Navigation
```bash
pwd                                   # Print current working directory
ls -la                                # List all files including hidden ones with permissions
find / -name "file.txt" 2>/dev/null   # Search for a file while hiding permission errors
grep -rn "password" /etc/             # Search text recursively inside files
```
File Permissions & Users
Permission values: `Read (r) = 4`, `Write (w) = 2`, `Execute (x) = 1`
```bash
chmod 755 script.sh     # rwxr-xr-x
chmod +x script.sh      # Make a script executable
chown user:group file   # Change file owner and group
sudo -l                 # Check what commands the current user can run as root
```
Important user files:
`/etc/passwd` — User account details (world-readable)
`/etc/shadow` — Password hashes (root-only readable)
Process & Service Management
```bash
ps aux                  # View all running processes
kill -9 <PID>           # Force kill a process by ID
systemctl status ssh    # Check status of a service
systemctl start apache2 # Start a service
```
Networking & Transfers
```bash
ip a                    # Check network interfaces and IP addresses
ss -tulpn               # View active listening TCP/UDP ports and processes
curl -I http://<IP>     # Fetch HTTP response headers
wget http://<IP>/file   # Download a file from a web server
```
Basic Bash Scripting
```bash
#!/bin/bash
for ip in $(seq 1 10); do
    ping -c 1 10.129.1.$ip | grep "bytes from" &
done
wait
```
