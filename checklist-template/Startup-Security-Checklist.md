# Startup Security Baseline Checklist

## Project Information

**Project:** Startup Security Baseline Audit Framework

**Target:** DVWA (Damn Vulnerable Web Application)

**Authorization:** Personal authorized security testing lab

**Purpose:** Reusable security health-check framework for small startups.

---

## Audit Checklist

| Check Item | CIS Control Mapping | Risk Rating | Status | Evidence |
|---|---|---|---|---|
| **Network Exposure** | | | | |
| Any ports open beyond expected (80/443 only for a web-facing host)? | TBD | TBD | TBD | TBD |
| Is SSH exposed to 0.0.0.0/0 rather than restricted IP ranges? | TBD | TBD | TBD | TBD |
| Are database ports (3306/5432/27017/etc.) publicly reachable? | TBD | TBD | TBD | TBD |
| Do service banners leak outdated software versions? | TBD | TBD | TBD | TBD |
| **Transport Security** | | | | |
| Is TLS 1.0 or 1.1 still enabled? | TBD | TBD | TBD | TBD |
| Are weak/deprecated cipher suites offered? | TBD | TBD | TBD | TBD |
| Is HSTS header present and correctly configured? | TBD | TBD | TBD | TBD |
| Is the certificate valid, non-expired, and not self-signed in production? | TBD | TBD | TBD | TBD |
| **Web Application Layer** | | | | |
| Are CSP, X-Frame-Options, X-Content-Type-Options headers present? | TBD | TBD | TBD | TBD |
| Is an admin panel exposed without rate limiting or IP restriction? | TBD | TBD | TBD | TBD |
| Do error pages leak stack traces or server details? | TBD | TBD | TBD | TBD |
| Are default credentials still active on any interface? | TBD | TBD | TBD | TBD |
| **Identity & Access** | | | | |
| Is MFA enforced on admin/privileged accounts? | TBD | TBD | TBD | TBD |
| Is there an enforced password complexity policy? | TBD | TBD | TBD | TBD |
| Are account roles/permissions scoped to least privilege? | TBD | TBD | TBD | TBD |
| **Cloud / Storage Misconfiguration** | | | | |
| Are storage buckets (S3-style) publicly accessible? | TBD | TBD | TBD | TBD |
| Are bucket/object policies overly permissive? | TBD | TBD | TBD | TBD |

---

## Status Definitions

- **Pass:** The security check was tested and no issue was identified.
- **Fail:** The security check identified a security weakness.
- **N/A:** The check is not applicable to the audit scope.
- **TBD:** The check has not yet been assessed.

## Risk Rating

Risk ratings will be assigned after the assessment based on the
observed finding, exposure, potential impact, and likelihood.

## Evidence

Evidence should reference the supporting screenshot, command output,
HTTP response, or other audit artifact collected during the assessment.

---

## Audit Scope

The assessment will focus on:

1 Network Exposure
2. Transport Security
3. Web Application Layer
4. Identity & Access
5. Cloud / Storage Misconfiguration

Testing will be performed only against the authorized local DVWA
laboratory environment.

---

## Phase 1 Status

**Checklist Development:** Complete after all checklist items have
been reviewed and finalized.

**Next Phase:** Baseline Assessment
