# Finding 4: Excessive Data Exposure

**OWASP Category:** API3:2023 - Broken Object Property Level Authorization (Excessive Data Exposure)
**Severity:** Medium

## Description
Multiple endpoints return more data than the UI displays or than is 
necessary for the client's function, including PII not visible in 
the application interface.

## Evidence

1. **GET /identity/api/v2/vehicle/{id}/location** returns full name 
   and email alongside location data (not shown in dashboard UI).

2. **GET /workshop/api/shop/orders/{id}** returns user email and 
   phone number, plus internal payment transaction_id — none of 
   which are needed just to show order status.

3. **GET /community/api/v2/community/posts/recent** returns every 
   post author's email address, even though the UI only shows 
   nicknames.

## Impact
Combined with the BOLA findings above, this amplifies the damage: 
an attacker doesn't just access unauthorized objects, but each 
object leaks more PII (emails, phone numbers, vehicle IDs) than 
the feature requires.

## Recommendation
Apply response filtering (DTOs/serializers) so each endpoint returns 
only the fields the corresponding UI feature actually needs. Email 
and phone number should never be included in location or community 
post responses.
