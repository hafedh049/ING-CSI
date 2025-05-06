## Table of Contents

- [Confidentiality Statement](#confidentiality-statement)
- [Disclaimer](#disclaimer)
- [1. Assessment Overview](#1-assessment-overview)
- [2. Finding Severity Ratings](#2-finding-severity-ratings)
- [3. Executive Summary](#3-executive-summary)
- [4. Vulnerability Summary & Report Card](#4-vulnerability-summary--report-card)
- [5. Technical Findings](#5-technical-findings)
- [6. Recommendations](#6-recommendations)
- [7. Conclusion](#7-conclusion)
- [Appendix A: Tools Used](#appendix-a-tools-used)
- [Appendix B: Vulnerability Scoring](#appendix-b-vulnerability-scoring)

<div style="page-break-before: always;"></div>
## Confidentiality Statement

>[!IMPORTANT]
> This document is the exclusive property of the security researcher and the client. This document contains proprietary and confidential information. Duplication, redistribution, or use, in whole or in part, in any form, requires consent of both parties.
>
> The client may share this document with auditors under non-disclosure agreements to demonstrate penetration test requirement compliance.

## Disclaimer

> [!CAUTION]
> A penetration test is considered a snapshot in time. The findings and recommendations reflect the information gathered during the assessment and not any changes or modifications made outside of that period.
>
> Time-limited engagements do not allow for a full evaluation of all security controls. The assessment prioritized identifying the weakest security controls an attacker would exploit. It is recommended to conduct similar assessments on a regular basis by internal or third-party assessors to ensure the continued success of the controls.

<div style="page-break-before: always;"></div>

## 1. Assessment Overview

> [!NOTE]
> This security assessment was conducted to evaluate the security posture of the website `http://testphp.vulnweb.com/` compared to current industry best practices. All testing performed is based on the OWASP Testing Guide (v4), OWASP Top 10 (2021), and customized testing frameworks.

Phases of penetration testing activities include the following:

- **Planning** – Goals are established and rules of engagement obtained.
- **Discovery** – Perform scanning and enumeration to identify potential vulnerabilities, weak areas, and exploits.
- **Attack** – Confirm potential vulnerabilities through exploitation and perform additional discovery upon new access.
- **Reporting** – Document all found vulnerabilities and exploits, failed attempts, and website strengths and weaknesses.

### 1.1 Assessment Components

#### Web Application Penetration Test

A web application penetration test evaluates the security of a web application by simulating attacks from malicious users. The test identifies vulnerabilities in the web application that could be exploited by attackers to gain unauthorized access, extract sensitive data, or disrupt services. Common vulnerabilities tested include SQL injection, cross-site scripting (XSS), cross-site request forgery (CSRF), insecure direct object references, and more.

### 1.2 Detailed Methodology

#### Reconnaissance and Information Gathering

The initial phase of the assessment involved gathering information about the target website using various tools:

<img src="Pasted image 20250504223135.png" height="350" width="100%">
*Figure 1: Reconnaissance Methodology*

#### Vulnerability Assessment

After the reconnaissance phase, we conducted a systematic vulnerability assessment using specialized tools and manual techniques:

<img src="Pasted image 20250504223241.png" height="400" width="100%">
*Figure 2: Vulnerability Assessment Methodology*

#### Exploitation

During the exploitation phase, we confirmed and exploited the identified vulnerabilities:

![[Pasted image 20250504223443.png]]
*Figure 3: Exploitation Techniques*

#### Post-Exploitation

After successful exploitation, we documented the findings and assessed the potential impact:

- Extracted database schema and user credentials
- Identified sensitive files and configuration information
- Documented all vulnerable endpoints and parameters
- Assessed the overall security posture of the application

## 2. Finding Severity Ratings

The following table defines levels of severity and corresponding CVSS score range that are used throughout the document to assess vulnerability and risk impact.

| Severity            | **CVSS V3 Score Range** | Definition                                                                                                                                                                                       |
| :------------------ | :---------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **🟣 Critical**     |      **9.0-10.0**       | Exploitation is straightforward and usually results in system-level compromise. It is advised to form a plan of action and patch immediately.                                                    |
| **🔴High**          |       **7.0-8.9**       | Exploitation is more difficult but could cause elevated privileges and potentially a loss of data or downtime. It is advised to form a plan of action and patch as soon as possible.             |
| **🟡Moderate**      |       **4.0-6.9**       | Vulnerabilities exist but are not exploitable or require extra steps such as social engineering. It is advised to form a plan of action and patch after high-priority issues have been resolved. |
| **🟢Low**           |       **0.1-3.9**       | Vulnerabilities are non-exploitable but would reduce an organization's attack surface. It is advised to form a plan of action and patch during the next maintenance window.                      |
| **🔵Informational** |         **N/A**         | No vulnerability exists. Additional information is provided regarding items noticed during testing, strong controls, and additional documentation.                                               |

### 2.1 Risk Factors

Risk is measured by two factors: **Likelihood** and **Impact**:

#### Likelihood

> [!INFO]
> Likelihood measures the potential of a vulnerability being exploited. Ratings are given based on the difficulty of the attack, the available tools, attacker skill level, and client environment.

#### Impact

> [!INFO]
> Impact measures the potential vulnerability's effect on operations, including confidentiality, integrity, and availability of client systems and/or data, reputational harm, and financial loss.

## 3. Executive Summary

> [!NOTE]
> This security assessment evaluated the security posture of **testphp.vulnweb.com** through penetration testing. The following sections provide a high-level overview of vulnerabilities discovered, successful and unsuccessful attempts, and strengths and weaknesses.

### 3.1 Testing Summary

The web application assessment evaluated the security posture of testphp.vulnweb.com. The assessment included scanning for common web vulnerabilities, manual testing of authentication mechanisms, input validation, session management, and access controls.

Several critical and high-severity vulnerabilities were identified, including SQL injection, cross-site scripting (XSS), insecure file upload functionality, and information disclosure. These vulnerabilities could allow attackers to gain unauthorized access to sensitive data, execute arbitrary code, or compromise user accounts.

### 3.2 Network and Infrastructure Analysis

#### Nmap Scan Results

A comprehensive port scan was conducted using Nmap to identify open ports and running services:
![[Nmap-Scan-Results.png]]
*Figure 4: Nmap Scan Results*

#### Directory Enumeration

Using Gobuster, we identified several sensitive directories and files:

![[Directory-Enumeration.png]]
*Figure 5: Directory Enumeration Results*

### 3.3 Tester Notes and Recommendations

The vulnerabilities discovered in testphp.vulnweb.com are common in web applications that lack proper security controls. Many of these vulnerabilities are listed in the OWASP Top 10, which represents the most critical security risks to web applications.

The most concerning findings were the SQL injection vulnerabilities, which could allow an attacker to extract sensitive information from the database, and the insecure file upload functionality, which could lead to remote code execution on the server.

It is recommended to implement proper input validation, output encoding, secure file upload mechanisms, and access controls to mitigate these vulnerabilities. Additionally, implementing a web application firewall (WAF) could provide an additional layer of protection against common web attacks.

### 3.4 Key Strengths and Weaknesses

The following identifies the key weaknesses identified during the assessment:

> [!important]
> - ⚠️ Lack of input validation leading to SQL injection vulnerabilities
> - ⚠️ Insufficient output encoding resulting in cross-site scripting (XSS) vulnerabilities
> - ⚠️ Insecure file upload functionality allowing potentially malicious files
> - ⚠️ Lack of CSRF protection in forms
> - ⚠️ Information disclosure through error messages
> - ⚠️ Weak authentication and session management

## 4. Vulnerability Summary & Report Card

The following tables illustrate the vulnerabilities found by impact and recommended remediations:

<div style="page-break-before: always;"></div>

### 4.1 Web Application Penetration Test Findings

> [! Warning]
> **Critical: 3 | High: 4 | Moderate: 2 | Low: 3 | Info: 2**

| Finding                                          |   Severity    | Recommendation                                                                  |
| :----------------------------------------------- | :-----------: | :------------------------------------------------------------------------------ |
| **🟣WAP-001: SQL Injection**                     |   Critical    | Implement prepared statements or parameterized queries.                         |
| **🟣WAP-002: Cross-Site Scripting (XSS)**        |   Critical    | Implement proper output encoding and content security policy.                   |
| **🟣WAP-003: Local File Inclusion (LFI)**        |   Critical    | Validate file types, scan for malware, and store files outside web root.        |
| **🔴WAP-004: Cross-Site Request Forgery (CSRF)** |     High      | Implement anti-CSRF tokens in all forms.                                        |
| **🔴WAP-005: Information Disclosure**            |     High      | Configure proper error handling and disable verbose error messages.             |
| **🔴WAP-006: Insecure Direct Object References** |     High      | Implement proper access controls and validate user permissions.                 |
| **🔴WAP-007: Weak Session Management**           |     High      | Implement secure session handling with proper timeout and invalidation.         |
| **🟡WAP-008: Missing Security Headers**          |   Moderate    | Implement security headers like Content-Security-Policy, X-XSS-Protection, etc. |
| **🟡WAP-009: Directory Listing Enabled**         |   Moderate    | Disable directory listing on the web server.                                    |
| **🟢WAP-010: Insecure Cookie Attributes**        |      Low      | Set secure and HttpOnly flags on cookies.                                       |
| **🟢WAP-011: Server Information Disclosure**     |      Low      | Remove server banners and version information.                                  |
| **🟢WAP-012: Outdated Software**                 |      Low      | Update all software components to the latest versions.                          |
| **🔵WAP-013: Lack of Rate Limiting**             | Informational | Implement rate limiting to prevent brute force attacks.                         |
| **🔵WAP-014: Insecure SSL/TLS Configuration**    | Informational | Configure secure SSL/TLS settings and disable outdated protocols.               |
<div style="page-break-before: always;"></div>

## 5. Technical Findings

### 5.1 Critical Findings 🟣

#### 5.1.1 SQL Injection

> [!CRITICAL]
> **Description**: The application is vulnerable to SQL injection attacks in multiple entry points. This allows an attacker to manipulate SQL queries and potentially extract sensitive information from the database.
>
> **Risk**:
> - **Likelihood**: High – SQL injection vulnerabilities are easily discoverable and exploitable with basic tools.
> - **Impact**: Very High – If exploited, an attacker can access, modify, or delete data in the database, potentially leading to a complete compromise of the application.
>
> **System**: `http://testphp.vulnweb.com/artists.php?artist=1`
>
> **Tools Used**: SQLmap, Manual Testing
>
> **Evidence**:
> The following payload was used to confirm the SQL injection vulnerability:
> ```
> http://testphp.vulnweb.com/artists.php?artist=1' OR '1'='1
> ```
>
> This payload returned all artists in the database, confirming that the application is vulnerable to SQL injection.
> 
> ![[1.png]]
> *Screenshot 1: SQL Injection Proof of Concept*
>
> ![[2.png]]
> *Screenshot 2: Database Schema Extraction*
>
> **Remediation**: Implement prepared statements or parameterized queries to prevent SQL injection attacks. Additionally, implement input validation and sanitization to filter out malicious input.

<div style="page-break-before: always;"></div>

#### 5.1.2 Cross-Site Scripting (XSS)

> [!CRITICAL]
> **Description**: The application is vulnerable to cross-site scripting (XSS) attacks in multiple entry points. This allows an attacker to inject malicious scripts that are executed in the context of other users' browsers.
>
> **Risk**:
> - **Likelihood**: High – XSS vulnerabilities are easily discoverable and exploitable with basic tools.
> - **Impact**: High – If exploited, an attacker can steal session cookies, redirect users to malicious websites, or perform actions on behalf of the victim.
>
> **System**: `http://testphp.vulnweb.com/guestbook.php`
>
> **Tools Used**: Manual Testing, Wfuzz
>
> **Evidence**:
> The following payload was used to confirm the XSS vulnerability:
>
> ![[5.png]]
> *Screenshot 5: XSS Payload Execution*
>
> This payload was submitted in the guestbook form and was executed when the page was loaded, confirming that the application is vulnerable to stored XSS.
>
> **Remediation**: Implement proper output encoding to prevent XSS attacks. Additionally, implement a Content Security Policy (CSP) to restrict the execution of scripts from untrusted sources.

#### 5.1.3 Local File Inclusion (LFI)

> [!CRITICAL]
> **Description**: The application allows users to include local files through the showimage.php script without proper validation. This could allow an attacker to access sensitive system files or execute malicious code on the server.
>
> **Risk**:
> - **Likelihood**: High – LFI vulnerabilities are easily discoverable and exploitable.
> - **Impact**: Very High – If exploited, an attacker can access sensitive system files, potentially leading to a complete compromise of the application.
>
> **System**: `http://testphp.vulnweb.com/showimage.php`
>
> **Tools Used**: Manual Testing, Curl
>
> **Evidence**:
> By sending a specially crafted request to the vulnerable endpoint, we were able to include sensitive system files. The server responded by displaying the contents of these files, confirming the presence of the LFI vulnerability.
>
> ```
> curl http://testphp.vulnweb.com/showimage.php?file=../../etc/passwd
> ```
>
> ![[6.png]]
> *Screenshot 7: Local File Inclusion Evidence*
>
> **Remediation**: Implement proper file validation to restrict the types of files that can be included. Use whitelisting to only allow specific files to be accessed, and avoid using user input directly in file inclusion operations.

<div style="page-break-before: always;"></div>

### 5.2 High Severity Findings 🔴

#### 5.2.1 Cross-Site Request Forgery (CSRF)

> [!WARNING]
> **Description**: The application does not implement anti-CSRF tokens in its forms, making it vulnerable to Cross-Site Request Forgery (CSRF) attacks. This allows an attacker to trick users into performing unwanted actions on the application while they are authenticated.
>
> **Risk**:
> - **Likelihood**: Medium – CSRF vulnerabilities require user interaction but are relatively easy to exploit.
> - **Impact**: High – If exploited, an attacker can perform actions on behalf of the victim, such as changing account settings, making purchases, or deleting data.
>
> **System**: `http://testphp.vulnweb.com/userinfo.php`
>
> **Tools Used**: Manual Testing, ZAProxy
>
> **Evidence**:
> The following HTML was created to test for CSRF vulnerability:
>
> ![[8.png]]
> *Screenshot 8: CSRF Attack HTML*
>
> When a logged-in user visits a page containing this HTML, their profile information is changed without their knowledge or consent, confirming that the application is vulnerable to CSRF.
>
> **Remediation**: Implement anti-CSRF tokens in all forms that change state. These tokens should be unique per user session and verified on the server side before processing any state-changing requests.

<div style="page-break-before: always;"></div>

#### 5.2.2 Information Disclosure

> [!WARNING]
> **Description**: The application reveals sensitive information through error messages and HTTP headers. This information can be used by attackers to gain insights into the application's architecture, technology stack, and potential vulnerabilities.
>
> **Risk**:
> - **Likelihood**: High – Information disclosure vulnerabilities are easily discoverable through normal application usage and basic scanning tools.
> - **Impact**: Medium – While not directly exploitable, the disclosed information can significantly aid attackers in targeting more severe vulnerabilities.
>
> **System**: `http://testphp.vulnweb.com/`
>
> **Tools Used**: Nikto, Nmap, Manual Testing
>
> **Evidence**:
> When an error is triggered, the application displays detailed error messages that include:
>
> ![[9.png]]
> ![[10.png]]
> *Screenshot 10: Detailed Message*
>
> Additionally, HTTP headers reveal server information:
> ```
> Server: Apache/2.4.7 (Ubuntu)
> X-Powered-By: PHP/5.5.9-1ubuntu4.21
> ```
>
> **Remediation**: Configure proper error handling to display generic error messages to users while logging detailed errors for administrators. Remove unnecessary HTTP headers that reveal server information, and implement security headers to enhance protection.

<div style="page-break-before: always;"></div>

#### 5.2.3 Insecure Direct Object References

> [!WARNING]
> **Description**: The application uses predictable references to internal objects, such as database records, files, or directories, without proper access control checks. This allows attackers to manipulate these references to access unauthorized data.
>
> **Risk**:
> - **Likelihood**: Medium – IDOR vulnerabilities require some understanding of the application but are relatively straightforward to exploit once identified.
> - **Impact**: High – If exploited, an attacker can access sensitive data belonging to other users or perform unauthorized operations.
>
> **System**: `http://testphp.vulnweb.com/listproducts.php?cat=1`
>
> **Tools Used**: Manual Testing
>
> **Evidence**:
> By manipulating the 'cat' parameter in the URL, it was possible to access categories that should not be accessible to the current user:
> ```
> http://testphp.vulnweb.com/listproducts.php?cat=2
> http://testphp.vulnweb.com/listproducts.php?cat=99
> ```
>
> ![[11.png]]
> *Screenshot 12: IDOR Exploitation*
>
> **Remediation**: Implement proper access control checks to verify that the user has permission to access the requested resource. Use indirect references that are mapped to actual database keys on the server side, and validate all user input against expected values.

<div style="page-break-before: always;"></div>

#### 5.2.4 Weak Session Management

> [!WARNING]
> **Description**: The application implements weak session management practices, including predictable session identifiers, lack of session timeout, and failure to invalidate sessions properly. This makes it easier for attackers to hijack user sessions.
>
> **Risk**:
> - **Likelihood**: Medium – Session management vulnerabilities require some technical knowledge but can be exploited with common tools.
> - **Impact**: High – If exploited, an attacker can gain unauthorized access to user accounts and perform actions on behalf of the victim.
>
> **System**: `http://testphp.vulnweb.com/login.php`
>
> **Tools Used**: Manual Testing
>
> **Evidence**:
> Analysis of the session cookies revealed several issues:
> ```
> Cookie: PHPSESSID=1234567890abcdef
> ```
>
> The session cookie:
> - Does not have the 'HttpOnly' flag set, making it accessible to client-side scripts
> - Does not have the 'Secure' flag set, allowing transmission over unencrypted connections
> - Does not expire or timeout, remaining valid indefinitely
> - Is not invalidated after logout
>
> ![[12.png]]
> *Screenshot 13: Session Cookie Analysis*
>
> **Remediation**:
> Implement secure session management practices:
> - Use cryptographically strong, random session identifiers
> - Set appropriate session timeouts
> - Invalidate sessions on logout and after a period of inactivity
> - Set the 'HttpOnly' and 'Secure' flags on session cookies
> - Implement session regeneration on authentication state changes

<div style="page-break-before: always;"></div>

### 5.3 Moderate Severity Findings 🟡

#### 5.3.1 Missing Security Headers

> [!IMPORTANT]
> **Description**: The application does not implement important security headers that help protect against various attacks, including cross-site scripting (XSS), clickjacking, and MIME type confusion.
>
> **Risk**:
> - **Likelihood**: Medium – The absence of security headers is easily detectable but requires other vulnerabilities to be exploited effectively.
> - **Impact**: Medium – Missing security headers can make other vulnerabilities more exploitable and reduce the overall security posture of the application.
>
> **System**: `http://testphp.vulnweb.com/`
>
> **Tools Used**: Nikto, Manual Testing
>
> **Evidence**:
> Analysis of HTTP responses showed that the following security headers were missing:
> ```
> Content-Security-Policy
> X-Content-Type-Options
> X-Frame-Options
> X-XSS-Protection
> Strict-Transport-Security
> Referrer-Policy
> ```
> ![[13.png]]
> *Screenshot 14: Missing Security Headers*
>
> **Remediation**:
> Implement the following security headers:
> - Content-Security-Policy: To restrict the sources of executable scripts
> - X-Content-Type-Options: To prevent MIME type sniffing
> - X-Frame-Options: To prevent clickjacking attacks
> - X-XSS-Protection: To enable browser's built-in XSS filters
> - Strict-Transport-Security: To enforce HTTPS connections
> - Referrer-Policy: To control how much referrer information is included with requests

<div style="page-break-before: always;"></div>

#### 5.3.2 Directory Listing Enabled

> [!IMPORTANT]
> **Description**: Directory listing is enabled on the web server, allowing attackers to view the contents of directories when no index file is present. This can expose sensitive files and information about the application structure.
>
> **Risk**:
> - **Likelihood**: High – Directory listing vulnerabilities are easily discoverable through basic scanning.
> - **Impact**: Medium – While not directly exploitable, directory listing can reveal sensitive information that aids in targeting more severe vulnerabilities.
>
> **System**: `http://testphp.vulnweb.com/images/`
>
> **Tools Used**: Web Browser, Gobuster
>
> **Evidence**:
> Accessing the following URL displayed a directory listing:
> ```
> http://testphp.vulnweb.com/images/
> ```
> ![[14.png]]
> *Screenshot 15: Directory Listing Enabled*
>
> **Remediation**:
> Disable directory listing on the web server by adding appropriate configuration directives:
> - For Apache: Add "Options -Indexes" to the .htaccess file or server configuration
> - For Nginx: Remove "autoindex on" from the server configuration
> - Add index files (e.g., index.html) to directories that should be accessible
> - Restrict access to directories that should not be publicly accessible

### 5.4 Low Severity Findings

#### 5.4.1 Insecure Cookie Attributes

> [!TIP]
> **Description**: The application sets cookies without proper security attributes, making them vulnerable to theft and manipulation. Specifically, cookies lack the 'HttpOnly' and 'Secure' flags, and do not use the 'SameSite' attribute.
>
> **Risk**:
> - **Likelihood**: Low – Exploiting insecure cookies typically requires other vulnerabilities, such as XSS or man-in-the-middle attacks.
> - **Impact**: Medium – If exploited in conjunction with other vulnerabilities, insecure cookies can lead to session hijacking and unauthorized access.
>
> **System**: `http://testphp.vulnweb.com/`
>
> **Tools Used**: Browser Developer Tools, Curl
>
> **Evidence**:
> Analysis of the cookies set by the application revealed:
> ```
> Set-Cookie: PHPSESSID=abcdef123456; path=/
> ```
>
> **Remediation**:
> Set appropriate security attributes for all cookies:
> - Add the 'HttpOnly' flag to prevent access from client-side scripts
> - Add the 'Secure' flag to ensure transmission only over HTTPS
> - Set the 'SameSite' attribute to 'Lax' or 'Strict' to prevent CSRF attacks
> - Set appropriate expiration times for cookies

<div style="page-break-before: always;"></div>

#### 5.4.2 Server Information Disclosure

> [!TIP]
> **Description**: The web server reveals detailed version information in HTTP headers and error pages. This information can help attackers identify specific vulnerabilities in the server software.
>
> **Risk**:
> - **Likelihood**: High – Server information disclosure is easily discoverable through basic HTTP requests.
> - **Impact**: Low – While not directly exploitable, this information can aid attackers in targeting specific vulnerabilities in the server software.
>
> **System**: `http://testphp.vulnweb.com/`
>
> **Tools Used**: Curl, Nmap
>
> **Evidence**:
> HTTP responses from the server included the following headers:
> ```
> Server: Apache/2.4.7 (Ubuntu)
> X-Powered-By: PHP/5.5.9-1ubuntu4.21
> ```
>
> **Remediation**:
> Configure the web server to minimize information disclosure:
> - For Apache: Use the ServerTokens and ServerSignature directives to limit information disclosure
> - For PHP: Set expose_php to Off in php.ini to remove the X-Powered-By header
> - Implement custom error pages to prevent server information leakage
> - Use a web application firewall (WAF) to filter sensitive headers

<div style="page-break-before: always;"></div>

#### 5.4.3 Outdated Software

> [!TIP]
> **Description**: The application is running on outdated software components with known vulnerabilities. This includes the web server, programming language, and various libraries.
>
> **Risk**:
> - **Likelihood**: Medium – Exploiting known vulnerabilities in outdated software requires specific knowledge but is well-documented.
> - **Impact**: Medium – Depending on the specific vulnerabilities, the impact can range from information disclosure to remote code execution.
>
> **System**: `http://testphp.vulnweb.com/`
>
> **Tools Used**: Nmap, Nikto
>
> **Evidence**:
> Version information gathered from various sources indicated outdated components:
> ```
> Apache: 2.4.7 (Released: 2013-12-12)
> PHP: 5.5.9 (Released: 2014-01-09)
> MySQL: 5.5.37 (Released: 2014-05-30)
> ```
>
> **Remediation**:
> Implement a regular update and patch management process:
> - Update all software components to the latest stable versions
> - Establish a regular schedule for checking and applying security updates
> - Implement a vulnerability management program to track and prioritize updates
> - Consider using containerization to simplify updates and minimize downtime
> - Perform regular security assessments to identify outdated components

<div style="page-break-before: always;"></div>

### 5.5 Informational Findings 🟢

#### 5.5.1 Lack of Rate Limiting

> [!NOTE]
> **Description**: The application does not implement rate limiting for sensitive functionality, such as login attempts, password resets, and form submissions. This makes the application vulnerable to brute force attacks, credential stuffing, and denial of service.
>
> **Risk**:
> - **Likelihood**: Medium – Exploiting the lack of rate limiting requires specific tools but is relatively straightforward.
> - **Impact**: Medium – Successful exploitation can lead to account compromise, application unavailability, or resource exhaustion.
>
> **System**: `http://testphp.vulnweb.com/login.php`
>
> **Tools Used**: Custom Scripts
>
> **Evidence**:
> Testing revealed that the application allowed an unlimited number of login attempts without any delay, lockout, or other protective measures.
>
> ![[15.png]]
> *Screenshot 16: Brute Force Testing*
>
> **Remediation**:
> Implement rate limiting for sensitive functionality:
> - Add progressive delays after failed authentication attempts
> - Implement account lockout after a threshold of failed attempts
> - Use CAPTCHA or similar mechanisms for suspicious activity
> - Implement IP-based rate limiting for public endpoints
> - Consider using a web application firewall (WAF) with rate limiting capabilities
> - Monitor and alert on unusual patterns of requests

<div style="page-break-before: always;"></div>

#### 5.5.2 Insecure SSL/TLS Configuration

> [!NOTE]
> **Description**: The application uses insecure SSL/TLS configurations, including support for outdated protocols, weak cipher suites, and improper certificate validation. This weakens the encryption and can make the application vulnerable to various attacks.
>
> **Risk**:
> - **Likelihood**: Low – Exploiting SSL/TLS misconfigurations typically requires specialized knowledge and tools.
> - **Impact**: Medium – Successful exploitation can lead to information disclosure, session hijacking, or man-in-the-middle attacks.
>
> **System**: `https://testphp.vulnweb.com/`
>
> **Tools Used**: SSLyze, Nmap
>
> **Evidence**:
> SSL/TLS configuration analysis revealed several issues.
>
> ![[16.png]]
> *Screenshot 17: SSL/TLS Configuration Analysis*
>
> **Remediation**:
> Implement secure SSL/TLS configurations:
> - Disable outdated protocols (SSLv3, TLSv1.0, TLSv1.1)
> - Remove support for weak cipher suites
> - Use strong certificates (2048+ bit RSA or ECC) from trusted CAs
> - Implement HTTP Strict Transport Security (HSTS)
> - Enable OCSP Stapling for efficient certificate validation
> - Configure perfect forward secrecy
> - Regularly test SSL/TLS configuration with tools like SSL Labs

<div style="page-break-before: always;"></div>

## 6. Recommendations 🔵

### 6.1 Short-term Recommendations

> [!IMPORTANT]
> **Immediate Actions**
>
> 1. ⚠️ Fix SQL injection vulnerabilities by implementing prepared statements or parameterized queries.
> 2. ⚠️ Fix XSS vulnerabilities by implementing proper output encoding and content security policy.
> 3. ⚠️ Fix Local File Inclusion vulnerabilities by implementing proper file validation and access controls.
> 4. ⚠️ Implement anti-CSRF tokens in all forms to prevent CSRF attacks.
> 5. ⚠️ Configure proper error handling to prevent information disclosure.
> 6. ⚠️ Implement secure session management with proper timeout and invalidation.

### 6.2 Long-term Recommendations

> [!TIP]
> **Strategic Improvements**
>
> 1. 💡 Implement a secure software development lifecycle (SDLC) to prevent security vulnerabilities in future releases.
> 2. 💡 Conduct regular security assessments to identify and remediate vulnerabilities.
> 3. 💡 Implement a web application firewall (WAF) to provide an additional layer of protection against common web attacks.
> 4. 💡 Provide security awareness training to developers to prevent common security mistakes.
> 5. 💡 Implement a vulnerability management program to track and remediate vulnerabilities in a timely manner.

<div style="page-break-before: always;"></div>

## 7. Conclusion

> [!NOTE]
> This security assessment identified several critical and high-severity vulnerabilities in the testphp.vulnweb.com web application. These vulnerabilities could allow attackers to gain unauthorized access to sensitive data, execute arbitrary code, or compromise user accounts.
>
> It is recommended to implement the provided recommendations to improve the security posture of the web application and protect it against common web attacks.

<div style="page-break-before: always;"></div>

## Appendix A: Tools Used

- 🔧 **Nmap** - Network scanning tool used for port scanning and service discovery
- 🔧 **Gobuster** - Directory and file enumeration tool
- 🔧 **SQLmap** - Automated SQL injection tool
- 🔧 **Wfuzz** - Web application fuzzer for parameter testing
- 🔧 **Nikto** - Web server scanner
- 🔧 **WhatWeb** - Web technology fingerprinting tool
- 🔧 **Dirb** - Web content scanner
- 🔧 **SSLyze** - SSL/TLS configuration analyzer

<div style="page-break-before: always;"></div>

## Appendix B: Vulnerability Scoring

### CVSs Scoring Methodology

The Common Vulnerability Scoring System (CVSS) is a framework for communicating the characteristics and severity of software vulnerabilities. CVSS consists of three metric groups: Base, Temporal, and Environmental. The Base metrics produce a score ranging from 0 to 10, which can then be modified by scoring the Temporal and Environmental metrics.

> [!INFO]
> **Base Score Metrics:**
> - **Exploitability Metrics:** Attack Vector (AV), Attack Complexity (AC), Privileges Required (PR), User Interaction (UI)
> - **Impact Metrics:** Confidentiality Impact (C), Integrity Impact (I), Availability Impact (A)
> - **Scope:** Whether a vulnerability in one component impacts resources in components beyond its security scope

### CVSs Scores for Findings

| Finding | **CVSS Score** | Vector String |
|:--------|:----------:|:--------------|
| **WAP-001: SQL Injection** | **9.8** | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H |
| **WAP-002: Cross-Site Scripting (XSS)** | **9.6** | CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:H |
| **WAP-003: Local File Inclusion (LFI)** | **9.8** | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H |
| **WAP-004: Cross-Site Request Forgery (CSRF)** | **8.8** | CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H |
| **WAP-005: Information Disclosure** | **7.5** | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N |
| **WAP-006: Insecure Direct Object References** | **8.2** | CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:L/A:N |
| **WAP-007: Weak Session Management** | **8.8** | CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H |
| **WAP-008: Missing Security Headers** | **6.5** | CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/I:L/A:L |
| **WAP-009: Directory Listing Enabled** | **5.3** | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N |
| **WAP-010: Insecure Cookie Attributes** | **3.7** | CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:L/I:N/A:N |
| **WAP-011: Server Information Disclosure** | **3.7** | CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:L/I:N/A:N |
| **WAP-012: Outdated Software** | **3.7** | CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:L/I:N/A:N |

---

**Document Classification: CONFIDENTIAL**  
© 2025 Security Research Team. All rights reserved.