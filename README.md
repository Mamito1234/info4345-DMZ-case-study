# INFO 4345 Web Application Security Case Study

## Group Name: DMZ

### Group Members & Roles

| Name                                             | Matric Number | Role & Task Description                                                                                  |
|--------------------------------------------------|----------------|-----------------------------------------------------------------------------------------------------------|
| KEHMESS EL MOCTAR                                | 2113559        | Analyzed `https://hrsystem.iium.edu.my/apar-user/`, handled CSP, clickjacking, and CORS misconfiguration. |
| Muhammad Dinnie Haiqal bin Muhammad Salman       | 2124291        | Scanned `https://hrsystem.iium.edu.my/apar-admin/login/`, handled clickjacking and CORS misconfig issues. |
| AHMAD ZAED HAKIMI BIN ROSLI ALLANI               | 2217059        | Scanned `http://iattach.iium.edu.my/`, focused on missing headers, CSP, and informational leak vulnerabilities. |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)  
2. [Group Member Details](#2-group-member-details)  
3. [Assigned Tasks Summary](#3-assigned-tasks-summary)  
4. [Description of Target Web Apps](#4-description-of-assigned-web-applications)  
5. [Identified Vulnerabilities](#5-identified-vulnerabilities-sample-highlights)  
6. [Evaluation of Vulnerabilities](#6-evaluation-of-vulnerabilities)  
7. [Preventive Measures](#7-preventive-measures-general)  
8. [Recommendations & Next Steps](#8-recommendations--next-steps)  
9. [Appendix](#9-appendix)  

---

## 1. Executive Summary

Across three assigned endpoints, our group identified multiple **medium-risk vulnerabilities** primarily involving security headers, cross-origin misconfiguration, and information disclosure. No critical or high-risk threats were discovered.

| Metric                  | Value        |
|-------------------------|--------------|
| Total Issues Identified | 52           |
| Critical Issues         | 0            |
| High-Risk Issues        | 0            |
| Medium-Risk Issues      | 9            |
| Low/Informational Issues| 43           |
| Remediation Status      | In Progress  |

---

## 2. Group Member Details

Each member conducted an active scan using OWASP ZAP and contributed equally to identifying, evaluating, and recommending preventive controls.

---

## 3. Assigned Tasks Summary

- **KEHMESS EL MOCTAR (2113559)**: Scanned `apar-user`, handled CSP header absence, missing X-Frame-Options, and open CORS endpoint.  
- **Muhammad Dinnie Haiqal bin Muhammad Salman (2124291)**: Scanned `apar-admin/login`, validated anti-clickjacking and CORS misconfig, authored testing scripts.  
- **AHMAD ZAED HAKIMI BIN ROSLI ALLANI (2217059)**: Scanned `iattach.iium.edu.my`, covered missing headers, insecure cookies, and server info leaks.  

---

## 4. Description of Assigned Web Applications

- **apar-user**: Staff-facing HR interface – contains sensitive data forms.  
- **apar-admin**: Admin login portal – includes sensitive role-based controls.  
- **iattach**: Document submission system – handles student and staff file uploads.  

> These applications are integral to IIUM’s internal processes and should be well-secured.

---

## 5. Identified Vulnerabilities (Sample Highlights)

- **Missing CSP Header** (Moctar, Zaed)  
- **Cross-Domain Misconfiguration** (Moctar, Dinnie)  
- **Missing X-Frame-Options** (Moctar, Dinnie)  
- **Leaked Server Headers** (Zaed)  
- **Lack of Secure Cookie Flag** (Zaed)  

> Full details provided in each member’s `.rtfd` report.

---

## 6. Evaluation of Vulnerabilities

All medium-risk issues impact the application’s surface layer and may allow:
- Cross-site scripting (XSS)  
- UI redressing (clickjacking)  
- Unauthorized access via cross-origin calls  
- Information leakage via headers  

These do not immediately compromise backend systems but pose a threat to frontend and session-level controls.

---

## 7. Preventive Measures (General)

- **Add Missing HTTP Headers**:  
  - Content-Security-Policy  
  - X-Frame-Options  
  - X-Content-Type-Options  
  - Referrer-Policy  
- **Configure CORS Strictly**: Avoid wildcards, whitelist trusted domains only  
- **Secure Cookies**: Add HttpOnly, Secure, and SameSite attributes  
- **Suppress Tech Info**: Remove or generalize `X-Powered-By`, `Server` headers  

---

## 8. Recommendations & Next Steps

- Apply all header-based security fixes  
- Re-scan after remediation is completed  
- Conduct periodic automated scans  
- Include secure coding audits in development lifecycle  

---

## 9. Appendix

- **Tool Used**: OWASP ZAP 2.16.1 (Active Scanning)  
- **Individual Findings**:  
  - `Moctar.rtfd`  
  - `Dinnie.rtfd`  
  - `Zaed.rtfd`  
- **Attached File**: `zap_report.html`  
- **References**:  
  - https://owasp.org/www-project-top-ten/  
  - https://owasp.org/www-community/attacks/CORS_OriginHeaderScrutiny  
  - https://owasp.org/www-community/attacks/Clickjacking  

---

**Prepared by Group DMZ**  
INFO 4345 – Web Application Security  
International Islamic University Malaysia  
Date: 2025-05-29
