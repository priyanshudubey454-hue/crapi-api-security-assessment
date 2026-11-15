# crapi-api-security-assessment
# crAPI API Security Assessment — Black-Box Pentest Report

Black-box penetration test of OWASP crAPI (Completely Ridiculous API), 
focused on the OWASP API Security Top 10 (2023).

## Summary
5 confirmed findings — 1 Critical, 1 High, 1 Medium-High, 2 Medium.

| # | Finding | Severity | OWASP Category |
|---|---|---|---|
| F1 | BOLA — Vehicle Location Disclosure | High | API1:2023 |
| F2 | BOLA — Order Details Disclosure (PII + Payment) | Critical | API1:2023 |
| F3 | Missing Rate Limiting on Login | Medium-High | API4:2023 |
| F4 | Excessive Data Exposure | Medium | API3:2023 |
| F5 | CORS Misconfiguration | Medium | API8:2023 |

## Tools Used
Burp Suite Community Edition, Postman, MailHog, Docker

## Full Report
See [`report/crAPI_Security_Assessment_Report.pdf`](./report/) for complete 
findings with reproduction steps, evidence, and remediation guidance.

## Disclaimer
Testing performed against a self-hosted, intentionally vulnerable instance 
of OWASP crAPI for educational purposes only.
