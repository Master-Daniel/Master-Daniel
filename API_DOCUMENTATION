# Customer-Facing API Documentation

This documentation describes the REST API endpoints for the customer-facing website. All endpoints are prefixed with `/api`.

## Base URL

```
http://restaurant-pos.backend.elitebot.com.ng/api
```

## Authentication

Most endpoints require authentication using Bearer tokens. Include the token in the Authorization header:

```
Authorization: Bearer {your_access_token}
```

## HTTP Methods

All endpoints explicitly document their HTTP method (GET, POST, PUT, DELETE, etc.). Using the wrong HTTP method will result in a `405 Method Not Allowed` error. For example:
- `/api/public/contact` only accepts `POST` requests
- `/api/public/settings` only accepts `GET` requests
- `/api/public/customer/me` only accepts `GET` requests

Always check the **Method:** line for each endpoint before making requests.

---

## Table of Contents

1. [Public Endpoints](#public-endpoints)
   - [Restaurant Settings](#restaurant-settings)
   - [Branches](#branches)
   - [Contact/Feedback](#contactfeedback)
   - [Coupons](#coupons)
2. [Authentication](#authentication)
   - [Register Customer](#register-customer)
   - [Login](#login)
   - [Logout](#logout)
   - [Forgot Password](#forgot-password)
   - [Reset Password](#reset-password)
3. [Menu & Items](#menu--items)
4. [Orders](#orders)
5. [Payments](#payments)
6. [Profile](#profile)
   - [Get Profile](#get-profile)
   - [Update Profile](#update-profile)
   - [Change Password](#change-password)
   - [Update Profile Picture](#update-profile-picture)
7. [Shipping Addresses](#shipping-addresses)
8. [Transaction History](#transaction-history)
9. [Loyalty Program](#loyalty-program)
10. [Error Handling](#error-handling)

---

## Public Endpoints

These endpoints do not require authentication and are available to all users.

### Restaurant Settings

Get public restaurant information including logo, name, contact details, and social media.

**Method:** `GET`

**Endpoint:** `/public/settings`

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "restaurant_name": "My Restaurant",
    "restaurant_email": "info@restaurant.com",
    "restaurant_phone": "+2348012345678",
    "customer_care_email": "support@restaurant.com",
    "customer_care_phone": "+2348012345679",
    "copyright_text": "© 2024 My Restaurant. All rights reserved.",
    "about_us_html": "<p>About us content...</p>",
    "social_media": {
      "facebook": "https://facebook.com/restaurant",
      "instagram": "https://instagram.com/restaurant",
      "twitter": "https://twitter.com/restaurant",
      "tiktok": "https://tiktok.com/@restaurant",
      "whatsapp": "+2348012345678",
      "youtube": "https://youtube.com/restaurant"
    },
    "delivery_fee_rules": [
      {
        "max_km": 5,
        "fee": 500.00
      },
      {
        "max_km": 10,
        "fee": 1000.00
      }
    ],
    "favicon_url": "http://example.com/storage/settings/favicon.png",
    "navbar_logo_url": "http://example.com/storage/settings/navbar-logo.png",
    "footer_logo_url": "http://example.com/storage/settings/footer-logo.png"
  }
}
```

---

### Branches

Get a list of all active restaurant branches.

**Method:** `GET`

**Endpoint:** `/public/branches`

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Main Branch",
      "address": "123 Main Street, Lagos",
      "phone": "+2348012345678",
      "email": "main@restaurant.com",
      "is_active": true,
      "latitude": 6.5244,
      "longitude": 3.3792,
      "opening_hours": "Mon-Sat: 9am-10pm"
    }
  ]
}
```

---

### Contact/Feedback

Submit customer feedback or contact form.

**Method:** `POST` (only POST is allowed, GET/PUT/DELETE will return 405 Method Not Allowed)

**Endpoint:** `/public/contact`

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+2348012345678",
  "subject": "Feedback",
  "message": "Great service! Keep it up."
}
```

**Note:** `customer_id` is optional. If the user is authenticated, it will be automatically included.

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Thank you for your feedback. We'll get back to you soon.",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+2348012345678",
    "subject": "Feedback",
    "message": "Great service! Keep it up.",
    "status": "pending",
    "created_at": "2024-12-15T10:30:00.000000Z"
  }
}
```

---

### Coupons

#### Get Active Coupons

Get a list of all active coupons available for use.

**Method:** `GET`

**Endpoint:** `/public/coupons`

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "code": "WELCOME10",
      "name": "Welcome Discount",
      "description": "10% off on your first order",
      "discount_type": "percentage",
      "discount_value": 10.00,
      "min_purchase": 1000.00,
      "max_discount": 500.00,
      "valid_from": "2024-12-01T00:00:00.000000Z",
      "valid_until": "2024-12-31T23:59:59.000000Z",
      "usage_limit": 100,
      "usage_limit_per_customer": 1,
      "times_used": 45,
      "is_active": true
    }
  ]
}
```

#### Validate Coupon

Validate a coupon code and get discount information.

**Method:** `POST`

**Endpoint:** `/public/coupons/validate`

**Request Body:**
```json
{
  "code": "WELCOME10",
  "order_total": 5000.00
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Coupon is valid",
  "data": {
    "coupon": {
      "id": 1,
      "code": "WELCOME10",
      "name": "Welcome Discount",
      "discount_type": "percentage",
      "discount_value": 10.00
    },
    "discount_amount": 500.00,
    "final_amount": 4500.00
  }
}
```

**Error Responses:**
- `400 Bad Request` - Coupon not found, expired, or invalid
- `422 Validation Error` - Validation failed

**Note:** When placing an order, include the `coupon_code` in the order request to apply the discount.

---

## Authentication

### Register Customer

Register a new customer account.

**Method:** `POST`

**Endpoint:** `/public/customer/register`

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+2348012345678",
  "password": "password123",
  "password_confirmation": "password123"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Registration successful",
  "data": {
    "user": {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com",
      "phone": "+2348012345678",
      "roles": [
        {
          "name": "Customer"
        }
      ]
    },
    "access_token": "1|xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "token_type": "Bearer"
  }
}
```

**Validation Errors (422):**
```json
{
  "success": false,
  "errors": {
    "email": ["The email has already been taken."],
    "password": ["The password must be at least 8 characters."]
  }
}
```

---

### Login

Authenticate a customer and receive an access token.

**Method:** `POST`

**Endpoint:** `/public/customer/login`

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com",
      "phone": "+2348012345678",
      "roles": [
        {
          "name": "Customer"
        }
      ]
    },
    "access_token": "1|xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "token_type": "Bearer"
  }
}
```

**Error Responses:**
- `401 Unauthorized` - Invalid credentials
- `403 Forbidden` - Account is inactive or not a customer account

---

### Logout

Revoke the current access token.

**Method:** `POST`

**Endpoint:** `/public/customer/logout` or `/customer/logout`

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

### Forgot Password

Request a password reset link via email.

**Method:** `POST`

**Endpoint:** `/public/customer/forgot-password`

**Request Body:**
```json
{
  "email": "john@example.com"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Password reset link has been sent to your email address. Please check your inbox."
}
```

**Error Responses:**
- `422 Validation Error` - Email is required or doesn't exist
- `403 Forbidden` - Email is not associated with a customer account
- `500 Server Error` - Failed to send email

---

### Reset Password

Reset password using the token from email.

**Method:** `POST`

**Endpoint:** `/public/customer/reset-password`

**Request Body:**
```json
{
  "email": "john@example.com",
  "token": "reset_token_from_email",
  "password": "newpassword123",
  "password_confirmation": "newpassword123"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Password reset successfully"
}
```

**Error Responses:**
- `400 Bad Request` - Invalid or expired token
- `422 Validation Error` - Validation failed

---

## Menu & Items

### Get Menu

Get all available menu items, optionally filtered by branch.

**Method:** `GET`

**Endpoint:** `/public/menu`

**Query Parameters:**
- `branch_id` (optional) - Filter items by branch ID
- `category_id` (optional) - Filter items by category ID
- `search` (optional) - Search items by name

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Jollof Rice",
      "description": "Delicious Nigerian jollof rice",
      "base_price": 1500.00,
      "image": "https://example.com/images/jollof.jpg",
      "is_available": true,
      "category": {
        "id": 1,
        "name": "Main Dishes"
      },
      "variations": [
        {
          "id": 1,
          "name": "Small",
          "price_adjustment": 0.00
        },
        {
          "id": 2,
          "name": "Large",
          "price_adjustment": 500.00
        }
      ],
      "extras": [
        {
          "id": 1,
          "name": "Extra Chicken",
          "price": 300.00
        }
      ]
    }
  ]
}
```

---

### Get Categories

Get all available menu categories.

**Method:** `GET`

**Endpoint:** `/public/menu/categories`

**Query Parameters:**
- `branch_id` (optional) - Filter categories by branch

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Main Dishes",
      "description": "Our main course dishes",
      "image": "https://example.com/images/main-dishes.jpg",
      "is_active": true
    },
    {
      "id": 2,
      "name": "Drinks",
      "description": "Refreshing beverages",
      "image": "https://example.com/images/drinks.jpg",
      "is_active": true
    }
  ]
}
```

---

### Get Menu Item

Get details of a specific menu item.

**Method:** `GET`

**Endpoint:** `/public/menu/{id}`

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Jollof Rice",
    "description": "Delicious Nigerian jollof rice with chicken",
    "base_price": 1500.00,
    "image": "https://example.com/images/jollof.jpg",
    "is_available": true,
    "category": {
      "id": 1,
      "name": "Main Dishes"
    },
    "variations": [
      {
        "id": 1,
        "name": "Small",
        "price_adjustment": 0.00
      },
      {
        "id": 2,
        "name": "Large",
        "price_adjustment": 500.00
      }
    ],
    "extras": [
      {
        "id": 1,
        "name": "Extra Chicken",
        "price": 300.00
      },
      {
        "id": 2,
        "name": "Extra Sauce",
        "price": 100.00
      }
    ]
  }
}
```

**Error Responses:**
- `404 Not Found` - Menu item not found

---

## Orders

### Place Order

Create a new order and automatically initialize Paystack payment. **All customer orders require online payment via Paystack.**

**Method:** `POST`

**Endpoint:** `/public/customer/orders` or `/customer/orders`

**Headers:**
```
Authorization: Bearer {token}
Content-Type: application/json
```

**Request Body:**
```json
{
  "branch_id": 1,
  "order_type": "delivery",
  "items": [
    {
      "item_id": 1,
      "quantity": 2,
      "variation_id": 2,
      "extras": [
        {
          "id": 1,
          "quantity": 1
        }
      ]
    },
    {
      "item_id": 3,
      "quantity": 1
    }
  ],
  "delivery_address": "123 Main Street, Lagos",
  "delivery_phone": "+2348012345678",
  "notes": "Please deliver before 6pm",
  "coupon_code": "WELCOME10"
}
```

**Order Types:**
- `dine_in` - Dine in at restaurant
- `takeaway` - Takeaway order
- `delivery` - Home delivery (requires `delivery_address` and `delivery_phone`)

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Order placed successfully. Please complete payment.",
  "data": {
    "order": {
      "id": 123,
      "order_number": "ORD-20241215-ABC123",
      "branch_id": 1,
      "customer_id": 1,
      "order_type": "delivery",
      "status": "pending",
      "payment_status": "pending",
      "subtotal": 3500.00,
      "tax_amount": 350.00,
      "discount_amount": 0.00,
      "total_amount": 3850.00,
      "delivery_address": "123 Main Street, Lagos",
      "delivery_phone": "+2348012345678",
      "created_at": "2024-12-15T10:30:00.000000Z",
      "orderItems": [
        {
          "id": 1,
          "item_id": 1,
          "item": {
            "id": 1,
            "name": "Jollof Rice",
            "base_price": 1500.00
          },
          "quantity": 2,
          "unit_price": 2000.00,
          "subtotal": 4000.00
        }
      ],
      "branch": {
        "id": 1,
        "name": "Main Branch",
        "address": "123 Restaurant Street"
      }
    },
    "payment": {
      "authorization_url": "https://checkout.paystack.com/xxxxx",
      "access_code": "xxxxx",
      "reference": "ORD-123-1234567890"
    }
  }
}
```

**Important Notes:**
- The order is created with `payment_status: "pending"` and a Paystack payment is automatically initialized
- Each order is assigned a unique `order_number` in the format: `ORD-YYYYMMDD-XXXXXX` (e.g., `ORD-20241215-ABC123`)
- Redirect the customer to the `authorization_url` from the response to complete payment
- The payment webhook will automatically update the order status when payment is successful
- You can also manually verify payment using the `/public/customer/payment/verify` endpoint
- **All customer orders require online payment via Paystack** - payment must be completed before the order can be processed

**Payment Flow:**
1. Customer places order → Order created with `payment_status: "pending"`
2. Paystack payment initialized → Customer receives `authorization_url`
3. Customer redirected to Paystack → Completes payment
4. Payment webhook/verification → Order `payment_status` updated to `"paid"`
5. Order can now be processed by the restaurant

**Error Responses:**
- `400 Bad Request` - Item not available or payment initialization failed
- `422 Validation Error` - Validation failed

---

### Get My Orders

Get all orders for the authenticated customer.

**Method:** `GET`

**Endpoint:** `/public/customer/orders` or `/customer/orders`

**Headers:**
```
Authorization: Bearer {token}
```

**Query Parameters:**
- `status` (optional) - Filter by status (`pending`, `confirmed`, `preparing`, `ready`, `completed`, `cancelled`)
- `page` (optional) - Page number for pagination
- `per_page` (optional) - Items per page (default: 20)

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 123,
        "order_number": "ORD-20241215-ABC123",
        "status": "preparing",
        "payment_status": "paid",
        "total": 3850.00,
        "order_type": "delivery",
        "created_at": "2024-12-15T10:30:00.000000Z",
        "branch": {
          "id": 1,
          "name": "Main Branch"
        },
        "orderItems": [
          {
            "id": 1,
            "item": {
              "name": "Jollof Rice"
            },
            "quantity": 2
          }
        ]
      }
    ],
    "first_page_url": "http://example.com/api/customer/orders?page=1",
    "last_page": 5,
    "per_page": 20,
    "total": 100
  }
}
```

---

### Get Order Details

Get detailed information about a specific order.

**Method:** `GET`

**Endpoint:** `/public/customer/orders/{id}` or `/customer/orders/{id}`

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": 123,
    "order_number": "ORD-20241215-ABC123",
    "branch_id": 1,
    "customer_id": 1,
    "order_type": "delivery",
    "status": "preparing",
    "payment_status": "paid",
    "subtotal": 3500.00,
    "tax_amount": 350.00,
    "total": 3850.00,
    "delivery_address": "123 Main Street, Lagos",
    "delivery_phone": "+2348012345678",
    "created_at": "2024-12-15T10:30:00.000000Z",
    "branch": {
      "id": 1,
      "name": "Main Branch",
      "address": "123 Restaurant Street",
      "phone": "+2348012345678"
    },
    "orderItems": [
      {
        "id": 1,
        "item": {
          "id": 1,
          "name": "Jollof Rice",
          "base_price": 1500.00,
          "image": "https://example.com/images/jollof.jpg"
        },
        "variation": {
          "id": 2,
          "name": "Large"
        },
        "quantity": 2,
        "unit_price": 2000.00,
        "subtotal": 4000.00
      }
    ],
    "payments": [
      {
        "id": 1,
        "amount": 3850.00,
        "payment_method": "paystack",
        "status": "completed",
        "created_at": "2024-12-15T10:35:00.000000Z"
      }
    ]
  }
}
```

**Error Responses:**
- `404 Not Found` - Order not found or doesn't belong to customer

---

### Cancel Order

Cancel a pending or confirmed order.

**Method:** `POST`

**Endpoint:** `/public/customer/orders/{id}/cancel` or `/customer/orders/{id}/cancel`

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Order cancelled successfully",
  "data": {
    "id": 123,
    "status": "cancelled",
    "cancelled_at": "2024-12-15T11:00:00.000000Z"
  }
}
```

**Error Responses:**
- `400 Bad Request` - Order cannot be cancelled at this stage
- `404 Not Found` - Order not found

---

### Initialize Order Payment

Create an order and initialize Paystack payment in one step. This is the recommended method for customer orders with online payment.

**Method:** `POST`

**Endpoint:** `/public/customer/orders/initialize-payment` or `/customer/orders/initialize-payment`

**Headers:**
```
Authorization: Bearer {token}
Content-Type: application/json
```

**Request Body:**
```json
{
  "branch_id": 1,
  "items": [
    {
      "item_id": 1,
      "quantity": 2,
      "variation_id": null,
      "extras": [
        {
          "id": 1,
          "quantity": 1
        }
      ]
    }
  ],
  "order_type": "delivery",
  "delivery_address": "123 Main Street, Lagos",
  "delivery_phone": "+2348012345678",
  "notes": "Please deliver before 2pm",
  "payment_method": "paystack"
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Order created and payment initialized successfully",
  "data": {
    "order": {
      "id": 123,
      "customer_id": 1,
      "branch_id": 1,
      "total": 3850.00,
      "status": "pending",
      "payment_status": "pending",
      "orderItems": [...]
    },
    "payment": {
      "authorization_url": "https://checkout.paystack.com/xxxxx",
      "access_code": "xxxxx",
      "reference": "ORD-123-1234567890"
    }
  }
}
```

**Note:** After receiving the response, redirect the customer to `authorization_url` to complete payment. The webhook will automatically update the order status when payment is successful.

---

## Payments

### Initialize Payment

Initialize a payment for an order. Supports Paystack, cash, and card payments.

**Method:** `POST`

**Endpoint:** `/public/customer/payment/initialize` or `/customer/payment/initialize`

**Headers:**
```
Authorization: Bearer {token}
```

**Request Body:**
```json
{
  "order_id": 123,
  "payment_method": "paystack",
  "amount": 3850.00
}
```

**Payment Methods:**
- `paystack` - Online payment via Paystack
- `cash` - Cash payment
- `card` - Card payment (POS)

**Response (200 OK) - Paystack:**
```json
{
  "success": true,
  "message": "Payment initialized successfully",
  "data": {
    "payment_id": 456,
    "authorization_url": "https://checkout.paystack.com/xxxxxxxxxxxxx",
    "access_code": "xxxxxxxxxxxxx",
    "reference": "T1234567890"
  }
}
```

**Response (200 OK) - Cash/Card:**
```json
{
  "success": true,
  "message": "Payment initialized successfully",
  "data": {
    "payment_id": 456,
    "status": "pending",
    "payment_method": "cash"
  }
}
```

**Error Responses:**
- `400 Bad Request` - Order not found or invalid amount
- `422 Validation Error` - Validation failed

---

### Verify Payment

Verify a Paystack payment after redirect.

**Method:** `POST`

**Endpoint:** `/public/customer/payment/verify` or `/customer/payment/verify`

**Headers:**
```
Authorization: Bearer {token}
```

**Request Body:**
```json
{
  "reference": "T1234567890"
}
```

**Response (200 OK):**
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
    "paid_at": "2024-12-15T10:40:00.000000Z"
  }
}
```

**Error Responses:**
- `400 Bad Request` - Payment verification failed or already verified
- `404 Not Found` - Payment not found

---

## Profile

### Get Profile

Get the authenticated customer's profile.

**Method:** `GET`

**Endpoint:** `/public/customer/me` or `/customer/me`

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+2348012345678",
    "status": "active",
    "created_at": "2024-12-01T10:00:00.000000Z",
    "roles": [
      {
        "name": "Customer"
      }
    ]
  }
}
```

---

### Update Profile

Update the authenticated customer's profile.

**Method:** `PUT`

**Endpoint:** `/public/customer/profile` or `/customer/profile`

**Headers:**
```
Authorization: Bearer {token}
```

**Request Body:**
```json
{
  "name": "John Updated",
  "phone": "+2348012345679",
  "email": "johnupdated@example.com"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Profile updated successfully",
  "data": {
    "id": 1,
    "name": "John Updated",
    "email": "johnupdated@example.com",
    "phone": "+2348012345679",
    "roles": [
      {
        "name": "Customer"
      }
    ]
  }
}
```

**Error Responses:**
- `422 Validation Error` - Validation failed (e.g., email already taken)

---

### Change Password

Change the authenticated customer's password.

**Method:** `PUT`

**Endpoint:** `/public/customer/change-password` or `/customer/change-password`

**Headers:**
```
Authorization: Bearer {token}
```

**Request Body:**
```json
{
  "current_password": "oldpassword123",
  "password": "newpassword123",
  "password_confirmation": "newpassword123"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Password changed successfully"
}
```

**Error Responses:**
- `400 Bad Request` - Current password is incorrect
- `422 Validation Error` - Validation failed

---

### Update Profile Picture

Update the authenticated customer's profile picture.

**Method:** `POST`

**Endpoint:** `/public/customer/profile/picture` or `/customer/profile/picture`

**Headers:**
```
Authorization: Bearer {token}
Content-Type: multipart/form-data
```

**Request Body (Form Data):**
- `picture` (required) - Image file (jpg, png, gif, max 5MB)

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Profile picture updated successfully",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "profile_picture": "http://example.com/storage/users/profile-pictures/123.jpg",
    "updated_at": "2024-12-15T11:00:00.000000Z"
  }
}
```

**Error Responses:**
- `422 Validation Error` - Invalid file type or size
- `500 Server Error` - Failed to upload image

---

## Shipping Addresses

Manage delivery addresses for the authenticated customer.

### Get All Addresses

Get all shipping addresses for the authenticated customer.

**Method:** `GET`

**Endpoint:** `/public/customer/addresses` or `/customer/addresses`

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "customer_id": 1,
      "label": "Home",
      "address_line_1": "123 Main Street",
      "address_line_2": "Apt 4B",
      "city": "Lagos",
      "state": "Lagos State",
      "postal_code": "100001",
      "country": "Nigeria",
      "phone": "+2348012345678",
      "is_default": true,
      "latitude": 6.5244,
      "longitude": 3.3792,
      "created_at": "2024-12-01T10:00:00.000000Z",
      "updated_at": "2024-12-01T10:00:00.000000Z"
    }
  ]
}
```

---

### Create Address

Create a new shipping address.

**Method:** `POST`

**Endpoint:** `/public/customer/addresses` or `/customer/addresses`

**Headers:**
```
Authorization: Bearer {token}
```

**Request Body:**
```json
{
  "label": "Home",
  "address_line_1": "123 Main Street",
  "address_line_2": "Apt 4B",
  "city": "Lagos",
  "state": "Lagos State",
  "postal_code": "100001",
  "country": "Nigeria",
  "phone": "+2348012345678",
  "is_default": true,
  "latitude": 6.5244,
  "longitude": 3.3792
}
```

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Address created successfully",
  "data": {
    "id": 1,
    "customer_id": 1,
    "label": "Home",
    "address_line_1": "123 Main Street",
    "address_line_2": "Apt 4B",
    "city": "Lagos",
    "state": "Lagos State",
    "postal_code": "100001",
    "country": "Nigeria",
    "phone": "+2348012345678",
    "is_default": true,
    "created_at": "2024-12-15T10:30:00.000000Z"
  }
}
```

**Note:** If `is_default` is set to `true`, all other addresses for the customer will be set to `false`.

---

### Get Address

Get a specific shipping address.

**Method:** `GET`

**Endpoint:** `/public/customer/addresses/{id}` or `/customer/addresses/{id}`

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "customer_id": 1,
    "label": "Home",
    "address_line_1": "123 Main Street",
    "address_line_2": "Apt 4B",
    "city": "Lagos",
    "state": "Lagos State",
    "postal_code": "100001",
    "country": "Nigeria",
    "phone": "+2348012345678",
    "is_default": true,
    "created_at": "2024-12-01T10:00:00.000000Z"
  }
}
```

**Error Responses:**
- `404 Not Found` - Address not found or doesn't belong to customer

---

### Update Address

Update a shipping address.

**Method:** `PUT`

**Endpoint:** `/public/customer/addresses/{id}` or `/customer/addresses/{id}`

**Headers:**
```
Authorization: Bearer {token}
```

**Request Body:**
```json
{
  "label": "Work",
  "address_line_1": "456 Business Street",
  "city": "Lagos",
  "state": "Lagos State",
  "postal_code": "100002",
  "is_default": false
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Address updated successfully",
  "data": {
    "id": 1,
    "label": "Work",
    "address_line_1": "456 Business Street",
    "is_default": false,
    "updated_at": "2024-12-15T11:00:00.000000Z"
  }
}
```

---

### Delete Address

Delete a shipping address.

**Method:** `DELETE`

**Endpoint:** `/public/customer/addresses/{id}` or `/customer/addresses/{id}`

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Address deleted successfully"
}
```

**Error Responses:**
- `404 Not Found` - Address not found or doesn't belong to customer
- `400 Bad Request` - Cannot delete default address (set another as default first)

---

## Transaction History

Get a consolidated transaction history including orders, payments, and loyalty points.

**Method:** `GET`

**Endpoint:** `/public/customer/transactions` or `/customer/transactions`

**Headers:**
```
Authorization: Bearer {token}
```

**Query Parameters:**
- `type` (optional) - Filter by type: `all` (default), `orders`, `payments`, `loyalty`
- `from_date` (optional) - Filter from date (YYYY-MM-DD)
- `to_date` (optional) - Filter to date (YYYY-MM-DD)
- `page` (optional) - Page number for pagination
- `per_page` (optional) - Items per page (default: 20)

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 123,
      "type": "order",
      "reference": "ORD-20241215-ABC123",
      "description": "Order #ORD-20241215-ABC123",
      "amount": 3850.00,
      "status": "completed",
      "payment_status": "paid",
      "payment_method": "paystack",
      "date": "2024-12-15T10:30:00.000000Z",
      "branch": "Main Branch",
      "items_count": 3
    },
    {
      "id": 456,
      "type": "payment",
      "reference": "T1234567890",
      "description": "Payment for Order #ORD-20241215-ABC123",
      "amount": 3850.00,
      "status": "completed",
      "payment_method": "paystack",
      "date": "2024-12-15T10:35:00.000000Z",
      "branch": "Main Branch"
    },
    {
      "id": 789,
      "type": "loyalty",
      "reference": "Order #ORD-20241215-ABC123",
      "description": "Earned 385 points for Order #ORD-20241215-ABC123",
      "amount": 385.00,
      "status": "earned",
      "payment_method": null,
      "date": "2024-12-15T10:40:00.000000Z",
      "branch": "Main Branch"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 20,
    "total": 45,
    "last_page": 3,
    "from": 1,
    "to": 20
  }
}
```

**Transaction Types:**
- `order` - Order transactions
- `payment` - Payment transactions
- `loyalty` - Loyalty points transactions

---

## Loyalty Program

The loyalty program allows customers to earn points from orders and redeem rewards. Points are automatically awarded when orders are completed.

### Get Loyalty Points

Get the authenticated customer's current loyalty points balance and tier information.

**Method:** `GET`

**Endpoint:** `/public/customer/loyalty/points` or `/customer/loyalty/points` (Note: Currently only available under `/api/frontend/customer/loyalty/points`)

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "total_earned": 1250,
    "total_redeemed": 200,
    "available_points": 1050,
    "tier": {
      "id": 2,
      "name": "Gold",
      "min_points": 1000,
      "discount_percentage": 10.00,
      "benefits": "10% discount on all orders"
    }
  }
}
```

---

### Get Loyalty Transactions

Get the authenticated customer's loyalty points transaction history.

**Method:** `GET`

**Endpoint:** `/public/customer/loyalty/transactions` or `/customer/loyalty/transactions` (Note: Currently only available under `/api/frontend/customer/loyalty/transactions`)

**Headers:**
```
Authorization: Bearer {token}
```

**Query Parameters:**
- `page` (optional) - Page number for pagination
- `per_page` (optional) - Items per page (default: 20)

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 1,
        "type": "earned",
        "points": 150,
        "description": "Points earned from order #ORD-20241215-ABC123",
        "order_id": 123,
        "order": {
          "id": 123,
          "order_number": "ORD-20241215-ABC123",
          "total": 3850.00
        },
        "created_at": "2024-12-15T10:30:00.000000Z"
      },
      {
        "id": 2,
        "type": "redeemed",
        "points": -200,
        "description": "Redeemed for discount on order #ORD-20241215-XYZ789",
        "order_id": 124,
        "order": {
          "id": 124,
          "order_number": "ORD-20241215-XYZ789",
          "total": 2500.00
        },
        "created_at": "2024-12-14T15:20:00.000000Z"
      }
    ],
    "first_page_url": "http://example.com/api/customer/loyalty/transactions?page=1",
    "last_page": 3,
    "per_page": 20,
    "total": 45
  }
}
```

