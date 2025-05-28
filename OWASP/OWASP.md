# 1. OWASP (Open Web Application Security Project)

**What it is:**  
The _OG_ in web app security. OWASP is a nonprofit that publishes the famous **OWASP Top 10**, which is basically the "top 10 baddest security risks" for web apps. Think of it like a hacker's cheat sheet for the most common vulnerabilities.

---

# 2. HTTPX (HTTP Exploitation tool & HTTP basics)

You said **all versions** and **request/response details**, so imma break it down real smooth.

### HTTP Versions:

- **HTTP/0.9:** The first one. Super basic, no headers, just raw HTML response.
    
- **HTTP/1.0:** Introduced headers, status codes, and request methods.
    
- **HTTP/1.1:** Added persistent connections, chunked transfer encoding, and more headers. Still the most used.
    
- **HTTP/2:** Binary framing, multiplexing, header compression — faster and more efficient.
    
- **HTTP/3:** Uses QUIC (UDP-based), super low latency and better performance.
    

### HTTP Request Components:

- **Request line:**
    
    - Method (GET, POST, PUT, DELETE, etc.)
        
    - URL/Path
        
    - HTTP Version
        
- **Headers:** Key-value pairs giving info like user-agent, content-type, cookies, etc.
    
- **Body:** Data sent with POST/PUT requests (like form data, JSON).
    

### HTTP Response Components:

- **Status Line:**
    
    - HTTP Version
        
    - Status Code (like 200, 404)
        
    - Reason Phrase ("OK", "Not Found")
        
- **Headers:** Meta info about the response (content-type, length, caching).
    
- **Body:** Actual response content (HTML, JSON, images, etc.)
    

---

### Status Codes — The full squad:

|Code Range|Meaning|Examples|
|---|---|---|
|1xx|Informational|100 Continue|
|2xx|Success|200 OK, 201 Created|
|3xx|Redirection|301 Moved Permanently, 302 Found|
|4xx|Client errors|400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found|
|5xx|Server errors|500 Internal Server Error, 503 Service Unavailable|

---

### Request Headers — the main players:

- **Host:** Domain of the server.
    
- **User-Agent:** Browser or client type info.
    
- **Accept:** What content types the client can handle.
    
- **Content-Type:** Format of the data in the request body.
    
- **Authorization:** Credentials for authentication.
    
- **Cookie:** Stored data sent to the server.
    
- **Referer:** URL of the page that linked to the request URL.
    
- **Cache-Control:** Directives for caching.
    

### Response Headers — important ones:

- **Content-Type:** Type of returned content.
    
- **Content-Length:** Size of response body.
    
- **Set-Cookie:** Set cookies on the client.
    
- **Cache-Control:** How long response can be cached.
    
- **Location:** URL for redirection.
    
- **Server:** Server software info.
    
- **Strict-Transport-Security:** Enforces HTTPS.
    

---

# 3. SOP (Same-Origin Policy)

**What it is:**  
Browser security rule that says scripts from one origin (domain, protocol, port) can't mess with resources from another origin. Keeps your data from being snatched by evil sites.

- **Client-side policy** enforced by browsers.
    
- Stops cross-site scripting & data theft.
    

---

# 4. Google Dorks (Google hacking)

**Operations:**  
Using advanced Google search queries to find vulnerable sites or sensitive info accidentally exposed. Example:

`filetype:sql "password"` or `intitle:"index of" "backup"`

- Scours the internet for juicy info.
    
- Used for reconnaissance by pentesters or hackers.
    

---

# 5. Types of Tests

- **White-box testing:** Hacker knows everything — source code, design, etc.
    
- **Black-box testing:** Hacker knows nothing, attacks blindly.
    
- **Grey-box testing:** Partial knowledge, usually credentials or architecture.
    

---

# 6. Vulnerability, CVE, CVSS

- **Vulnerability:** Weakness in software or system.
    
