# 🔒 Internal Portal Access Control Audit  
**Simulated Employee Portal Penetration Test (AltSchool Case)**

---

## 📌 Project Overview

This project documents a **corporate-style penetration test** conducted on a simulated internal employee portal. The focus was to evaluate authentication and authorization controls, specifically testing whether users could access data belonging to other employees.

Key objectives:

- Assess user account access controls  
- Identify potential Broken Access Control (OWASP Top 10-A01) vulnerabilities  
- Demonstrate proof-of-concept exploitation in a safe, academic environment  
- Provide recommendations for mitigation and secure application design  

All testing was performed ethically in a controlled lab environment using simulated accounts.

---

## 🏗 Lab Environment

| Component | Role |
|-----------|------|
| Simulated Employee Portal | Target application |
| PortSwigger Web Security Academy | Lab environment |
| Tools Used | Burp Suite (HTTP interception), Browser |

---

## 🔹 Scope & Methodology

**Scope:**  
- Employee account details feature only  
- Authentication and authorization testing  

**Methodology:**  
- Manual black-box testing guided by the OWASP Web Security Testing Guide  
- Logged in as a normal user and intercepted HTTP requests with Burp Suite  
- Tested request parameters for potential access control bypass  
- No real employee data was accessed; lab accounts were used  

---

## 🔹 Vulnerability Identified

**Category:** Broken Access Control (OWASP A01)  
**Type:** User ID controlled by request parameter with data leakage in redirect  

**Description:**  
The portal uses a user-supplied “id” parameter to fetch account information but does **not validate ownership**. Any logged-in user can modify the parameter to access other users’ data.

**Example Vulnerable Request:**
```http
GET /my-account?id=wiener HTTP/2
```

**Proof-of-Concept Exploit:**
```http
GET /my-account?id=carlos HTTP/2
```

**Result:**  
- HTTP 302 redirect returned  
- Sensitive information (e.g., API key) of another user was exposed  
- Demonstrated the absence of object-level authorization checks  
  
📸 Evidence: Burp Suite screenshot showing API key leakage  (see report)

---

## 🔹 Impact Assessment

- Any logged-in user can access another user’s private data  
- Significant risk of confidential information exposure  
- Highlights the need for robust **object-level authorization** and parameter validation  

---

## 🔹 Lessons Learned

- Broken Access Control is one of the most critical web application vulnerabilities  
- Always enforce server-side ownership checks on sensitive data  
- Validate request parameters and implement proper access control mechanisms  
- Manual testing combined with HTTP interception is effective for uncovering authorization flaws  

---

## 🔹 Ethical & Transparency Statement

- All testing was performed in a **simulated lab environment**  
- No real employee or organization data was accessed  
- AI assistance (ChatGPT) was used **only for report polishing and structure**, not for generating findings  

---

## 🔹 References

- [OWASP Foundation – Broken Access Control (A01)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)  
- [PortSwigger Web Security Academy – Access Control Labs](https://portswigger.net/web-security/access-control)  
- OpenAI. ChatGPT – used for report structuring  

---

## 🔹 Skills Demonstrated

- Web application penetration testing  
- Broken Access Control assessment (OWASP Top 10)  
- HTTP request/response analysis with Burp Suite  
- Proof-of-concept development  
- Academic and corporate-style security reporting  