**Transaction Types:**
- `earned` - Points earned from completed orders
- `redeemed` - Points redeemed for discounts or rewards
- `adjusted` - Manual adjustment by admin

---

### Get Loyalty Program Details

Get information about the loyalty program, including available tiers and benefits.

**Method:** `GET`

**Endpoint:** `/public/customer/loyalty/program` or `/customer/loyalty/program` (Note: Currently only available under `/api/frontend/customer/loyalty/program`)

**Headers:**
```
Authorization: Bearer {token}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "tiers": [
      {
        "id": 1,
        "name": "Bronze",
        "min_points": 0,
        "discount_percentage": 0.00,
        "benefits": "Welcome to our loyalty program"
      },
      {
        "id": 2,
        "name": "Gold",
        "min_points": 1000,
        "discount_percentage": 10.00,
        "benefits": "10% discount on all orders, priority support"
      },
      {
        "id": 3,
        "name": "Platinum",
        "min_points": 5000,
        "discount_percentage": 15.00,
        "benefits": "15% discount on all orders, free delivery, birthday rewards"
      }
    ],
    "points_per_naira": 1,
    "point_value": 0.5
  }
}
```

**Notes:**
- Points are automatically calculated based on order total (1 point per ₦1 spent)
- Points are awarded when order status changes to `completed`
- Points can be redeemed during checkout for discounts
- Tier benefits (discounts) are automatically applied based on current tier
- Each point is worth ₦0.50 when redeemed

