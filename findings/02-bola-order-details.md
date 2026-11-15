# Finding 2: BOLA - Order Details Endpoint (Critical)

**Endpoint:** GET /workshop/api/shop/orders/{order_id}
**OWASP Category:** API1:2023 - Broken Object Level Authorization
**Severity:** Critical

## Description
Order IDs are sequential integers. Any authenticated user can access 
any other user's complete order details — including PII and payment 
data — by simply incrementing the order_id in the URL. No ownership 
check is performed.

## Steps to Reproduce
1. Login as User A, place an order (POST /workshop/api/shop/orders), 
   note order id = 7
2. Login as User B, capture JWT token
3. Send: GET /workshop/api/shop/orders/7
   Authorization: Bearer <User B's token>
4. Response (HTTP 200) returns User A's full order:
   - email: usera@test.com
   - phone number: 9876543210
   - product, quantity, status
   - transaction_id (payment system identifier)
   - payment amount and date

## Impact
- Full PII disclosure (email, phone) for any user
- Financial/transaction data disclosure
- Sequential IDs (1, 2, 3...7) allow trivial enumeration of ALL 
  orders in the system by any authenticated user — mass data breach 
  via simple automated script (e.g., Burp Intruder with numbers 1-N)

## Recommendation
1. Verify order.user_id == authenticated_user.id before returning data
2. Consider using non-sequential, non-guessable identifiers (UUIDs) 
   for order IDs to reduce enumeration risk as defense-in-depth
3. Do not return full payment/transaction details in this endpoint 
   unless strictly necessary
