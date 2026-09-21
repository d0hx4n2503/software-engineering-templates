# Security Policy

## 1. Supported Versions

List which versions are currently receiving security patches. Update this table on every major/minor release (see `01-code-management/versioning-policy.md`).

| Version | Supported |
|---|---|
| {{LATEST_MAJOR}}.x | ✅ |
| {{PREVIOUS_MAJOR}}.x | ✅ (Critical/High fixes only) |
| < {{PREVIOUS_MAJOR}}.0 | ❌ |

## 2. Reporting a Vulnerability

**Do not open a public GitHub issue for a security vulnerability.** Disclosing it publicly before a fix is available may allow it to be exploited.

Instead, report through one of the following channels:

1. **GitHub Security Advisory** (recommended): go to the `Security` tab → `Report a vulnerability` on this repo.
2. **Private email**: send to `{{SECURITY_EMAIL}}` with the subject `[SECURITY] <project name> - <short description>`.

Your report should include:
- A description of the vulnerability and its potential impact (e.g. RCE, data leak, auth bypass...).
- Steps to reproduce (proof-of-concept if available).
- Affected version(s)/commit(s).
- Your contact information (so the team can follow up for clarification if needed).

## 3. Handling Process & Response SLA

| Step | Target time |
|---|---|
| Acknowledge receipt of report | Within 48 hours |
| Initial severity assessment (CVSS) | Within 5 business days |
| Patch released for a **Critical** vulnerability | Within 7 days |
| Patch released for a **High** vulnerability | Within 14 days |
| Patch released for a **Medium/Low** vulnerability | Included in the regular release cycle |

Reporters will receive periodic progress updates until the vulnerability is resolved.

## 4. Severity Classification

Based on [CVSS v3.1](https://www.first.org/cvss/calculator/3.1):

| Level | CVSS Score | Example |
|---|---|---|
| Critical | 9.0 – 10.0 | RCE, leaked private key/secret, full auth bypass |
| High | 7.0 – 8.9 | Privilege escalation, SQL injection exposing sensitive data |
| Medium | 4.0 – 6.9 | Conditional XSS, non-sensitive information disclosure |
| Low | 0.1 – 3.9 | Minor configuration issue with no clear real-world impact |

## 5. Coordinated Disclosure

We follow a **coordinated disclosure** model:
- The vulnerability is kept confidential until an official fix is released.
- After the fix ships, details may be published (GitHub Security Advisory + CVE if eligible), crediting the reporter (unless they prefer to remain anonymous).
- Please allow the team a minimum of **90 days** before disclosing the vulnerability publicly yourself, unless otherwise agreed.

## 6. Scope

This policy applies to the source code in this repository and to services directly operated by the `{{TEAM_NAME}}` team. Vulnerabilities in third-party dependencies should be reported directly to the library's maintainers (you may still open a regular issue here for internal tracking — no need to treat it as confidential).

## 7. Acknowledgements

We credit individuals who report valid vulnerabilities at `{{SECURITY_ACKNOWLEDGEMENTS_LINK}}` (if the reporter agrees to be publicly credited).