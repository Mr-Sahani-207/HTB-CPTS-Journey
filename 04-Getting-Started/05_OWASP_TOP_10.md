POINT NO 1. WHAT IS OWASP TOP 10

**OWASP** ka full form hai **Open Worldwide Application Security Project**. Ye ek global, non-profit security community hai.

**OWASP Top 10** ek regularly updated document/report hai jo **Web Applications ke top 10 sabse critical aur dangerous security vulnerabilities** ki list provide karti hai. Iska maksad developers, pentesters aur organizations ko ye batana hota hai ki hackers kin kamzoriyo ka sabse zyada fayda utha rahe hain.

**OWASP Top 10 Vulnerabilities (Detailed Breakdown)**

- **A01: Broken Access Control (Number 1 Khatra)**
    
    - User ko aisi files ya dashboards ka access mil jana jiska use haq nahi hai.
        
    - _Example:_ Ek normal user URL me `user_id=101` ki jagah `user_id=1` karke admin ka account dekh le (IDOR - Insecure Direct Object Reference).
        
- **A02: Cryptographic Failures (Pehle: Sensitive Data Exposure)**
    
    - Passwords, credit card ya tokens ko bina encryption ke plaintext me store ya transmit karna, ya weak/purana encryption algorithm use karna.
        
- **A03: Injection**
    
    - Attacker form input ya URL me malicious code daal deta hai, aur server use normal data samajhkar execute kar deta hai.
        
    - _Example:_ **SQL Injection (SQLi)** jisse pura database dump ho jaye, ya **Command Injection** jisse server par direct reverse shell mil jaye.
        
- **A04: Insecure Design**
    
    - Application ke code likhne se pehle hi architecture ya architecture design me flaws chhod dena (e.g., password recovery flow me koi rate-limiting ya security verification na rakhna).
        
- **A05: Security Misconfiguration**
    
    - Default passwords na badalna (`admin:admin`), unnecessary ports ya services open chhodna, ya debug errors me full system details display karna.
        
- **A06: Vulnerable and Outdated Components**
    
    - Aisi purani libraries, plugins ya frameworks use karna jinka public exploit internet par pehle se maujood ho (e.g., outdated WordPress plugin ya Log4j vulnerability).
        
- **A07: Identification and Authentication Failures**
    
    - Login mechanism kamzor hona—bina brute-force protection ke login form, weak session IDs, ya MFA (Two-Factor Authentication) ka na hona.
        
- **A08: Software and Data Integrity Failures**
    
    - Code, updates ya data ko bina digital signature verify kiye accept karna (e.g., unverified CDN libraries load karna ya insecure deserialization).
        
- **A09: Security Logging and Monitoring Failures**
    
    - Server par login attempts ya attacks ke logs maintain na karna. Agar koi hacker system me ghus bhi jaye, toh alert na aana aur pata hi na chalna.
        
- **A10: Server-Side Request Forgery (SSRF)**
    
    - Web server ko trick karke internal network ke un systems se data request karwana jo bahar internet se direct accessible nahi hote (e.g., internal metadata service access karna).

POINT 2. SUMMARY TABLE OF OWASP TOP 10

|**Rank**|**Vulnerability**|**Core Problem**|**Real-World Impact**|
|---|---|---|---|
|**A01**|**Broken Access Control**|Authorization bypass|Unauthorized user account / admin takeover|
|**A02**|**Cryptographic Failures**|Plaintext ya weak crypto|Passwords & payment data leaks|
|**A03**|**Injection**|Unfiltered user input|SQL Database dump ya remote shell execution|
|**A04**|**Insecure Design**|Flawed business logic|Core application flow manipulation|
|**A05**|**Security Misconfiguration**|Default setup & verbose errors|Sensitive server infrastructure leaks|
|**A06**|**Outdated Components**|Known CVEs in libraries|1-click remote code execution|
|**A07**|**Auth Failures**|Weak login / session controls|Brute-force & credential stuffing|
|**A08**|**Integrity Failures**|Untrusted updates/deserialization|Supply-chain attack / code tampering|
|**A09**|**Logging Failures**|Inadequate monitoring|Undetected breaches for months|
|**A10**|**SSRF**|Server makes untrusted requests|Internal AWS/cloud metadata access|
