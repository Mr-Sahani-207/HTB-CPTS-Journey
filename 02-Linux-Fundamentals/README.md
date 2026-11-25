### 2. `02-Linux-Fundamentals/README.md`
```markdown
# Linux Fundamentals

Core Linux CLI commands and system administration notes for pentesting labs.

## File & Directory Navigation
```bash
pwd                     # Print current working directory
ls -la                  # List all files including hidden ones with permissions
find / -name "file.txt" 2>/dev/null   # Search for a file while hiding permission errors
grep -rn "password" /etc/             # Search text recursively inside files
