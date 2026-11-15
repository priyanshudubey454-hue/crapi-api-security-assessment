# Finding 5: Security Misconfiguration (CORS Wildcard)

**OWASP Category:** API8:2023 - Security Misconfiguration
**Severity:** Medium

## Description
All tested API responses include the header:
Access-Control-Allow-Origin: *

This allows any website (any origin) to make cross-origin requests 
to the crAPI backend and read the responses via JavaScript, when 
combined with a valid token (e.g., stolen via XSS or phishing).

## Evidence
Observed consistently across endpoints, e.g.:
- GET /identity/api/v2/vehicle/{id}/location
- GET /workshop/api/shop/orders/{id}
- POST /identity/api/auth/login

## Additional Observation: Error Handling
Error responses (e.g., invalid/expired token) return clean, generic 
messages without stack traces or internal implementation details:
  CRAPIResponse(message=InvalidToken, status=401)
This is good practice and not a finding — noted for completeness.

## Impact
Combined with XSS or any token leakage vector, wildcard CORS allows 
a malicious site to exfiltrate data cross-origin on behalf of a 
logged-in victim, bypassing same-origin policy protections.

## Recommendation
Restrict Access-Control-Allow-Origin to an explicit allowlist of 
trusted frontend domains instead of using a wildcard, especially 
for endpoints that return authenticated/sensitive data.
