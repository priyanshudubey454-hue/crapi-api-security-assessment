# Finding 3: Missing Rate Limiting on Login Endpoint

**Endpoint:** POST /identity/api/auth/login
**OWASP Category:** API4:2023 - Unrestricted Resource Consumption
**Severity:** Medium-High

## Description
The login endpoint does not implement any rate limiting, account 
lockout, or CAPTCHA mechanism. 8 consecutive failed login attempts 
with different passwords were sent for the same account within 
under a minute, and every request received an identical 401 
"Invalid Credentials" response with no throttling, delay increase, 
or blocking.

## Steps to Reproduce
1. Send POST /identity/api/auth/login with valid email and incorrect 
   password repeatedly:
   {"email":"usera@test.com","password":"wrong1"}
   {"email":"usera@test.com","password":"wrong2"}
   ... (repeated 8 times)
2. Observe: all 8 requests return HTTP 401 with no change in behavior,
   timing, or an eventual block/429 response

## Impact
- Enables unthrottled brute-force / credential-stuffing attacks 
  against user accounts
- An attacker can attempt passwords indefinitely from a single IP 
  without detection or blocking
- Combined with weak/common passwords, this significantly increases 
  account takeover risk

## Recommendation
1. Implement rate limiting per account and per IP (e.g., max 5 
   attempts per 15 minutes)
2. Return HTTP 429 Too Many Requests once threshold is exceeded
3. Consider progressive delays (exponential backoff) after repeated 
   failures
4. Add CAPTCHA after N failed attempts
5. Alert/log on repeated failed login attempts for monitoring