- **CVE (Common Vulnerabilities and Exposures):** Publicly disclosed vulnerability with a unique ID.
    
- **CVSS (Common Vulnerability Scoring System):** Scores vulnerabilities (0 to 10) based on impact, exploitability, etc.
    

---

# 7. BAC (Broken Access Control)

### Risks:

- Attackers access data or functions they shouldn’t.
    
- Directory traversal lets hackers grab files outside intended folders.
    

### Proof:

- Accessing `/etc/passwd` on a Linux server via `../` paths.
    
- Bypassing login controls.
    

### Remediation:

- Validate and sanitize all user inputs.
    
- Use proper authorization checks.
    
- Server-side enforcement.
    
- **Server-side vulnerability**
    

---

# 8. Cryptography Failure

### Risks:

- Unencrypted communications allow MITM (Man-in-the-middle).
    
- Weak or broken encryption leads to data leaks.
    

### Proof:

- Sniffing passwords or tokens in HTTP (instead of HTTPS).
    
- Using outdated ciphers.
    

### Remediation:

- Use TLS/SSL everywhere.
    
- Enforce strong ciphers and key management.
    
- **Client + Server side**
    

---

# 9. Injections

### SQL Injection Types:

- **Classic:** Insert malicious SQL in input fields.
    
- **Blind SQLi:** No direct output, attacker infers info via true/false.
    
- **Time-based:** Delays in response reveal info.
    
- **Error-based:** Uses DB errors to gather info.
    
- **Union-based:** Combines multiple queries.
    

### Command Injection:

- Executes shell commands on server through input.
    

### XSS (Cross-Site Scripting) Types:

- **Stored:** Malicious script saved in DB.
    
- **Reflected:** Script reflected off user input in URL.
    
- **DOM-based:** Malicious client-side scripts manipulate DOM.
    

### LFI (Local File Inclusion):

- Includes local files on server, often leading to RCE (remote code execution).
    

### Risks:

- Data theft, full server takeover, session hijacking.
    

### Proof:

- Test input fields with `' OR '1'='1` or `<script>alert(1)</script>`
    

### Remediation:

- Use prepared statements/parameterized queries.
    
- Sanitize all inputs.
    
- Use Content Security Policy (CSP) for XSS.
    
- Disable file inclusion where not needed.
    
- **Server-side and client-side (XSS)**
    

---

# 10. Insecure Design

### Risks:

- Information leakage: sensitive data visible.
    
- Unsafe file uploads: malware uploads.
    

### Proof:

- Info found in error messages.
    
- Uploading `.php` or `.exe` to bypass restrictions.
    

### Remediation:

- Minimal info in errors.
    
- Validate file types, sizes, and scan uploads.
    

---

# 11. Security Misconfiguration

### Risks:

- Default passwords, debug enabled in prod, outdated components.
    

### Proof:

- Accessing admin panels without login.
    
- Using old versions with known exploits.
    

### Remediation:

- Harden configurations.
    
- Regular patching.
    
- Disable unnecessary services.
    

---

# 12. Identification and Authentication Failures

### Risks:

- Password mismanagement: weak or reused passwords.
    
- Privilege escalation: users gain admin rights.
    
- User enumeration: attackers guess valid users.
    
- Session fixation: attacker fixes session ID for victim.
    
- Weak session IDs.
    

### Proof:

- Brute force login pages.
    
- Session ID predictable or reused.
    

### Remediation:

- Strong password policies.
    
- Multi-factor authentication.
    
- Secure session management.
    
- Rate limiting login attempts.
    

---

# 13. Defaillance d'intégrité des logiciels et des données

(Software and data integrity failure)

### Risks:

- Tampering with software/data without detection.
    

### Remediation:

- Use code signing.
    
- Validate file integrity with hashes.
    
- Secure backups.
    

---

# 14. Security Logging and Monitoring Failures

### Risks:

- Attacks go unnoticed.
    
