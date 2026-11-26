# Linux Administration & Command Reference (RHEL / Kali Linux)

> **Note:** Basic file navigation (`ls`, `cd`, `pwd`, `mkdir`, `cp`, `mv`, `rm`), basic text viewers (`cat`, `head`, `tail`, `less`), and `vim` basics covered in earlier notes are skipped here to focus only on new topics.

---

## 1. User Interfaces, TTYs & Shell Basics

### UI Types & Virtual Consoles (TTYs)
* **GUI (Graphical User Interface):** The visual desktop interface with windows and icons.
* **CLI (Command Line Interface):** The text-only terminal interface.
* **TTY (Teletypewriter / Virtual Consoles):** Linux systems (like RHEL and Kali Linux) give you 6 virtual consoles using `Ctrl + Alt + F1` to `F6`:
  * **`TTY1` (`F1`):** Login screen.
  * **`TTY2` (`F2`):** Graphical desktop (GUI).
  * **`TTY3` – `TTY6` (`F3`–`F6`):** Text-only virtual terminals (CLI).

### Shell Prompt & Command Structure
* **Bash (Bourne Again Shell):** The shell program that reads and runs your Linux commands.
* **Prompt Breakdown:**
  ```text
  root@localhost:~#        # '#' means root (admin) user
  redhat@localhost:~$      # '$' means normal user
  ```
  * `username` `@` `machine_name` `:` `current_folder` (`~` stands for your home folder).
* **Parts of a Command:**
  ```bash
  mkdir craw -v
  head /etc/passwd -n 1
  ```
  * **Command:** The main program you want to run (`mkdir`, `head`).
  * **Argument:** What you want the command to work on (`craw`, `/etc/passwd`).
  * **Option:** Extra flags that change how the command behaves (`-v` for verbose output, `-n 1` to show 1 line).

### Useful Session & History Commands
* **Power Control:**
  ```bash
  reboot          # Restart the system
  init 0          # Shut down the system
  ```
* **Run Multiple Commands (`;`):** Use a semicolon to run commands one after another on the same line:
  ```bash
  cmd1 ; cmd2 ; cmd3
  ```
* **Command History:**
  ```bash
  history         # See a numbered list of commands you ran earlier
  !<number>       # Re-run a command using its number from history (e.g., !15)
  ```
* **Count Lines, Words & Characters (`wc`):**
  ```bash
  wc <filename>   # Shows total lines, words, and characters in a file
  ```

---

## 2. Paths, Links & Wildcards

### File Paths
* **Root Slash (`/`):** At the very start of a path (`/`), it means the main root directory of Linux. Between names (`/etc/passwd`), it separates folders.
* **Home Directories:** `/root` is the home folder for the `root` user; `/home/<username>` is the home folder for normal users.
* **Absolute Path:** The full path starting from `/` (e.g., `/etc/ssh/sshd_config`).
* **Relative Path:** A path starting from your current folder without a leading `/` (e.g., `documents/notes.txt`).

### Creating Multiple Files, Tree View & Links
* **Brace Expansion (`{}`):** Create multiple folders or files at once:
  ```bash
  mkdir dir{1..5}
  touch file{a,b,c}.txt
  ```
* **Tree View:**
  ```bash
  tree            # Show all folders and files in a tree layout
  ```
* **Inodes, Soft Links & Hard Links:**
  ```bash
  ls -i <file>                              # Check the file's inode number
  ls -li                                    # Detailed list showing inode numbers and links
  ln -s ~/file /target/location/link_name   # Soft Link (shortcut — stops working if the original file is deleted)
  ln ~/file /target/location/link_name      # Hard Link (shares the same inode — still works even if the original file is deleted)
  ```

### Pattern Matching (Wildcards)
* **`*`:** Matches all files and folders in the current directory (e.g., `mv * /tmp/` moves everything to `/tmp/`).
* **`[ ]`:** Matches any single character inside the brackets (e.g., `ls [ab]*` shows files and folders starting with `a` or `b`).

---

## 3. Variables & Input/Output Redirection

