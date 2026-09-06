# awesome-api-security-checklist
# Production-Ready API & Security Pre-Flight Checklist

A battle-tested, 50-point pre-flight security inspection matrix to detect critical launch blockers, access control flaws, and configuration mistakes before hitting production traffic.

[![OWASP API Aligned](https://img.shields.io/badge/Security-OWASP%20API%20Aligned-0ea5e9.svg)](#)
[![Checks Total](https://img.shields.io/badge/Audit%20Checks-50%20Inspection%20Points-10b981.svg)](#)
[![Fulfillment](https://img.shields.io/badge/Kit%20Status-Instant%20Download-blue.svg)](#)

> 🚀 **Need the complete interactive framework?**  
> Get the [Full NullGate Security Audit Kit](https://gumroad.com/l/YOUR-GUMROAD-SLUG): Includes the **Excel Scoring Dashboard**, **1-Click Notion/Airtable Database**, all **50 Technical Markdown Cards**, and **SEV-1 Leaked Key Incident Runbooks**.

---

## 📋 The 5 Hardening Pillars

- [x] **1. Authentication & Sessions** *(Included below in full)*
- [ ] **2. Authorization & Access Control** *(BOLA/IDOR, Mass Assignment, Tenant RLS)*
- [ ] **3. Data Protection & Input/Output** *(SQLi, PII Log Scrubbing, Error Masking, SSRF)*
- [ ] **4. Rate Limiting & Resilience** *(Edge Gateway Caps, ReDoS, Circuit Breakers, DB Pools)*
- [ ] **5. Infrastructure & Network** *(Strict CORS, HSTS/CSP, Rootless Containers, VPC Isolation)*

---

## ⚡ Pillar 1 Preview: Authentication & Session Management

### 1. Cryptographic Algorithm Enforcement on JWTs (P0 Blocker)
* **Risk:** Unvalidated headers allow `alg: none` or RSA/HMAC algorithm confusion attacks to forge admin tokens.
* **Verification Command:**
```bash
FORGED_TOKEN="eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJzdWIiOiIxMjM0NTYiLCJyb2xlIjoiYWRtaW4ifQ."
curl -i -X GET [https://api.yourdomain.com/v1/user/me](https://api.yourdomain.com/v1/user/me) -H "Authorization: Bearer $FORGED_TOKEN"
```
* **Remediation:** Enforce an explicit algorithm allowlist during token verification (e.g., `algorithms=['RS256']`).

### 2. Short-Lived Access Tokens & Refresh Rotation (P0 Blocker)
* **Standard:** Access token TTL must not exceed 15 minutes (`exp - iat <= 900s`).
* **Rotation Policy:** Refresh tokens must be single-use. If a previously consumed refresh token is presented again, invalidate the entire token family immediately.

### 3. Password Hashing Standard (P0 Blocker)
* **Standard:** Reject legacy single-iteration hashes (MD5, SHA-1, standard SHA-256).
* **Remediation:** Mandate Argon2id (`memory_cost=64MB`, `time_cost=3`) or bcrypt with a minimum work factor of 12.

### 4. Centralized Session Revocation (P0 Blocker)
* **Standard:** Password updates, MFA resets, or suspected account compromises must terminate all active sessions instantly across devices.
* **Remediation:** Track a global user `token_version` in Redis or PostgreSQL and reject any JWT bearing an older version claim.

### 5. Credential Stuffing & Rate Limiting (P1 High)
* **Standard:** Capped at 5 failed authentication attempts per minute per IP and account identifier.
* **Verification:** Send 10 rapid failed login requests; confirm HTTP `429 Too Many Requests` is returned with a valid `Retry-After` header.

---

## 📦 What's Included in the Full NullGate Audit Kit

If you want the entire pre-flight verification system ready to deploy in your workspace:

| Asset | Format | Purpose |
| :--- | :--- | :--- |
| **Executive Audit Dashboard** | `.xlsx` / Sheets | Real-time calculation of readiness score, P0 blockers, and pillar completion. |
| **Notion & Airtable Database** | `.csv` | 1-click import into engineering task backlogs with priority scoring and owners. |
| **50 Inspection Cards** | `.md` | Exact `curl` recipes, verification steps, and 2-line code remedies for all 5 pillars. |
| **SEV-1 Leaked Secret Runbook** | `.md` | 30-minute containment and key rotation playbook without causing system downtime. |
| **Executive Sign-Off Template** | `.md` | 1-page formal launch approval template for stakeholders, enterprise clients, or board review. |
| **Vulnerability Disclosure Pack** | `.txt` / `.md` | RFC 9116 compliant `security.txt` and `SECURITY.md` policies for public repos. |

👉 **[Download the Full Kit on Gumroad ($49 Startup / $99 Agency License)](https://gumroad.com/l/YOUR-GUMROAD-SLUG)**

---

## 📄 License
The preview documentation in this repository is licensed under the [MIT License](LICENSE).  
The complete NullGate Audit Kit, templates, and spreadsheets are distributed under personal and commercial licenses via Gumroad.
