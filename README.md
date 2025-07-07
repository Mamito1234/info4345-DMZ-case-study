````markdown
# INFO 4345 Web Application Security Case Study

## Group Name: DMZ

### Group Members & Roles

| Name                                   | Matric Number | Role & Task Description                                                                                      |
|----------------------------------------|----------------|---------------------------------------------------------------------------------------------------------------|
| KEHMESS EL MOCTAR                      | 2113559        | Analyzed [apar-user](https://hrsystem.iium.edu.my/apar-user/), handled CSP, clickjacking, and CORS misconfig. |
| Muhammad Dinnie Haiqal bin M. Salman  | 2124291        | Scanned [apar-admin/login](https://hrsystem.iium.edu.my/apar-admin/login/), handled clickjacking & CORS issues.|
| AHMAD ZAED HAKIMI BIN ROSLI ALLANI     | 2217059        | Scanned [iattach](http://iattach.iium.edu.my/), focused on missing headers, CSP, and info leak vulnerabilities.|

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

### 1. Executive Summary

Across three assigned endpoints, our group identified multiple **medium-risk vulnerabilities** involving missing security headers, cross-origin misconfiguration, and information leakage. No critical or high-risk threats were discovered.

| Metric               | Value |
|----------------------|-------|
| Total Issues Identified | 52    |
| Critical Issues         | 0     |
| High-Risk Issues        | 0     |
| Medium-Risk Issues      | 9     |
| Low/Informational Issues| 43    |
| Remediation Status      | In Progress |

---

### 2. Group Member Details

Each member performed active scanning using OWASP ZAP and contributed to the evaluation and mitigation strategy collaboratively.

---

### 3. Assigned Tasks Summary

- **Moctar (2113559):** Scanned apar-user. Handled missing CSP, absent X-Frame-Options, and exposed CORS.
- **Dinnie (2124291):** Scanned apar-admin. Focused on clickjacking & CORS tests, scripted basic validation.
- **Zaed (2217059):** Scanned iattach. Identified missing headers, insecure cookies, and server header leakage.

---

### 4. Description of Assigned Web Applications

- **apar-user:** Staff portal for HR operations with personal data entry forms.
- **apar-admin:** Admin login for role management and data operations.
- **iattach:** Document upload portal for staff and students.

---

### 5. Identified Vulnerabilities (Sample Highlights)

- Missing `Content-Security-Policy` header (Moctar, Zaed)  
- Cross-Origin Resource Sharing (CORS) misconfiguration (Moctar, Dinnie)  
- Missing `X-Frame-Options` header (Moctar, Dinnie)  
- Server disclosure via headers (Zaed)  
- Unsecured cookies lacking `Secure`, `HttpOnly` flags (Zaed)

---

### 6. Evaluation of Vulnerabilities

All medium-risk issues affect the front-end and session-level security:

- Potential for **Cross-Site Scripting (XSS)**
- Susceptibility to **Clickjacking Attacks**
- Risks from **Open Cross-Origin Requests**
- **Information Exposure** via HTTP headers

---

### 7. Preventive Measures (General)

- Add the following HTTP headers:
  - `Content-Security-Policy`
  - `X-Frame-Options`
  - `X-Content-Type-Options`
  - `Referrer-Policy`
- Strict CORS setup: Avoid `*`, define specific origins.
- Secure cookies: Set `HttpOnly`, `Secure`, and `SameSite` attributes.
- Hide server details: Remove or sanitize `Server` and `X-Powered-By`.

---

### 8. Recommendations & Code-Level Implementation

This section outlines not only the *what* but the *how*, especially in **Laravel** and **PHP** frameworks:

#### A. Laravel Implementation (Backend-Level)

**Step 1: Create a Middleware to Inject Headers**

```bash
php artisan make:middleware SecurityHeaders
````

**Step 2: Edit `app/Http/Middleware/SecurityHeaders.php`**

```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class SecurityHeaders
{
    public function handle(Request $request, Closure $next)
    {
        $response = $next($request);

        $response->headers->set('Content-Security-Policy', "default-src 'self'");
        $response->headers->set('X-Frame-Options', 'DENY');
        $response->headers->set('X-Content-Type-Options', 'nosniff');
        $response->headers->set('Referrer-Policy', 'no-referrer');
        $response->headers->set('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');

        return $response;
    }
}
```

**Step 3: Register Middleware in `app/Http/Kernel.php`**

```php
protected $middleware = [
    \App\Http\Middleware\SecurityHeaders::class,
];
```

#### B. Native PHP / Sparky Setup (If Laravel is not used)

In files like `header.php` or early in `index.php`:

```php
header("Content-Security-Policy: default-src 'self'");
header("X-Frame-Options: DENY");
header("X-Content-Type-Options: nosniff");
header("Referrer-Policy: no-referrer");
header("Strict-Transport-Security: max-age=31536000; includeSubDomains");
```

Make sure this code is executed before any HTML output begins.

---

### 9. Appendix

* **Tool Used:** OWASP ZAP 2.16.1 (Active Scanning)
* **Individual Reports:**

  * [Moctar Report](./Moctar.rtfd)
  * [Dinnie Report](./Dinnie.rtfd)
  * [Zaed Report](./Zaed.rtfd)
* **Full ZAP HTML Report:** [zap\_report.html](./zap_report.html)
* **References:**

  * [https://owasp.org/www-project-top-ten/](https://owasp.org/www-project-top-ten/)
  * [https://owasp.org/www-community/attacks/CORS\_OriginHeaderScrutiny](https://owasp.org/www-community/attacks/CORS_OriginHeaderScrutiny)
  * [https://owasp.org/www-community/attacks/Clickjacking](https://owasp.org/www-community/attacks/Clickjacking)

---

Prepared by **Group DMZ**
*INFO 4345 – Web Application Security*
International Islamic University Malaysia
📅 Date: 2025-05-29

```
