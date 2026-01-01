# Payment Retry Flow Documentation

## Overview
This document explains the complete flow for payment initialization when a previous payment attempt has failed.

## Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. INITIAL ORDER PLACEMENT                                     │
│    POST /public/customer/orders                                 │
│    - Order created with payment_status = 'pending'              │
│    - Payment record created with status = 'pending'             │
│    - Paystack payment initialized                               │
│    - authorization_url returned to customer                      │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ 2. PAYMENT ATTEMPT                                             │
│    Customer redirected to Paystack checkout                     │
│    - Customer completes payment OR                              │
│    - Customer cancels OR                                      │
│    - Payment fails (insufficient funds, card declined, etc.)  │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ 3. PAYMENT OUTCOME                                             │
│                                                                 │
│    SUCCESS:                                                     │
│    - Paystack webhook: charge.success                           │
│    - Payment status → 'completed'                              │
│    - Order payment_status → 'paid'                             │
│    - Other pending payments → 'failed'                          │
│                                                                 │
│    FAILURE:                                                     │
│    - Paystack webhook: charge.failed                            │
│    - Payment status → 'failed'                                 │
│    - Order payment_status remains 'pending'                    │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼ (If payment failed)
┌─────────────────────────────────────────────────────────────────┐
│ 4. CUSTOMER RETRY PAYMENT                                      │
│    POST /public/customer/payment/initialize                    │
│    Body: {                                                      │
│      "order_id": 123,                                           │
│      "payment_method": "paystack"                               │
│    }                                                            │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ 5. VALIDATION & CHECKS                                          │
│    ✓ Order exists                                               │
│    ✓ Order belongs to authenticated customer                    │
│    ✓ Order payment_status ≠ 'paid' (reject if already paid)    │
│    ✓ Existing pending/failed payments detected                 │
│      (but retry is allowed - creates new payment record)      │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ 6. NEW PAYMENT INITIALIZATION                                  │
│    - Load order with customer relationship                     │
│    - Validate customer email exists                             │
│    - Initialize Paystack payment with:                           │
│      • Customer email                                          │
│      • Order total_amount                                       │
│      • Metadata (order_id, customer_id, branch_id)             │
│    - Create NEW payment record:                                 │
│      • status = 'pending'                                       │
│      • payment_reference = Paystack reference                  │
│      • transaction_id = Paystack reference                     │
│    - Old failed/pending payments remain in DB (audit trail)    │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ 7. RESPONSE TO CUSTOMER                                        │
│    {                                                            │
│      "success": true,                                           │
│      "message": "Payment initialized successfully",            │
│      "data": {                                                  │
│        "payment_id": 456,                                        │
│        "authorization_url": "https://checkout.paystack.com/...",│
│        "access_code": "xxxxx",                                  │
│        "reference": "T1234567890"                               │
│      }                                                           │
│    }                                                            │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ 8. CUSTOMER COMPLETES PAYMENT                                  │
│    Customer redirected to authorization_url                    │
│    - Completes payment on Paystack                              │
│    - Redirected back to application                             │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│ 9. PAYMENT VERIFICATION                                        │
│    Option A: Webhook (Automatic)                                │
│    POST /api/payment/paystack/webhook                           │
│    - Paystack sends charge.success event                        │
│    - handleChargeSuccess() method:                             │
│      • Payment status → 'completed'                            │
│      • Order payment_status → 'paid'                           │
│      • Other pending payments → 'failed'                       │
│                                                                 │
│    Option B: Manual Verification                                │
│    POST /public/customer/payment/verify                         │
│    Body: { "reference": "T1234567890" }                         │
│    - verifyPayment() method:                                    │
│      • Verifies with Paystack API                               │
│      • Payment status → 'completed'                            │
│      • Order payment_status → 'paid'                           │
│      • Other pending payments → 'failed'                       │
└─────────────────────────────────────────────────────────────────┘
```

## Key Points

### 1. Multiple Payment Records
- **Allowed**: Multiple payment records can exist for the same order
- **Purpose**: Maintains audit trail of all payment attempts
- **Status**: Only one payment can be 'completed' per order
- **Cleanup**: When a payment succeeds, other pending payments are marked as 'failed'

### 2. Retry Logic
- Customers can retry payment initialization even if previous attempts failed
- No limit on retry attempts (as long as order is not paid)
- Each retry creates a new payment record with a new Paystack reference

### 3. Order Status
- **Order payment_status**:
  - `'pending'` - No successful payment yet
  - `'paid'` - At least one payment is completed
- **Order status** (different from payment_status):
  - `'pending'` - Order placed, awaiting confirmation
  - `'confirmed'` - Order confirmed by restaurant
  - `'preparing'` - Order being prepared
  - etc.

### 4. Payment Statuses
- `'pending'` - Payment initialized, awaiting completion
- `'completed'` - Payment successful
- `'failed'` - Payment failed or cancelled
- `'refunded'` - Payment was refunded

### 5. Database State After Retry

**Before Retry:**
```
Order: id=123, payment_status='pending'
Payment: id=1, order_id=123, status='failed', reference='T111'
```

**After Retry (New Payment Created):**
```
Order: id=123, payment_status='pending'
Payment: id=1, order_id=123, status='failed', reference='T111'  (old)
Payment: id=2, order_id=123, status='pending', reference='T222' (new)
```

**After Successful Payment:**
```
Order: id=123, payment_status='paid'
Payment: id=1, order_id=123, status='failed', reference='T111'  (old)
Payment: id=2, order_id=123, status='completed', reference='T222' (successful)
```

## API Endpoints

### Initialize Payment (Retry)
```http
POST /public/customer/payment/initialize
Authorization: Bearer {token}
Content-Type: application/json

