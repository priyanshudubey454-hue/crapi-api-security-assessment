# Finding 1: BOLA - Vehicle Location Endpoint

**Endpoint:** GET /identity/api/v2/vehicle/{vehicle_id}/location
**OWASP Category:** API1:2023 - Broken Object Level Authorization
**Severity:** High

## Description
User B's JWT token was used to access User A's vehicle location data by 
supplying User A's vehicle UUID in the URL path. The server does not 
verify that the requesting user owns the vehicle before returning data.

## Steps to Reproduce
1. Login as User A, obtain vehicle UUID from GET /identity/api/v2/vehicle/vehicles
   (UUID: 8e93d2e6-0b0f-4cb8-b0b4-2ce5ec1506ff)
2. Login as User B, capture JWT token
3. Send: GET /identity/api/v2/vehicle/8e93d2e6-0b0f-4cb8-b0b4-2ce5ec1506ff/location
   Authorization: Bearer <User B's token>
4. Response (HTTP 200) returns User A's vehicle location, full name, and email

## Impact
Any authenticated user can enumerate vehicle UUIDs and access other users' 
real-time vehicle location, name, and email — enabling tracking/stalking 
and PII harvesting.

## Recommendation
Server must verify vehicle.owner_id == authenticated_user.id before 
returning vehicle data, in addition to JWT validation.

## Evidence
See screenshot: 01-bola-exploit.png
