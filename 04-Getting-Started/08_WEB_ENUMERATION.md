# 8. Web Enumeration

The primary objective of **Web Enumeration** is to uncover hidden pages, directories, files, CMS technologies, and parameters on a target website that are not visible through normal browser navigation.

When visiting `http://<TARGET_IP>` in a browser, only the public **Home Page** (such as About Us or Contact) is displayed. However, developers often leave sensitive directories and files on the web server without linking them on the main page—for example:
* `http://<TARGET_IP>/admin`
* `http://<TARGET_IP>/backup`
* `http://<TARGET_IP>/passwords.txt`

Since manually guessing thousands of directory and file names is impractical, automated fuzzing and enumeration tools are used.

---

## 1. Gobuster (Directory & File Brute-Forcing)

Gobuster is a fast automated enumeration tool that uses a **Wordlist (Dictionary)** to discover hidden directories and files on a web server:
1. Gobuster reads the first entry from a wordlist (e.g., `admin`).
2. It sends an HTTP request to `http://<TARGET_IP>/admin`.
3. If the server responds with **`200 OK`** or **`301 Redirect`**, Gobuster flags the path as a valid discovery.
4. If the server returns **`404 Not Found`**, Gobuster ignores it and moves to the next entry.

### Key Switches & Wordlists
* **`dir` mode:** Instructs Gobuster to perform directory and file brute-forcing (as opposed to DNS subdomain or cloud bucket enumeration).
* **`common.txt`:** A built-in wordlist on Kali Linux (`/usr/share/wordlists/dirb/common.txt`) containing over 4,000 common directory and file names (such as `admin`, `login`, `uploads`, and `robots.txt`).

### Core Command
```bash
gobuster dir -u http://<TARGET_IP>:<PORT> -w /usr/share/wordlists/dirb/common.txt
```
* **`gobuster dir`:** Launches directory brute-force mode.
* **`-u http://<TARGET_IP>:<PORT>`:** Specifies the target URL.
* **`-w /usr/share/...`:** Specifies the path to the wordlist file.

*Common Findings:* Uncovering hidden paths such as `robots.txt` or CMS installations like `/wordpress`.

---

## 2. cURL (Command-Line Web Client)

**cURL** allows penetration testers to send raw HTTP requests directly from the terminal, inspect HTTP response headers, and read file contents without rendering HTML in a browser.

### Key Commands
* **HTTP Header / Banner Grabbing (Server Fingerprinting):**
  ```bash
  curl -I http://<TARGET_IP>:<PORT>/
  ```
  *Example Output:* `Server: Apache/2.4.41 (Ubuntu)`

* **Reading Remote File Contents:**
  ```bash
  curl http://<TARGET_IP>:<PORT>/robots.txt
  ```
  *Example Output:* `Disallow: /admin-login-page.php`

---

## 3. Manual Page Source Inspection

Inspecting the client-side HTML source code of every discovered web page is an essential enumeration step:
* Press **`Ctrl + U`** in a web browser (or use `curl`) to view the raw HTML source code.
* Developers occasionally leave sensitive information inside HTML comments (`<!-- ... -->`), such as test account credentials, internal API endpoints, or staging paths that can be leveraged for unauthorized access.
