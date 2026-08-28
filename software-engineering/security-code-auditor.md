---
name: security-code-auditor
description: Conduct comprehensive application security reviews, threat modeling, and vulnerability remediation across web applications, APIs, smart contracts, and cloud environments. Identifies OWASP Top 10 vulnerabilities (SQLi, XSS, CSRF, SSRF, IDOR/BOLA, Broken Auth, Security Misconfigurations), secrets leakage, cryptographic weaknesses, and dependency risks. Use this skill when reviewing code for security flaws, performing penetration test style code audits, hardening authentication/authorization, or mitigating reported security vulnerabilities.
---

# Application Security & Code Auditor

An expert cybersecurity skill for proactively uncovering security vulnerabilities, performing rigorous threat modeling, auditing source code against the OWASP Top 10 / ASVS, and delivering prioritized, remediation-ready security fixes.

---

## 1. OWASP Top 10 Threat Audit Matrix

| Vulnerability Category | Common Root Cause | Audit & Remediation Checklist |
| :--- | :--- | :--- |
| **A01: Broken Access Control / IDOR** | Relying on client-supplied IDs without verifying tenant/user ownership on the server. | Enforce row-level ownership checks: `WHERE id = $1 AND tenant_id = $2`. Avoid sequential IDs; use UUIDv4/ULIDs. |
| **A02: Cryptographic Failures** | Hardcoded keys, weak hashing (MD5/SHA1), transmitting sensitive data over unencrypted channels. | Use Argon2id/bcrypt for passwords (work factor >= 12). Enforce HTTPS/HSTS, encrypt sensitive columns at rest with AES-256-GCM. |
| **A03: Injection (SQL, Command, LDAP)** | Concatenating untrusted user input directly into query strings or shell executions. | Use parameterized prepared statements or ORMs everywhere. Never use `eval()`, `child_process.exec()` with raw user strings. |
| **A04: Insecure Design & Logic Flaws** | Missing rate limits on login/OTP endpoints, race conditions in balance transfers, lack of idempotency. | Implement rate limiting (sliding window), atomic DB transactions with row locking (`SELECT ... FOR UPDATE`), idempotency keys. |
| **A05: Security Misconfiguration** | Debug mode active in prod, stack traces exposed in API errors, default admin credentials, overly permissive CORS. | Disable debug flags, standardize error responses, lock down CORS origins (`Access-Control-Allow-Origin: specific-domain.com`). |
| **A06: Vulnerable Dependencies** | Outdated packages containing known CVEs in npm/pip/maven. | Run automated SCA (`npm audit`, `pip-audit`, Trivy, Dependabot/Renovate). Lock dependency versions. |
| **A07: Identification & Auth Failures** | Weak password policies, insecure session storage in `localStorage`, missing MFA, session fixation. | Use HTTP-only, Secure, SameSite=Lax/Strict cookies. Invalidate sessions on password reset and logout. |
| **A08: Software & Data Integrity** | Deserializing untrusted JSON/YAML/pickle objects, CDN scripts without integrity hashes. | Never deserialize untrusted objects. Use Subresource Integrity (SRI) hashes on external scripts. |
| **A09: Logging & Monitoring Failures** | Failing to log security events (failed logins, privilege escalation), or conversely logging sensitive PII/passwords. | Log authentication anomalies with context; mask and sanitize PII, passwords, authorization tokens before writing logs. |
| **A10: SSRF (Server-Side Request Forgery)** | Server fetching remote URLs provided by users without validating IP ranges. | Validate and allowlist schemas (`https:` only) and resolve hostnames, blocking loopback (`127.0.0.1`), link-local (`169.254.169.254`), and private RFC 1918 subnets. |

---

## 2. Secure Code Review Workflow

### Phase 1: Attack Surface Mapping
- Identify all untrusted entry points: REST/GraphQL routes, WebSocket events, webhooks, file uploads, URL parameters, message queues.
- Map data flows from source (entry point) $\rightarrow$ sink (database query, filesystem write, external API call, HTML render).

### Phase 2: Vulnerability Triage & Proof of Concept
- For each entry point, test edge cases:
  - What happens with oversized input?
  - What happens when a user attempts to access another user's resource (`/api/users/123/invoices` $\rightarrow$ `/api/users/456/invoices`)?
  - Can SQL/Script payloads bypass client validation?

### Phase 3: Defense in Depth Remediation
- Provide explicit, drop-in code fixes that solve the root cause, not just patch symptoms.
- Implement security headers on all HTTP responses:
  ```http
  Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-...'; object-src 'none'; frame-ancestors 'none';
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
  Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
  ```

---

## 3. Security Audit Report Format

When delivering security findings, structure them with standard severity ratings:

```markdown
### [VULN-01] [CRITICAL/HIGH/MEDIUM/LOW] <Vulnerability Title>
- **Location**: `src/routes/billing.ts:L42`
- **Vulnerability Type**: Insecure Direct Object Reference (CWE-639)
- **Impact**: Any authenticated user can view and download invoices of other organizations by manipulating the `invoiceId` parameter.
- **Root Cause**: Database query filters by `id` alone without verifying `tenant_id = req.user.tenantId`.
- **Proof of Concept (PoC)**:
  `GET /api/invoices/98472` returned 200 OK with sensitive billing data belonging to another customer.
- **Remediation**:
  [Provide exact code diff implementing the fix]
```