### Shell & Environment Variables
```bash
HI="I am a good boy!"    # Save text in a variable (no spaces around '=')
echo $HI                 # Print the variable's value
env                      # View all system environment variables
```

### Saving Output & Errors to Files
| Symbol | What It Does | Example |
| :--- | :--- | :--- |
| `>` | Saves command output to a file (**overwrites** old content) | `ls -la > output.txt` |
| `>>` | Adds command output to the end of a file (**appends** without deleting old data) | `echo "new line" >> output.txt` |
| `2>` | Shows normal output on screen, but saves **errors** into a file | `find /etc -name passwd 2> error.txt` |
| `&>` | Saves **both** normal output and errors into the same file | `find /etc -name passwd &> all.txt` |

---

## 4. User & Group Management

### User IDs (UID) & System Files
* **UID `0`:** Default ID for the `root` user.
* **UID `1000` – `60000`:** Default ID range for normal users.
* **`/etc/passwd`:** Lists all users on the system in 7 fields:
  ```text
  user6:x:1024:1024:Comment:/home/user6:/bin/bash
  # username : password_placeholder : UID : GID : comment : home_folder : shell
  ```
* **`/etc/shadow`:** Shows encrypted user passwords and whether a password is set (only `root` can read this file).

### Creating, Editing & Deleting Users
```bash
useradd <username>                        # Create a new user
passwd <username>                         # Set or change a user's password
userdel <username>                        # Delete a user (keeps their home folder)
userdel -r <username>                     # Delete a user along with their /home folder

# Edit User Settings
usermod -u <new_uid> <username>           # Change a user's UID
usermod -c "Comment Text" <username>      # Add a description/comment to a user
usermod -l <new_name> <old_name>          # Change a user's login name
usermod -d /new/home/dir <username>       # Change a user's home folder
usermod -s /sbin/nologin <username>       # Block a user from logging in
usermod -s /bin/bash <username>           # Give a user normal Bash shell login access
```

### Managing Groups & Sudo Access
```bash
groupadd <groupname>                      # Create a new group
groupmod -n <new_name> <old_name>         # Rename a group
groups <username>                         # Check which groups a user belongs to
usermod -g <groupname> <username>         # Set a user's primary group
usermod -aG <groupname> <username>        # Add a user to extra groups without removing current ones
gpasswd -a <username> <groupname>         # Add a user to a group
gpasswd -d <username> <groupname>         # Remove a user from a group
visudo                                    # Edit /etc/sudoers to give sudo (admin) rights to a user or group
```

### Password Expiry Settings (`chage`) & Login Messages
```bash
chage -l <username>                       # Check when a user's password was changed and when it expires
chage -M <days> <username>                # Set max days before the user must change their password
chage -m <days> <username>                # Set min days before a password can be changed again
chage -W <days> <username>                # Set how many days before expiry a warning appears
chage -I <days> <username>                # Set inactive days allowed after password expiry
chage -E YYYY-MM-DD <username>            # Set an exact expiry date for the user account
```
* **`/etc/login.defs`:** Default rules for new users (UID ranges, password expiry days, etc.).
* **`~/.bashrc`:** User startup file; adding `echo "Welcome"` here displays a custom message every time you open a terminal.

---

## 5. File Permissions, Ownership & Special Permissions

### Reading Permissions
```text
drwxr-xr-x  root  root  folder_name
│└┬┘└┬┘└┬┘   │     │
│ │  │  │    │     └── Group
│ │  │  │    └──────── Owner
│ │  │  └───────────── Others (everyone else)
│ │  └──────────────── Group permissions
│ └─────────────────── Owner permissions
└───────────────────── File type ('d' = folder, '-' = file)
```
* **Default Permissions:** Folders start with `777` and files start with `666` (before `umask` is applied).

### Symbolic & Numeric Permissions (`chmod`)
* **Numbers:** `Read (r) = 4`, `Write (w) = 2`, `Execute (x) = 1`
  * `0` (`---`): No permission
  * `4` (`r--`): Read only
  * `5` (`r-x`): Read + Execute
  * `6` (`rw-`): Read + Write
  * `7` (`rwx`): Read + Write + Execute
