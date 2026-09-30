# Startup Security Baseline Audit Framework

A reusable security health-check framework for small and resource-constrained startups, demonstrated through an authorized DVWA laboratory assessment.

## Overview

Small startups may not have the resources for a full penetration test or managed security service. This project provides a lightweight and repeatable approach for identifying common security weaknesses, collecting evidence, assessing risk, mapping findings to security controls, and producing actionable audit documentation.

The framework was demonstrated against a deliberately vulnerable Damn Vulnerable Web Application (DVWA) instance running locally in Docker.

> **Important:** DVWA is intentionally vulnerable and was used only as a controlled training target. The findings in this project demonstrate the audit methodology, evidence collection, risk assessment, and reporting process. They do not represent the security posture of a real company.

### Results at a Glance

- **4 findings:** 1 Critical (demo), 1 High, 1 Medium, 1 Low
- **21 checklist items reviewed:** 7 Fail, 1 Pass, 2 Observation, 1 Not Determined, 10 N/A or Not Assessed
- Several checks could not be tested in the lab (TLS, MFA, password policy, RBAC, cloud storage) and were recorded as such instead of being forced into a Pass or Fail. See [Checks Not Applicable or Not Determined](#checks-not-applicable-or-not-determined).

## Project Objectives

- Build a reusable startup security baseline checklist
- Assess network and service exposure
- Review transport and web application security
- Identify applicable identity and access security checks
- Identify cloud and storage security checks where applicable
- Collect reproducible technical evidence
- Assess findings using a likelihood × impact risk model
- Map findings to CIS Controls v8 safeguards
- Produce practical remediation recommendations
- Package the methodology as a reusable security audit framework

## Assessment Scope

The framework covers five security areas:

1. Network Exposure
2. Transport Security
3. Web Application Layer
4. Identity & Access
5. Cloud / Storage

### Demonstration Environment

| Item | Details |
|---|---|
| Target | Damn Vulnerable Web Application (DVWA) |
| Platform | Ubuntu 24.04.1 LTS on WSL2 |
| Containerization | Docker 29.1.3 |
| Database | MariaDB 10 |
| Target Address | `127.0.0.1:4280` |
| Assessment Date | 18 September 2026 |
| Authorization | Self-owned laboratory |
| Purpose | Demonstration of the reusable audit methodology |

The DVWA instance was used as a controlled and deliberately vulnerable laboratory target. The DVWA security level was set to **Impossible**, and its built-in vulnerabilities were not exploited. No production or third-party systems were assessed.

## Tools Used

### Network & Environment

- Nmap 7.94SVN
- Docker CLI
- Docker Compose
- `ss`
- `lsof`

### Web Application Analysis

- `curl`
- Burp Suite Community Edition v2026.8

### Conditional Tool

- `testssl.sh`: applicable when an HTTPS endpoint is available

Because the demonstration target did not expose HTTPS, TLS-specific testing was treated as not applicable rather than forcing a result.

## Methodology

The framework follows a five-phase workflow:

### Phase 0: Environment & Scope Setup

Define the authorized target, assessment scope, environment, tools, and limitations.

### Phase 1: Checklist Development

Create a reusable checklist covering:

- Network Exposure
- Transport Security
- Web Application Layer
- Identity & Access
- Cloud / Storage

The checklist is maintained as a clean template so it can be reused for other environments.

### Phase 2: Baseline Assessment

Perform applicable technical and manual checks, collect evidence, and document whether each check can be assessed.

### Phase 3: Risk Assessment & CIS Mapping

Evaluate confirmed findings using the likelihood × impact matrix below and map relevant findings to CIS Controls v8 safeguards.

#### Risk Rating Matrix

Risk = Likelihood × Impact

| Likelihood \ Impact | Low | Medium | High |
|---|---|---|---|
| **Low** | Low | Low | Medium |
| **Medium** | Low | Medium | High |
| **High** | Medium | High | Critical |

- **Likelihood:** how easily the weakness could be found and used given the exposure observed.
- **Impact:** what an attacker gains if the weakness is successfully used, such as data exposure, account compromise, or service disruption.
- Ratings use a production-hypothetical context (as if the same setup were an internet-facing production application), while the technical testing itself was performed only against the local laboratory.

### Phase 4: Audit Report

Convert assessment results into a structured security audit report containing:

- Executive Summary
- Scope and Methodology
- Findings and Evidence
- Risk Ratings
- CIS Control Mapping
- Remediation Recommendations
- Limitations

### Phase 5: Portfolio Packaging

Organize the reusable checklist, assessment documentation, evidence, and reporting artifacts into a portable project structure.

## Assessment Results

The demonstration assessment produced four documented findings. The table shows how each rating follows from the matrix above.

| ID | Finding | Likelihood | Impact | Risk |
|---|---|---|---|---|
| F-01 | HTTP-only application and insecure session transport | Medium | High | **High** |
| F-02 | Missing security response headers | Medium | Medium | **Medium** |
| F-03 | Server/PHP version disclosure | Low | Low | **Low** |
| F-04 | Default credentials accepted | High | High | **Critical (demo)** |

### F-01: HTTP-only Application and Insecure Session Transport

The demonstration application was accessible over HTTP and did not provide an HTTPS endpoint (443/tcp closed). The PHPSESSID cookie was set without the `Secure` attribute, and the `security` cookie had neither `Secure` nor `SameSite`.

**Risk:** High

**CIS mapping:** Control 3 / Safeguard 3.10, Encrypt Sensitive Data in Transit.

**Recommended direction:** Deploy HTTPS with a valid certificate and TLS 1.2 or higher, redirect HTTP to HTTPS, enable HSTS once HTTPS works, and set the `Secure` attribute on cookies.

### F-02: Missing Security Response Headers

Responses did not include Content-Security-Policy, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, or Permissions-Policy.

**Risk:** Medium

**CIS mapping:** Control 16 / Safeguard 16.7, Use Standard Hardening Configuration Templates for Application Infrastructure.

**Recommended direction:** Define and implement a security-header baseline through the web server configuration (for example, `mod_headers` on Apache). Test CSP in report-only mode first.

### F-03: Server/PHP Version Disclosure

Normal responses disclosed Apache and PHP version information, and the 404 page also exposed the Apache version.

**Risk:** Low

**CIS mapping:** Safeguard 4.1 (Establish and Maintain a Secure Configuration Process) and Control 16.

**Recommended direction:** Set `ServerTokens Prod` and `ServerSignature Off` in Apache and `expose_php = Off` in PHP. Keep software patched regardless.

### F-04: Default Credentials Accepted

The demonstration DVWA instance accepted its documented default credentials.

**Risk:** Critical (demo)

**CIS mapping:** Safeguard 4.7 (Manage Default Accounts) and Safeguard 5.2 (Use Unique Passwords).

**Recommended direction:** Change or remove default accounts before deployment, require unique strong passwords, and add MFA for administrative accounts.

> **Demonstration finding:** DVWA is intentionally vulnerable and is designed for security training. This result demonstrates how the framework records an authentication weakness; it should not be interpreted as evidence of a real organization's security posture.

## Checks Not Applicable or Not Determined

Not every security check can be meaningfully performed in every environment. The framework deliberately records these cases rather than forcing a Pass or Fail result.

Examples from the demonstration environment:

- TLS versions and cipher configuration: N/A, no HTTPS endpoint was exposed
- TLS certificate validation: N/A, no HTTPS endpoint was exposed
- HSTS: N/A as a separate check because the application was HTTP-only; the underlying transport issue is covered by F-01
- MFA configuration: N/A / Not Assessed, the demonstration application did not provide a suitable privileged MFA configuration
- Password policy: N/A / Not Assessed, no suitable password-policy configuration was exposed
- Role-based access control: N/A / Not Assessed, the required administrative configuration was not available
- Admin-panel rate limiting: N/A / Not Assessed, no suitable test was available in the DVWA scope
- Cloud/storage configuration: N/A, the laboratory did not use a cloud-storage service
- SSH bind address (`0.0.0.0:22`): Not Determined, the bind address alone does not establish Internet reachability
- Internet exposure: not established, because the target was bound to the local environment

**N/A, Not Assessed, and Not Determined are valid assessment outcomes and should not be converted into failures without supporting evidence.**

## Important Lab Limitations

- Testing was performed only against a self-owned local DVWA laboratory.
- The target was `127.0.0.1:4280`; Internet exposure was not established.
- The presence of `0.0.0.0:22` for SSH was treated as a listening/bind configuration and not automatically interpreted as Internet exposure.
- DVWA is intentionally vulnerable and is not representative of a production application.
- The assessment did not perform destructive exploitation.
- Identity and access controls that were not exposed by the laboratory application were not assessed.
- Cloud/storage controls were not applicable to the laboratory.
- TLS-specific testing was not performed because HTTPS was not exposed.
- Risk ratings are contextual to the assessment methodology and should be reassessed for an actual target environment.
- Results are a point-in-time snapshot from 18 September 2026.
- CIS mappings are control references and do not constitute a formal CIS compliance assessment.

## How to Reuse This Framework

1. **Define scope and authorization.**
   Identify the target, assessment scope, date, and authorization before testing. Only test systems you own or have written permission to test.

2. **Copy the checklist.**
   Duplicate `checklist-template/Startup-Security-Checklist.md` and use the copy for the new assessment. Keep the original as a clean template.

3. **Perform the applicable checks.**
   Use appropriate tools and methods for the target environment and store supporting evidence under `evidence/`.

4. **Complete the assessment.**
   Record one of the following for each check, based on the evidence collected:
   - **Pass:** tested, no issue identified
   - **Fail:** tested, weakness identified
   - **Observation:** noteworthy evidence, but not a demonstrated vulnerability
   - **Not Determined:** evidence is insufficient to establish the condition
   - **N/A:** technology or check not applicable to the environment (use "N/A / Not Assessed" where the check applies in general but the environment could not support testing it)

5. **Assess and prioritize findings.**
   Use the Risk Rating Matrix to evaluate confirmed findings and map them to relevant CIS Controls v8 safeguards.

6. **Prepare the audit report.**
   Document the findings, evidence, risk ratings, remediation recommendations, and assessment limitations.

## Reusable Checklist

The checklist is intentionally maintained as a reusable template rather than being filled with the results of the DVWA demonstration.

Assessment-specific results belong in the assessment documentation and report, while the original checklist remains clean for future use.

[Security Checklist](checklist-template/Startup-Security-Checklist.md)

## Project Deliverables

- [Security Checklist](checklist-template/Startup-Security-Checklist.md)
- [Phase 3 Risk Assessment](Phase_3_Risk_Assessment_CIS_Mapping_Final.pdf)
- [Phase 4 Sample Audit Report](Phase_4_Sample_Security_Audit_Report.pdf)
- [Evidence](evidence/)

## Repository Structure

```text
startup-security-baseline-audit/
├── checklist-template/
│   └── Startup-Security-Checklist.md
├── evidence/
│   ├── README.md
│   ├── environment/
│   ├── network/
│   └── web/
├── Phase_3_Risk_Assessment_CIS_Mapping_Final.pdf
├── Phase_4_Sample_Security_Audit_Report.pdf
└── README.md
```