---

## Error Handling

All endpoints follow a consistent error response format:

### Validation Error (422)
```json
{
  "success": false,
  "errors": {
    "field_name": ["Error message 1", "Error message 2"]
  }
}
```

### Unauthorized (401)
```json
{
  "success": false,
  "message": "Invalid credentials"
}
```

### Forbidden (403)
```json
{
  "success": false,
  "message": "Access denied. This endpoint is for customers only."
}
```

### Not Found (404)
```json
{
  "success": false,
  "message": "Resource not found"
}
```

### Method Not Allowed (405)
```json
{
  "success": false,
  "message": "The GET method is not supported for route /api/public/contact. Supported methods: POST."
}
```

This error occurs when you use the wrong HTTP method for an endpoint. Always check the **Method:** line in the documentation for each endpoint.

### Server Error (500)
```json
{
  "success": false,
  "message": "Internal server error",
  "error": "Error details (only in development)"
}
```

---

## Order Statuses

- `pending` - Order received, awaiting confirmation
- `confirmed` - Order confirmed by restaurant
- `preparing` - Order is being prepared
- `ready` - Order is ready for pickup/delivery
- `out_for_delivery` - Order is out for delivery
- `delivered` - Order has been delivered
- `completed` - Order completed
- `cancelled` - Order cancelled

## Payment Statuses

- `pending` - Payment pending
- `completed` - Payment completed
- `failed` - Payment failed
- `refunded` - Payment refunded

## Payment Methods

- `paystack` - Online payment via Paystack
- `cash` - Cash payment
- `card` - Card payment (POS)

---

## Rate Limiting

API endpoints are rate-limited to prevent abuse. If you exceed the rate limit, you will receive a `429 Too Many Requests` response.

---

## Webhooks

### Paystack Webhook

Paystack sends webhook notifications to verify payments. The webhook endpoint is:

**Method:** `POST`

**Endpoint:** `/public/payment/paystack/webhook`

This endpoint is handled automatically by the backend and does not require client-side implementation.

---

## Notes

1. All prices are in Nigerian Naira (₦)
2. All timestamps are in ISO 8601 format (UTC)
3. Image URLs are absolute URLs pointing to the server
4. Pagination uses Laravel's default pagination format
5. Tokens expire after 24 hours of inactivity (configurable)
6. Password reset tokens expire after 1 hour

---

## Support

For API support or questions, please contact the development team or refer to the backend documentation.