- No audit trails for incident response.
    

### Remediation:

- Implement centralized logging.
    
- Monitor for suspicious activity.
    
- Alerting on anomalies.
    

---

# 15. SSRF (Server-Side Request Forgery)

### Risks:

- Server makes unintended requests internally or externally.
    
- Attacker accesses internal systems.
    

### Proof:

- Input URL that points to internal IPs (like 127.0.0.1).
    

### Remediation:

- Validate and whitelist URLs.
    
- Disable unused protocols.
    
- **Server-side**
    

---

# Summary Table (Risk, Proof, Remediation, Client/Server Side):

|Vulnerability|Risk|Proof|Remediation|Side|
|---|---|---|---|---|
|BAC (Dir traversal, CSRF)|Unauthorized data/function access|`../etc/passwd`, CSRF tokens missing|Validate input, auth checks|Server|
|Cryptography Failure|Data interception|HTTP sniffing, weak cipher|Use TLS, strong crypto|Client + Server|
|Injections (SQLi, XSS, Cmd Inj)|Data theft, RCE, session hijack|`' OR '1'='1`, `<script>alert(1)`|Prepared statements, sanitize|Server + Client|
|Insecure Design|Info leaks, malware upload|Error details, upload malicious|Minimal errors, validate uploads|Server|
|Security Misconfiguration|Exploitable defaults, outdated SW|Admin access, old version exposed|Harden configs, patch regularly|Server|
|Auth Failures|Account takeover, privilege abuse|Brute force, session fixation|MFA, strong sessions, rate limit|Server|
|Integrity Failures|Tampering undetected|File checksum mismatches|Code signing, hashing|Server|
|Logging & Monitoring Failures|Attacks unnoticed|No logs, no alerts|Central logging, alerts|Server|
|SSRF|Internal resource access|Internal IP requests|Validate URLs, whitelist|Server|

---
# **Google Dork Operators List**

|Operator|What it does|Example|
|---|---|---|
|`site:`|Search within a specific website or domain|`site:example.com`|
|`intitle:`|Pages with specific word(s) in the title|`intitle:"login page"`|
|`inurl:`|URLs containing specific word(s)|`inurl:admin`|
|`intext:`|Pages containing specific word(s) in the text|`intext:"confidential"`|
|`filetype:`|Search for specific file types|`filetype:pdf` or `filetype:sql`|
|`ext:`|Same as filetype (older syntax)|`ext:log`|
|`allinurl:`|All words must be in the URL|`allinurl: admin login`|
|`allintitle:`|All words must be in the page title|`allintitle: confidential data`|
|`allintext:`|All words must appear in page text|`allintext: password file`|
|`cache:`|Show cached version of a page|`cache:example.com`|
|`link:`|Pages that link to a specific URL|`link:example.com`|
|`related:`|Find sites related to a URL|`related:example.com`|
|`info:`|Info about a URL|`info:example.com`|
|`define:`|Definitions of a word|`define:encryption`|
|`inanchor:`|Search for words in anchor text of links|`inanchor:"click here"`|
|`daterange:`|Search within a date range (Julian date format)|`daterange:2458850-2458855`|
|`AROUND(X)`|Finds pages where words are within X words apart|`"admin" AROUND(3) "login"`|
|`-` (minus)|Exclude terms|`password -example`|
|`"` (quotes)|Exact phrase match|`"confidential data"`|
|`*` (wildcard)|Placeholder for any word|`"best * hacking tools"`|
|`OR`|Search for either term|`admin OR root`|
|`allinanchor:`|All words must appear in anchor text|`allinanchor: admin login`|
|`source:`|Search news from specific source|`source:bbc`|
|`weather:`|Get weather info|`weather:London`|

---

# Bonus: Common Google Dork Queries for Hackers

- `filetype:sql "password"` — Find exposed SQL database dumps.
    
- `intitle:"index of" passwd` — Directory listings with password files.
    
