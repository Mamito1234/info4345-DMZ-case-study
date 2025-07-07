# INFO 4345 Web Application Security Case Study

**Group Name:** DMZ

## Group Members & Roles

| Name                                 | Matric Number | Role & Task Description                                                                 |
|--------------------------------------|---------------|------------------------------------------------------------------------------------------|
| KEHMESS EL MOCTAR                    | 2113559       | Analyzed `https://hrsystem.iium.edu.my/apar-user/`, handled CSP, clickjacking, and CORS misconfig.                    |
| Muhammad Dinnie Haiqal bin M. Salman| 2124291       | Scanned `https://hrsystem.iium.edu.my/apar-admin/login/`, handled clickjacking & CORS issues.                         |
| AHMAD ZAED HAKIMI BIN ROSLI ALLANI  | 2217059       | Scanned `http://iattach.iium.edu.my/`, focused on missing headers, CSP, and info leak vulnerabilities.      |

---

## Table of Contents

1. Executive Summary  
2. Group Member Details  
3. Assigned Tasks Summary  
4. Description of Target Web Apps  
5. Identified Vulnerabilities  
6. Evaluation of Vulnerabilities  
7. Preventive Measures  
8. Recommendations & Code-Level Implementation  
9. Appendix  

---

## 1. Executive Summary

Across three assigned endpoints, our group identified multiple medium-risk vulnerabilities involving missing security headers, cross-origin misconfiguration, and information leakage. No critical or high-risk threats were discovered.

| Metric                  | Value |
|-------------------------|-------|
| Total Issues Identified | 52    |
| Critical Issues         | 0     |
| High-Risk Issues        | 0     |
| Medium-Risk Issues      | 9     |
| Low/Informational Issues| 43    |
| Remediation Status      | In Progress |

---

## 2. Group Member Details

Each member performed active scanning using OWASP ZAP and contributed to the evaluation and mitigation strategy collaboratively.

---

## 3. Assigned Tasks Summary

- **Moctar (2113559)**: Scanned `apar-user`. Handled missing CSP, absent `X-Frame-Options`, and exposed CORS.
- **Dinnie (2124291)**: Scanned `apar-admin`. Focused on clickjacking & CORS tests, scripted basic validation.
- **Zaed (2217059)**: Scanned `iattach`. Identified missing headers, insecure cookies, and server header leakage.

---

## 4. Description of Assigned Web Applications

- **apar-user**: Staff portal for HR operations with personal data entry forms.
- **apar-admin**: Admin login for role management and data operations.
- **iattach**: Document upload portal for staff and students.

---

## 5. Identified Vulnerabilities (Sample Highlights)

- Missing `Content-Security-Policy` header *(Moctar, Zaed)*
- CORS misconfiguration *(Moctar, Dinnie)*
- Missing `X-Frame-Options` header *(Moctar, Dinnie)*
- Server disclosure via headers *(Zaed)*
- Unsecured cookies lacking `Secure`, `HttpOnly` flags *(Zaed)*

---

## 6. Evaluation of Vulnerabilities

All medium-risk issues affect the front-end and session-level security:

- Potential for Cross-Site Scripting (XSS)  
- Susceptibility to Clickjacking Attacks  
- Risks from Open Cross-Origin Requests  
- Information Exposure via HTTP headers  

---

## 7. Preventive Measures (General)

- Add the following HTTP headers:
  - `Content-Security-Policy`
  - `X-Frame-Options`
  - `X-Content-Type-Options`
  - `Referrer-Policy`
  - `Strict-Transport-Security`
- Enforce strict CORS policy: Avoid `*`, define allowed origins explicitly
- Secure cookies using:
  - `Secure`
  - `HttpOnly`
  - `SameSite` attributes
- Sanitize headers: Remove or generalize `Server`, `X-Powered-By`

---

## 8. Recommendations & Code-Level Implementation

To fix these issues effectively, they must be addressed at the **code level**, where the application sends HTTP responses. Here’s how to apply them regardless of framework:

### General Implementation Steps

#### A. Apply Security Headers Globally

Find the part of your backend system that handles outgoing HTTP responses (e.g., middleware, filters, interceptors, controller base classes). Then insert the following headers:

```http
Content-Security-Policy: default-src 'self';
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: no-referrer
Strict-Transport-Security: max-age=31536000; includeSubDomains
````

In code, this generally looks like:

```pseudo
response.setHeader("Content-Security-Policy", "default-src 'self'");
response.setHeader("X-Frame-Options", "DENY");
response.setHeader("X-Content-Type-Options", "nosniff");
response.setHeader("Referrer-Policy", "no-referrer");
response.setHeader("Strict-Transport-Security", "max-age=31536000; includeSubDomains");
```

Ensure this is **executed for every response**, ideally through a central mechanism.

#### B. CORS Configuration

Do **not** use wildcard origins (`Access-Control-Allow-Origin: *`). Instead, specify trusted domains:

```http
Access-Control-Allow-Origin: https://trusted.domain.com
Access-Control-Allow-Methods: GET, POST
Access-Control-Allow-Headers: Content-Type, Authorization
```

Set these headers **only** for necessary routes and ensure OPTIONS preflight handling is secure.

#### C. Secure Cookie Implementation

When setting cookies, ensure your application uses:

```pseudo
Set-Cookie: sessionid=abc123; Secure; HttpOnly; SameSite=Strict
```

You can implement this during cookie creation, either server-side or through secure framework configuration.

#### D. Remove Header Disclosures

Turn off or override default server headers that expose technologies:

```http
Server: hidden
X-Powered-By: removed
```

---

## 9. Appendix

* **Tool Used**: OWASP ZAP 2.16.1 (Active Scanning)

### Individual Reports:

* **Moctar Report** [](file:///Users/elmoctarkehmess/Downloads/zap_report%20(1).html)
* **Dinnie Report**
* **Zaed Report**

### References:

* [https://owasp.org/www-project-top-ten/](https://owasp.org/www-project-top-ten/)
* [https://owasp.org/www-community/attacks/CORS\_OriginHeaderScrutiny](https://owasp.org/www-community/attacks/CORS_OriginHeaderScrutiny)
* [https://owasp.org/www-community/attacks/Clickjacking](https://owasp.org/www-community/attacks/Clickjacking)

---

**Prepared by Group DMZ**
**INFO 4345 – Web Application Security**
**International Islamic University Malaysia**
Date: 2025-05-29

```
