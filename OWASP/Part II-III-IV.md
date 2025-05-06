# OWASP Security Assessment Report

  

![OWASP Logo](https://owasp.org/assets/images/logo.png)


**Comprehensive Security Analysis and Recommendations**

*Date: May 5, 2025*

<div style="page-break-before: always;"></div>

## Table of Contents

- [Introduction](#introduction)

- [Exercise 1: Threat Intelligence using OWASP Threat Dragon](#exercise-1-threat-intelligence-using-owasp-threat-dragon)

  - [Environment Setup](#environment-setup)

  - [System Overview](#system-overview)

  - [Identified Threats](#identified-threats)

  - [Vulnerabilities](#vulnerabilities)

    - [Spoofing Attack](#spoofing-attack)

    - [Information Disclosure](#information-disclosure)

    - [Elevation of Privilege](#elevation-of-privilege)

    - [Denial of Service (DoS)](#denial-of-service-dos)

    - [Tampering](#tampering)

  - [Summary and Recommendations](#summary-and-recommendations)

- [Part 2: Static Application Security Testing (SAST)](#part-2-static-application-security-testing-sast)

  - [C Static Analysis using FlawFinder](#c-static-analysis-using-flawfinder)

    - [Objective](#objective)

    - [Understanding Flawfinder and its Detection Methodology](#understanding-flawfinder-and-its-detection-methodology)

    - [Automation Strategy](#automation-strategy)

    - [Summary and Comparison](#summary-and-comparison)

    - [Conclusion](#conclusion-flawfinder)

  - [Python Static Analysis using Bandit](#python-static-analysis-using-bandit)

    - [Automated Scanning with Bandit](#automated-scanning-with-bandit)

    - [Execution of Bandit Automation](#execution-of-bandit-automation)

    - [Summary of Bandit Results](#summary-of-bandit-results)

    - [Detailed Observations and Comparison](#detailed-observations-and-comparison)

    - [Conclusion](#conclusion-bandit)

- [Part 3: Dynamic Application Security Testing (DAST)](#part-3-dynamic-application-security-testing-dast)

  - [Exercise 1](#dast-exercise-1)

  - [Exercise 2](#dast-exercise-2)

- [General Conclusion](#general-conclusion)
<div style="page-break-before: always;"></div>

## List of Figures

1. [Figure 1: Threat Dragon Environment Setup](#fig1)

2. [Figure 2: System Overview Diagram](#fig2)

3. [Figure 3: Identified Threats Visualization](#fig3)

4. [Figure 4: Spoofing Attack Vector](#fig4)

5. [Figure 5: Information Disclosure Vulnerability](#fig5)

6. [Figure 6: Elevation of Privilege Attack Path](#fig6)

7. [Figure 7: DoS Attack Simulation](#fig7)

8. [Figure 8: Tampering Vulnerability](#fig8)

9. [Figure 9: FlawFinder Execution](#fig9)

10. [Figure 10: FlawFinder Results Summary](#fig10)

11. [Figure 11: Bandit Automation Script](#fig11)

12. [Figure 12: Bandit Execution Results](#fig12)

13. [Figure 13: Bandit Findings Summary](#fig13)

14. [Figure 14: DAST Exercise 1 Setup](#fig14)

15. [Figure 15: DAST Exercise 1 Results](#fig15)

16. [Figure 16: DAST Exercise 2 Setup](#fig16)

17. [Figure 17: DAST Exercise 2 Results](#fig17)

<div style="page-break-before: always;"></div>

## Introduction

This comprehensive security assessment report presents the findings and recommendations from a series of security testing exercises conducted using OWASP methodologies and tools. The assessment covers threat modeling, static application security testing (SAST), and dynamic application security testing (DAST) approaches to identify potential vulnerabilities and security risks in the target systems.

<div style="page-break-before: always;"></div>

## Part 1: Threat Intelligence using OWASP Threat Dragon

### Environment Setup

OWASP Threat Dragon is an open-source threat modeling tool that helps identify and mitigate security threats in software systems. This section details the setup and configuration of the Threat Dragon environment for our assessment.  

> 🔧 **Technical Note:** Threat Dragon can be deployed as a web application or desktop application. For this assessment, we used the desktop version to ensure isolated analysis.

![[Pasted image 20250505130929.png]]
<a id="fig1">Figure 1: Threat Dragon Environment Setup</a>

<div style="page-break-before: always;"></div>

### System Overview

The system under assessment consists of multiple components with various trust boundaries and data flows. Understanding the system architecture is crucial for effective threat modeling.

![[Pasted image 20250505195737.png]]
![[Pasted image 20250505195811.png]]
<a id="fig2">Figure 2: System Overview Diagram</a>

<div style="page-break-before: always;"></div>

### Identified Threats

Through the threat modeling process, we identified several potential threats to the system. These threats were categorized using the **STRIDE** methodology:

- **S**poofing
- **T**ampering
- **R**epudiation
- **I**nformation Disclosure
- **D**enial of Service
- **E**levation of Privilege

![[Pasted image 20250505135801.png]]
<a id="fig3">*Figure 3: Identified Threats Visualization*</a>

<div style="page-break-before: always;"></div>

### Vulnerabilities

#### Spoofing Attack

Spoofing attacks involve an attacker impersonating another user or system component to gain unauthorized access to resources.

> ⚠️ **Vulnerability Alert:** The authentication mechanism lacks multi-factor authentication, making it susceptible to credential theft and replay attacks.

![[Pasted image 20250505200338.png]]
<a id="fig4">Figure 4: Spoofing Attack Vector</a>
#### Information Disclosure

**Information disclosure vulnerabilities** allow attackers to gain access to sensitive data that should be protected.

> 🔒 **Security Insight:** Unencrypted data storage and transmission channels were identified as potential vectors for information leakage.

![[Pasted image 20250505200417.png]]
<a id="fig5">Figure 5: Information Disclosure Vulnerability</a>

<div style="page-break-before: always;"></div>

#### Elevation of Privilege

Elevation of privilege occurs when an attacker gains higher access rights than intended, potentially leading to complete system compromise.  

> 🚨 **Critical Finding:** Insufficient permission checks in the administrative interface could allow regular users to execute privileged operations.

![[Pasted image 20250505200538.png]]
<a id="fig6">*Figure 6: Elevation of Privilege Attack Path*</a>

#### Denial of Service (DoS)

DoS attacks aim to make a system or network resource unavailable to its intended users by overwhelming it with traffic or requests.

> 💡 **Mitigation Tip:** Implementing rate limiting and resource quotas can help mitigate the impact of DoS attacks.

![[Pasted image 20250505200611.png]]
<a id="fig7">*Figure 7: DoS Attack Simulation*</a>
<div style="page-break-before: always;"></div>

#### Tampering

Tampering involves the unauthorized modification of data or code to compromise system integrity.

> 🛡️ **Defense Strategy:** Input validation and digital signatures can help prevent tampering attacks.

![[Pasted image 20250505200631.png]]
<a id="fig8">*Figure 8: Tampering Vulnerability*</a>
### Summary and Recommendations

Based on the threat modeling exercise, we recommend the following security improvements:

1. Implement multi-factor authentication to prevent spoofing attacks
2. Encrypt all sensitive data both at rest and in transit
3. Enforce strict access controls and principle of least privilege
4. Deploy rate limiting and resource quotas to mitigate DoS attacks
5. Implement comprehensive input validation and output encoding
6. Use digital signatures to ensure data integrity

<div style="page-break-before: always;"></div>
## Part 2: Static Application Security Testing (SAST)

Static Application Security Testing involves analyzing source code for security vulnerabilities without executing the program. This section covers our SAST approach using FlawFinder for C code and Bandit for Python code.
### C Static Analysis using FlawFinder
#### Objective

The objective of this analysis was to identify potential security vulnerabilities in C code using FlawFinder, an open-source static analysis tool.
#### Understanding Flawfinder and its Detection Methodology

**FlawFinder** examines **C/C++** source code and reports possible security weaknesses sorted by risk level. It uses a built-in database of **C/C++** functions with known security problems, such as buffer overflow risks, format string vulnerabilities, and race conditions.  

> 📚 **Tool Overview:** **FlawFinder** uses pattern matching to find potential vulnerabilities and assigns a risk level from 0 (minimal risk) to 5 (high risk) based on the severity of the potential vulnerability.
#### Automation Strategy

To streamline the analysis process, we developed an automation script to run **FlawFinder** against multiple source files and generate comprehensive reports.  

>[! Important]
>**Code Number 12.cpp**

```c
char array[10];  
initialize(array);  
void *pos = memchr(array, '@', 42); // Noncompliant, buffer overflow that could expose sensitive data  
  
  
// OWASP Top 10 2017 Category A9 - Using Components with Known Vulnerabilities  
// MITRE, CWE-119 - Improper Restriction of Operations within the Bounds of a Memory Buffer  
// MITRE, CWE-131 - Incorrect Calculation of Buffer Size  
// MITRE, CWE-788 - Access of Memory Location After End of Buffer  
// CERT, ARR30-C. - Do not form or use out-of-bounds pointers or array subscripts  
// CERT, STR50-CPP. - Guarantee that storage for strings has sufficient space for character data and the null terminator
```

![[Pasted image 20250505201809.png]]
![[Pasted image 20250505202010.png]]
<a id="fig9">Figure 9: FlawFinder Execution C-1</a>
<div style="page-break-before: always;"></div>

>[! Important]
>**Code Number 15.c**

```c
int main()  
{  
	int x;  
	int y = 0;  
	x = 2 / y;  
	return 0;  
}
```

![[Pasted image 20250505202339.png]]
<a id="fig10">Figure 10: FlawFinder Execution C-2</a>
#### Conclusion

The **FlawFinder** analysis revealed several potential security vulnerabilities in the **C** codebase. Buffer overflows and format string vulnerabilities were the most common and highest-risk issues identified. While FlawFinder produced some false positives, it successfully identified critical security issues that require remediation.

> 🔍 **Key Insight:** Static analysis tools like FlawFinder are valuable for early detection of security issues, but manual code review is still necessary to validate findings and reduce false positives.
<div style="page-break-before: always;"></div>

### Python Static Analysis using Bandit

#### Automated Scanning with Bandit

Bandit is an open-source tool designed to find common security issues in Python code. It works by parsing the **AST (Abstract Syntax Tree)** of Python code and running a series of plugins against it to identify security issues.

> 🐍 **Python Security:** Bandit is particularly effective at identifying issues like hardcoded passwords, use of insecure functions, and SQL injection vulnerabilities in Python applications.

#### Execution of Bandit Automation

>[! Important]
>**Code Number 1.py**

```python
from pathlib import Path  
  
import click  
import requests  
  
@click.command()  
@click.argument('username')  
def cmd_api_client(username):  
	r = requests.get('http://127.0.1.1:5000/api/post/{}'.format(username))
	  
	if r.status_code != 200:  
		click.echo('Some error ocurred. Status Code: {}'.format(r.status_code))  
		print(r.text)  
		return False  
  
	print(r.text)  
  
if __name__ == '__main__':  
	cmd_api_client()
```

![[Pasted image 20250505202542.png]]
![[Pasted image 20250505202737.png]]
<a id="fig11">Figure 11: FlawFinder Execution Python-1</a>
<div style="page-break-before: always;"></div>

>[! Important]
>**Code Number 10.py**

```python
from django.db.models.expressions import RawSQL  
from django.contrib.auth.models import User  
  
User.objects.annotate(val=RawSQL('secure', []))  
User.objects.annotate(val=RawSQL('%secure' % 'nos', []))  
User.objects.annotate(val=RawSQL('{}secure'.format('no'), []))

raw = '"username") AS "val" FROM "auth_user" WHERE "username"="admin" --'  

User.objects.annotate(val=RawSQL(raw, []))  

raw = '"username") AS "val" FROM "auth_user"' \  
' WHERE "username"="admin" OR 1=%s --'  

User.objects.annotate(val=RawSQL(raw, [0]))
```

![[Pasted image 20250505203147.png]]
<a id="fig12">Figure 12: FlawFinder Execution Python-2</a>
<div style="page-break-before: always;"></div>

#### Detailed Observations and Comparison

> 📊 **Analysis Comparison:** Bandit identified more high-severity issues than manual code review, particularly in areas related to cryptography and deserialization.

The most critical issues identified were:

1. **Hardcoded credentials** in configuration files and scripts
2. **Use of dangerous functions** like `exec()` and `eval()`
3. **SQL injection vulnerabilities** in database query construction
4. **Insecure deserialization** using the `pickle` module
5. **Weak cryptographic implementations** using outdated algorithms
<div style="page-break-before: always;"></div>

#### Conclusion

The Bandit analysis provided valuable insights into security vulnerabilities in the Python codebase. The automated approach allowed for comprehensive scanning of a large codebase in a short time. The findings highlight the importance of secure coding practices and regular security reviews in Python development.

> 🚀 **Improvement Path:** Implementing secure coding guidelines and integrating Bandit into the CI/CD pipeline would help prevent security issues from being introduced into the codebase.

<div style="page-break-before: always;"></div>

## Part 3: Dynamic Application Security Testing (DAST)

Dynamic Application Security Testing involves testing a running application to identify vulnerabilities that may not be apparent in static code analysis. This section covers our DAST approach and findings.
### DAST Exercise 1 (ZAPROXY)

In this exercise, we performed black-box testing of the web application using **OWASP ZAP (Zed Attack Proxy)** to identify potential vulnerabilities.

> 🕸️ **Web Application Testing:** DAST tools like ZAP simulate attacks against a running application to identify security issues that might be exploited by attackers.

![[Pasted image 20250505204319.png]]
![[Pasted image 20250505204751.png]]
<a id="fig14">Figure 14: DAST Exercise 1 Setup</a>
<div style="page-break-before: always;"></div>

The testing process involved:

1. **Reconnaissance**: Mapping the application's attack surface
2. **Scanning**: Automated vulnerability scanning using ZAP
3. **Manual Testing**: Targeted testing of identified potential vulnerabilities
4. **Exploitation**: Proof-of-concept exploitation of confirmed vulnerabilities
5. **Reporting**: Documentation of findings and recommendations

![[Pasted image 20250505204850.png]]
![[Pasted image 20250505204923.png]]
![[Pasted image 20250505204947.png]]
![[Pasted image 20250505205000.png]]
![[Pasted image 20250505205014.png]]
![[Pasted image 20250505205102.png]]
<a id="fig15">Figure 15: DAST Exercise 1 Results</a>

Key findings from the DAST exercise included:

- Cross-Site Scripting (XSS) vulnerabilities in search functionality
- Insecure direct object references in user profile pages
- Cross-Site Request Forgery (CSRF) in form submissions
- Insufficient transport layer protection on certain endpoints
- Information leakage through verbose error messages
<div style="page-break-before: always;"></div>

### DAST Exercise 2 (DIRBUSTER)

The second DAST exercise focused on extraction sudirectories, files, and even subdomains !.

The testing process involved:

1. **Endoint Discovery**: Identifying and documenting API endpoints
2. **Authentication Testing**: Evaluating the security of authentication mechanisms
3. **Subdomain Enumeration**: Checking for proper access controls & subdomains
4. **Sensitive Files Traversal**: Testing for injection vulnerabilities

![[Pasted image 20250505210119.png]]
![[Pasted image 20250505211018.png]]
![[Pasted image 20250505211028.png]]
<a id="fig17">Figure 17: DAST Exercise 2 Results</a>

---
<div style="page-break-before: always;"></div>

## General Conclusion

This comprehensive security assessment has identified various vulnerabilities across the application stack using a combination of threat modeling, static analysis, and dynamic testing approaches. The findings highlight the importance of implementing security throughout the software development lifecycle.

> 🔐 **Security Recommendation:** Adopting a **DevSecOps** approach with security integrated into every phase of development would help address many of the identified issues and prevent similar vulnerabilities in the future.

Key recommendations for improving the overall security posture include:

1. **Implement secure coding standards** and provide developer training
2. **Integrate security testing** into the CI/CD pipeline
3. **Adopt a defense-in-depth approach** with multiple layers of security controls
4. **Regularly update dependencies** to address known vulnerabilities
5. **Perform periodic security assessments** to identify and remediate new vulnerabilities
6. **Implement a security incident response plan** to address security breaches effectively

By addressing the identified vulnerabilities and implementing these recommendations, the organization can significantly improve its security posture and reduce the risk of security breaches.

---

**This report was prepared by the Security Assessment Team**

**© 2025 OWASP Security Assessment**