- `inurl:wp-admin` — WordPress admin login pages.
    
- `ext:log error` — Exposed log files.
    
- `"index of" "backup"` — Backup files exposed via directory listing.
    

---
# 1. **Directory Traversal**

- **Risk:**  
    Attackers can access files and directories outside the intended folder, exposing sensitive system files (`/etc/passwd`, config files, source code).
    
- **Proof:**  
    Using Google dorks like `inurl:../../etc/passwd` or `filetype:log "password"` you might find files that shouldn’t be public.
    
- **Remediation:**
    
    - Validate and sanitize all user inputs in URLs and file paths.
        
    - Use secure API calls that restrict file access.
        
    - Implement whitelist of accessible directories.
        
    - Harden server configs to deny traversal characters (`../`).
        
- **Side:** Server-side
    

---

# 2. **CSRF (Cross-Site Request Forgery)**

- **Risk:**  
    Unauthorized commands sent from a user’s browser without their consent, e.g., changing passwords, transferring money.
    
- **Proof:**  
    Look for forms or endpoints lacking CSRF tokens. Can test by trying to submit actions from third-party domains.
    
- **Remediation:**
    
    - Implement anti-CSRF tokens (Synchronizer tokens).
        
    - Use SameSite cookies and validate Origin/Referer headers.
        
    - Require user re-authentication for sensitive actions.
        
- **Side:** Both client and server (client triggers attack; server must validate)
    

---

# 3. **Cryptography Failure (e.g., Unencrypted Communication)**

- **Risk:**  
    Data is sent over unencrypted channels (HTTP instead of HTTPS), exposing credentials, session tokens, personal data to eavesdroppers.
    
- **Proof:**  
    Google dorks like `site:example.com -inurl:https` or manually inspecting traffic with tools like Wireshark.
    
- **Remediation:**
    
    - Enforce HTTPS with valid SSL/TLS certificates.
        
    - Use HSTS headers to force HTTPS.
        
    - Avoid deprecated crypto algorithms.
        
- **Side:** Client & Server (client must check TLS; server must enforce)
    

---

# 4. **SQL Injection (SQLi)**

- **Risk:**  
    Injected SQL code can read, modify, or delete database info, dump credentials, or escalate privileges.
    
- **Proof:**
    
    - Using dorks to find vulnerable inputs like `inurl:login.php?id=1'`
        
    - Manual tests with `' OR '1'='1` or `' UNION SELECT...`.
        
- **Types:**
    
    - **Error-based**: Extracts data via error messages.
        
    - **Union-based**: Combines results from multiple queries.
        
    - **Blind (Boolean, Time-based)**: No direct output; infer by true/false or time delays.
        
    - **Out-of-band**: Data retrieved through alternative channels.
        
- **Remediation:**
    
    - Use parameterized queries (prepared statements).
        
    - Validate and sanitize inputs.
        
    - Limit database permissions.
        
    - Employ Web Application Firewalls (WAF).
        
- **Side:** Server-side
    

---

# 5. **Command Injection**

- **Risk:**  
    Attackers run arbitrary OS commands on the server, risking full system takeover.
    
- **Proof:**  
    Google dorks exposing admin panels or upload forms, testing inputs with `; ls` or `&& whoami`.
    
- **Remediation:**
    
    - Validate, sanitize all inputs.
        
    - Avoid direct shell calls with user input.
        
    - Use safe APIs or escape inputs.
        
    - Implement least privilege for system commands.
        
- **Side:** Server-side
    

---

# 6. **XSS (Cross-Site Scripting)**

- **Risk:**  
    Inject malicious scripts into web pages viewed by other users, stealing cookies, session tokens, or defacing sites.
    
- **Proof:**  
    Google dorks targeting search boxes or comments, trying scripts like `<script>alert(1)</script>`.
    
