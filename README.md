<div align="center">

# 🔒 Internal Portal Access Control Audit

![Type](https://img.shields.io/badge/Type-Web%20App%20Pentest-red?style=for-the-badge)
![OWASP](https://img.shields.io/badge/OWASP-A01%3A2021%20Broken%20Access%20Control-orange?style=for-the-badge)
![Tool](https://img.shields.io/badge/Tool-Burp%20Suite-blueviolet?style=for-the-badge)
![Auth](https://img.shields.io/badge/Authorization-AltSchool%20Africa-blue?style=for-the-badge)

> A corporate-style penetration test on a simulated internal employee portal — demonstrating Broken Access Control exploitation where any authenticated user could access another user's private data by modifying a single request parameter.

</div>

---

## 📌 Project Overview

This project documents a penetration test conducted on a simulated internal employee portal, evaluating authentication and authorization controls. The focus was on testing whether users could access data belonging to other employees.

**Key Outcome:** A critical Broken Access Control vulnerability was identified — any logged-in user could retrieve another user's private data, including API keys, by modifying the `id` parameter in an HTTP request.

| Field | Details |
|---|---|
| **Vulnerability** | Broken Access Control — IDOR |
| **OWASP Category** | A01:2021 – Broken Access Control |
| **Lab Environment** | PortSwigger Web Security Academy |
| **Tools Used** | Burp Suite, Browser |
| **Authorization** | AltSchool Africa Internal Training |

---

## 🏗️ Lab Environment

| Component | Role |
|---|---|
| **Simulated Employee Portal** | Target application |
| **PortSwigger Web Security Academy** | Lab environment |
| **Burp Suite** | HTTP interception and request manipulation |
| **Browser** | Manual navigation and testing |

---

## 🔎 Scope & Methodology

**Scope:**
- Employee account details feature only
- Authentication and authorization control testing

**Methodology:**
- Manual black-box testing guided by the **OWASP Web Security Testing Guide**
- Logged in as a normal user and intercepted HTTP requests with Burp Suite
- Tested request parameters for access control bypass
- No real employee data accessed — lab accounts used throughout

---

## 🔬 Vulnerability Identified

**Type:** Insecure Direct Object Reference (IDOR) — User ID controlled by request parameter with data leakage in redirect

**Description:** The portal uses a user-supplied `id` parameter to fetch account information but performs **no server-side ownership validation**. Any authenticated user can modify the parameter to access another user's private data.

### Normal Authenticated Request
```http
GET /my-account?id=wiener HTTP/2
```

### Proof-of-Concept Exploit
```http
GET /my-account?id=carlos HTTP/2
```

### Result
- Server returned an **HTTP 302 redirect**
- Response body contained the **API key of the targeted user**
- No ownership check performed — zero authorization validation

> 📸 Evidence: Burp Suite screenshot showing API key leakage — see full report

---

## ⚠️ Impact Assessment

| Impact | Detail |
|---|---|
| **Data Exposure** | Any authenticated user can read another user's private data |
| **Credential Leakage** | API keys and sensitive account info exposed |
| **No Privilege Required** | Standard user account sufficient to exploit |
| **Undetectable** | No anomalous behavior — uses normal application requests |

---

## 🛡️ Remediation

**Root Cause:** The server trusts the client-supplied `id` parameter without verifying it belongs to the authenticated session.

### Vulnerable Pattern
```http
GET /my-account?id=carlos HTTP/2
// Server fetches carlos's data without checking if the requester IS carlos
```

### Recommended Fix
- **Enforce server-side ownership checks** — verify the `id` in the request matches the authenticated session user
- **Never use user-supplied IDs** to fetch sensitive objects without authorization validation
- Implement **object-level authorization** on every sensitive endpoint
- Use **indirect references** (e.g. session-bound tokens) instead of predictable user IDs in URLs

---

## 📘 Key Lessons

- Broken Access Control is the **#1 web application vulnerability** (OWASP 2021)
- Authentication ≠ Authorization — logging in does not mean access is controlled
- Server-side ownership checks must be enforced on **every** sensitive request
- HTTP interception with Burp Suite is highly effective for uncovering IDOR flaws

---

## 📄 Report

| File | Description |
|---|---|
| [`audit-report.pdf`](audit-report.pdf) | Full penetration test report with evidence and findings |

> **Note:** PDF files will download rather than preview directly on GitHub.

---

## 🧠 Skills Demonstrated

- Web application penetration testing
- Broken Access Control / IDOR assessment (OWASP A01)
- HTTP request/response analysis with Burp Suite
- Proof-of-concept exploit development
- Corporate-style security reporting

---

## 📚 References

- [OWASP – Broken Access Control (A01:2021)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger Web Security Academy – Access Control Labs](https://portswigger.net/web-security/access-control)

---

## ⚖️ Ethical & Transparency Statement

> All testing was performed in a **simulated lab environment** (PortSwigger Web Security Academy). No real employee or organization data was accessed or compromised.
>
> AI assistance was used **only for report polishing and structure** — all findings, exploitation, and analysis were performed independently.
>
> **Do not replicate these techniques against any application you do not own or have explicit written permission to test.** Unauthorized access is illegal under the CFAA and equivalent legislation worldwide.