{
  "order_id": 123,
  "payment_method": "paystack"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Payment initialized successfully",
  "data": {
    "payment_id": 456,
    "authorization_url": "https://checkout.paystack.com/xxxxx",
    "access_code": "xxxxx",
    "reference": "T1234567890"
  }
}
```

### Verify Payment
```http
POST /public/customer/payment/verify
Authorization: Bearer {token}
Content-Type: application/json

{
  "reference": "T1234567890"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Payment verified successfully",
  "data": {
    "payment_id": 456,
    "order_id": 123,
    "amount": 3850.00,
    "payment_method": "paystack",
    "status": "completed",
    "reference": "T1234567890",
    "paid_at": "2024-12-15T10:40:00.000000Z",
    "order": {
      "id": 123,
      "order_number": "ORD-20241215-ABC123",
      "payment_status": "paid",
      "status": "pending"
    }
  }
}
```

## Error Scenarios

### 1. Order Already Paid
```json
{
  "success": false,
  "message": "Order is already paid"
}
```
**Status Code:** 400

### 2. Unauthorized Access
```json
{
  "success": false,
  "message": "Unauthorized access to this order"
}
```
**Status Code:** 403

### 3. Missing Customer Email
```json
{
  "success": false,
  "message": "Customer email is required for payment"
}
```
**Status Code:** 400

### 4. Paystack Initialization Failed
```json
{
  "success": false,
  "message": "Failed to initialize payment",
  "error": "Paystack error message"
}
```
**Status Code:** 400

### 5. Server Error
```json
{
  "success": false,
  "message": "Payment initialization failed",
  "error": "Exception message"
}
```
**Status Code:** 500

## Best Practices

1. **Always check order payment_status** before allowing retry
2. **Use webhook for automatic updates** (more reliable than manual verification)
3. **Keep payment history** for audit and customer support
4. **Handle webhook failures** gracefully (implement retry mechanism)
5. **Log all payment attempts** for debugging and support

## Testing Scenarios

1. ✅ Retry payment after initial failure
2. ✅ Retry payment after customer cancellation
3. ✅ Prevent retry if order already paid
4. ✅ Multiple retries create multiple payment records
5. ✅ Successful payment marks other pending payments as failed
6. ✅ Webhook updates order status correctly
7. ✅ Manual verification updates order status correctly