- **Types:**
    
    - **Stored:** Malicious code saved on server, delivered to multiple users.
        
    - **Reflected:** Code reflected off a web server (URL or form).
        
    - **DOM-based:** Happens in client-side script, no server involvement.
        
- **Remediation:**
    
    - Escape and encode all outputs.
        
    - Use Content Security Policy (CSP).
        
    - Sanitize user inputs.
        
    - Use frameworks that auto-escape.
        
- **Side:** Client-side (in browser) but also server-side (where data is saved or reflected)
    

---

# 7. **LFI (Local File Inclusion)**

- **Risk:**  
    Attackers include files from the local server, leading to information disclosure or code execution.
    
- **Proof:**  
    Dorks with `inurl:"page=../../etc/passwd"` or testing parameters like `?page=../../../../etc/passwd`.
    
- **Remediation:**
    
    - Sanitize file include parameters.
        
    - Use whitelists for allowed files.
        
    - Disable unnecessary file inclusion.
        
    - Keep error reporting minimal.
        
- **Side:** Server-side
    

---

# 8. **Insecure Design (Info Leakage, File Upload)**

- **Risk:**  
    Flaws in system design expose sensitive info or allow malicious file uploads (leading to malware).
    
- **Proof:**
    
    - Google dorks revealing backup files, config files, or upload portals.
        
    - Uploading webshells via insecure upload forms.
        
- **Remediation:**
    
    - Implement strict access control and validation.
        
    - Validate and restrict file types/extensions.
        
    - Scan uploaded files for malware.
        
    - Hide sensitive data, disable directory listing.
        
- **Side:** Server-side
    

---

# 9. **Security Misconfiguration**

- **Risk:**  
    Default accounts, unnecessary services, verbose errors, open ports, outdated components lead to easy breaches.
    
- **Proof:**  
    Dorks like `intitle:"Apache2 Debian Default Page"` or `filetype:log password`.
    
- **Remediation:**
    
    - Harden server & app configs.
        
    - Disable unused services.
        
    - Update components regularly.
        
    - Hide detailed error messages from public.
        
- **Side:** Server-side
    

---

# 10. **Authentication & Identification Failures**

- **Risk:**  
    Password mismanagement, privilege escalation, user enumeration, session fixation lead to account takeover.
    
- **Proof:**
    
    - Testing login forms for error differences.
        
    - Checking session IDs for predictability.
        
    - Attempting privilege changes via URL tampering.
        
- **Remediation:**
    
    - Enforce strong password policies.
        
    - Use multi-factor authentication (MFA).
        
    - Avoid revealing user existence in error messages.
        
    - Regenerate session IDs on login.
        
- **Side:** Both client and server
    

---

# 11. **Software & Data Integrity Failures**

- **Risk:**  
    Tampering with software or data causes corrupted or malicious operations.
    
- **Proof:**
    
    - Look for unsigned or tampered files.
        
    - Exploit update mechanisms.
        
- **Remediation:**
    
    - Use code signing and integrity checks.
        
    - Validate updates via cryptographic signatures.
        
- **Side:** Server-side
    

---

# 12. **Security Logging and Monitoring Failures**

- **Risk:**  
    Lack of logs or alerts delays detection of breaches, enabling attacker persistence.
    
- **Proof:**  
    Absence of logs on suspicious activities.
    
- **Remediation:**
    
    - Implement centralized logging.
        
    - Alert on suspicious activities.
        
    - Regularly audit logs.
        
- **Side:** Server-side
    

---

# 13. **SSRF (Server-Side Request Forgery)**

- **Risk:**  
    Server is tricked to make requests to internal resources or external malicious servers.
    
- **Proof:**  
    Inputs like URLs that the server fetches can be manipulated. Test with private IPs: `http://127.0.0.1/admin`.
    
- **Remediation:**
    
    - Validate and whitelist allowed URLs.
        
    - Disable unnecessary URL fetching.
        
    - Use network segmentation.
        
- **Side:** Server-side
    