* **Symbolic Commands (`u` = user, `g` = group, `o` = others, `a` = all):**
  ```bash
  chmod u+rwx <target>        # Give full permissions to owner
  chmod g+rwx <target>        # Give full permissions to group
  chmod o+rwx <target>        # Give full permissions to others
  chmod a-rwx <target>        # Remove all permissions from everyone
  chmod 755 <target>          # Numeric way to set rwxr-xr-x
  ```

### Changing Owner, Group & Default Mask (`chown`, `chgrp`, `umask`)
```bash
chown <username> <target>                 # Change the owner of a file or folder
chgrp <groupname> <target>                # Change the group of a file or folder
chown <username>:<groupname> <target>     # Change both owner and group at once
umask                                     # View current default permission mask
umask 022                                 # Change default permissions for new files/folders (add to ~/.bashrc to keep permanent)
```

### Special Permissions (SUID, SGID, Sticky Bit)
| Permission | Number | Command | What It Does |
| :--- | :--- | :--- | :--- |
| **SUID** (User) | `4` (`4000`) | `chmod u+s <file>` | Runs the file with the **owner's** permissions instead of the current user's |
| **SGID** (Group) | `2` (`2000`) | `chmod g+s <dir>` | Any new file created inside this folder automatically gets the **folder's group** |
| **Sticky Bit** (Others) | `1` (`1000`) | `chmod o+t <dir>` | Users cannot delete other users' files inside a shared folder (like `/tmp`) |

---

## 6. Networking, Hostname & SSH

### Network Connections (`nmcli` & `nmtui`)
```bash
ping <ip_or_domain>                       # Check if internet or network connection is working
nmcli connection show                     # See all network connections on the system
nmcli connection up "<conn_name>"         # Turn on a network connection
nmcli connection down "<conn_name>"       # Turn off a network connection
nmcli connection delete "<conn_name>"     # Delete a network connection
nmtui                                     # Open a simple text menu to create or edit network connections
```

### Checking & Changing Hostname
```bash
hostname                                  # Check the current hostname
hostnamectl set-hostname <new_hostname>   # Set a new hostname
bash                                      # Refresh the shell so the new hostname shows up right away
```

### SSH (Secure Shell) & Passwordless Login
* **Config File:** `/etc/ssh/sshd_config` (main configuration file for the SSH service).
* **Start / Stop SSH Service:**
  ```bash
  systemctl enable --now sshd             # Start SSH service and keep it enabled on boot
  systemctl disable --now sshd            # Stop SSH service and block incoming SSH connections
  ```
* **Connect to Another Machine:**
  ```bash
  ssh <target_ip>                           # Connect using your current username
  ssh <username>@<target_ip_or_hostname>    # Connect using a specific username
  ```
* **How to Set Up Passwordless SSH Login:**
  1. Go to your `.ssh` folder:
     ```bash
     cd ~/.ssh
     ```
  2. Generate SSH keys (press `Enter` 3 times):
     ```bash
     ssh-keygen
     ```
  3. Copy your public key to the remote machine:
     ```bash
     ssh-copy-id <username>@<target_ip>
     ```
  4. Enter the remote password one last time. Next time, running `ssh <username>@<target_ip>` will log you in directly without a password.
  *(To remove passwordless login, delete the key from `~/.ssh/authorized_keys` on the remote machine or delete your local private key.)*

---

## 7. RHEL Cockpit Web Console

**Cockpit** lets you manage and monitor a RHEL server directly from a web browser on port `9090`.

```bash
systemctl status cockpit                  # Check if Cockpit is running
systemctl enable --now cockpit            # Start and enable Cockpit
systemctl disable --now cockpit           # Stop and disable Cockpit

# Open port 9090 in the firewall
firewall-cmd --permanent --add-port=9090/tcp
firewall-cmd --reload
```
* **Open in Browser:** `https://<hostname_or_ip>:9090`
