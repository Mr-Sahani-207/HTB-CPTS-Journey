### Section 8 — Web Enumeration & Source Code Disclosure

* **Objective:** Enumerate hidden directories and files on the target web application, inspect exposed metadata and client-side source code for hardcoded credentials, and authenticate to retrieve the flag.

* **Steps & Commands:**
  1. **Directory & File Brute-Forcing:**
     ```bash
     gobuster dir -u http://<TARGET_IP>:<PORT> -w /usr/share/wordlists/dirb/common.txt
     ```
     * **Finding:** Discovered `/wordpress`, `index.php`, and `robots.txt`.

  2. **`robots.txt` & Hidden Endpoint Discovery:**
     ```bash
     curl -s http://<TARGET_IP>:<PORT>/robots.txt
     ```
     * **Finding:** Identified a disallowed administrative login path (`/[REDACTED].php`).

  3. **HTML Source Code Review & Authentication:**
     * Inspected the page source (`view-source:`) of the disallowed login page and discovered a hardcoded developer comment exposing test credentials (`REDACTED:[REDACTED]`).
     * Authenticated via the web login form using the discovered credentials and retrieved the flag (`HTB{REDACTED}`).

* **Takeaway:** Always inspect `robots.txt` for disallowed paths and review client-side HTML source code (`view-source:`) on login portals—developers frequently leave sensitive paths and test credentials in HTML comments.
