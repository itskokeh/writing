# Paystack API Documentation

The Paystack API enables developers to integrate secure payment processing into
their applications. It supports features like one-time and recurring payments,
card tokenization, refunds, and transaction verification. This documentation
covers core endpoints, authentication, and examples to help you get started.

## Authentication

- All requests require a `Bearer` token in the `Authorization` header.
- Get your secret key from the [Paystack Dashboard](https://dashboard.paystack.com).

## Endpoints

### 1. Initialize a Transaction

**Endpoint**: `POST /transaction/initialize`  
**Purpose**: Start a payment process.

**Request Parameters (Body)**:

- `email` (string): Customer's email.
- `amount` (integer): Amount in **kobo** (e.g., `5000` = ₦50).

**Example Request**:

```javascript
const https = require('https')

const params = JSON.stringify({
  "email": "customer@email.com",
  "amount": "5000"
})

const options = {
  hostname: 'api.paystack.co',
  port: 443,
  path: '/transaction/initialize',
  method: 'POST',
  headers: {
    Authorization: 'Bearer SECRET_KEY',
    'Content-Type': 'application/json'
  }
}

const req = https.request(options, res => {
  let data = ''

  res.on('data', (chunk) => {
    data += chunk
  });

  res.on('end', () => {
    console.log(JSON.parse(data))
  })
}).on('error', error => {
  console.error(error)
})

req.write(params)
req.end()
```

**Example Response**:

```json
{
  "status": true,
  "message": "Authorization URL created",
  "data": {
    "authorization_url": "https://checkout.paystack.com/3ni8kdavz62431k",
    "access_code": "3ni8kdavz62431k",
    "reference": "re4lyvq3s3"
  }
}
```

### 2. Verify Transaction

**Endpoint**: `GET /transaction/verify/:reference`  
**Purpose**: Confirm a transaction's status.

**Path Parameter**:

- `reference` (string): Unique ID for the transaction.

**Example Request**:

```javascript
const https = require('https')

const options = {
  hostname: 'api.paystack.co',
  port: 443,
  path: '/transaction/verify/:reference',
  method: 'GET',
  headers: {
    Authorization: 'Bearer SECRET_KEY'
  }
}

https.request(options, res => {
  let data = ''

  res.on('data', (chunk) => {
    data += chunk
  });

  res.on('end', () => {
    console.log(JSON.parse(data))
  })
}).on('error', error => {
  console.error(error)
})
```

**Example Response (Success)**:

```json
{
  "status": true,
  "message": "Verification successful",
  "data": {
    "id": 4099260516,
    "domain": "test",
    "status": "success",
    "reference": "re4lyvq3s3",
    "receipt_number": null,
    "amount": 40333,
    "message": null,
    "gateway_response": "Successful",
    "paid_at": "2024-08-22T09:15:02.000Z",
    "created_at": "2024-08-22T09:14:24.000Z",
    "channel": "card",
    "currency": "NGN",
    "ip_address": "197.210.54.33",
    "metadata": "",
    "log": {
      "start_time": 1724318098,
      "time_spent": 4,
      "attempts": 1,
      "errors": 0,
      "success": true,
      "mobile": false,
      "input": [],
      "history": [
        {
          "type": "action",
          "message": "Attempted to pay with card",
          "time": 3
        },
        {
          "type": "success",
          "message": "Successfully paid with card",
          "time": 4
        }
      ]
    },
    "fees": 10283,
    "fees_split": null,
    "authorization": {
      "authorization_code": "AUTH_uh8bcl3zbn",
      "bin": "408408",
      "last4": "4081",
      "exp_month": "12",
      "exp_year": "2030",
      "channel": "card",
      "card_type": "visa ",
      "bank": "TEST BANK",
      "country_code": "NG",
      "brand": "visa",
      "reusable": true,
      "signature": "SIG_yEXu7dLBeqG0kU7g95Ke",
      "account_name": null
    },
    "customer": {
      "id": 181873746,
      "first_name": null,
      "last_name": null,
      "email": "demo@test.com",
      "customer_code": "CUS_1rkzaqsv4rrhqo6",
      "phone": null,
      "metadata": null,
      "risk_action": "default",
      "international_format_phone": null
    },
    "plan": null,
    "split": {},
    "order_id": null,
    "paidAt": "2024-08-22T09:15:02.000Z",
    "createdAt": "2024-08-22T09:14:24.000Z",
    "requested_amount": 30050,
    "pos_transaction_data": null,
    "source": null,
    "fees_breakdown": null,
    "connect": null,
    "transaction_date": "2024-08-22T09:14:24.000Z",
    "plan_object": {},
    "subaccount": {}
  }
}
```

### 3. Error Codes

Common HTTP status and fixes.

| **Status Code** | **Meaning** | **Resolution** |
|-----|-----|-----|
| `200` | **OK** - Request succeeded | No action needed. The response body contains the requested data |
| `201` | Created - Resource (e.g., transaction, customer) was successfully created | Check the response body for the newly created resource details |
| `400` | **Bad Request** - Invalid request syntax or missing parameters | 1. Validate all required fields (e.g., email, amount) <br> 2. Ensure data formats are correct <br> 3. Check for typos in JSON payloads |
| `401` | Unauthorized - Invalid or missing API Key | 1. Verify your `Authorization` header: `Bearer YOUR_SECRET_KEY` <br> 2. Ensure you're using the correct test/live key <br> 3. Regenerate key if expired |
| `404` | **Not Found** - Resource (e.g., transaction) doesn't exist | Double-check the `reference` or `ID` in the URL <br> Verify the resource wasn't deleted |
| `5xx` | **Server Error** - Paystack's Internal Issue | 1. Retry the request with exponential backoff <br> 2. Check [Paystack's Status Page](https://status.paystack.com/) <br> 3. Contact support if the issue persists |

#### **Examples**

**200 Ok**:

```json
{
  "status": true,
  "message": "Charge attempted",
  "data": {
    "amount": 200,
    "currency": "NGN",
    "transaction_date": "2017-05-24T05:56:12.000Z",
    "status": "success",
    "reference": "zuvbpizfcf2fs7y",
    "domain": "test",
    "metadata": {
      "custom_fields": [
        {
          "display_name": "Merchant name",
          "variable_name": "merchant_name",
          "value": "Van Damme"
        },
        {
          "display_name": "Paid Via",
          "variable_name": "paid_via",
          "value": "API call"
        }
      ]
    },
    "gateway_response": "Successful",
    "message": null,
    "channel": "card",
    "ip_address": "54.154.89.28, 162.158.38.82, 172.31.38.35",
    "log": null,
    "fees": 3,
    "authorization": {
      "authorization_code": "AUTH_6tmt288t0o",
      "bin": "408408",
      "last4": "4081",
      "exp_month": "12",
      "exp_year": "2020",
      "channel": "card",
      "card_type": "visa visa",
      "bank": "TEST BANK",
      "country_code": "NG",
      "brand": "visa",
      "reusable": true,
      "signature": "SIG_uSYN4fv1adlAuoij8QXh",
      "account_name": "BoJack Horseman"
    },
    "customer": {
      "id": 14571,
      "first_name": null,
      "last_name": null,
      "email": "test@email.co",
      "customer_code": "CUS_hns72vhhtos0f0k",
      "phone": null,
      "metadata": null,
      "risk_action": "default"
    },
    "plan": null
  }
}
```

**200 OTP**:

```json
{
  "status": true,
  "message": "Charge attempted",
  "data": {
    "reference": "0t4gwo2ft6q0n9h",
    "status": "send_otp",
    "display_text": "Please send OTP (none will be sent to your phone)"
  }
}
```

**200 Pending**:

```json
{
  "status": true,
  "message": "Reference check successful",
  "data": {
    "reference": "5bwib5v6anhe9xa",
    "status": "pending"
  }
}
```

**200 PIN**:

```json
{
  "status": true,
  "message": "Charge attempted",
  "data": {
    "reference": "apg8mnjf5yg7zp8",
    "status": "send_pin"
  }
}
```

**200 Failed**:

```json
{
  "status": true,
  "message": "Reference check successful",
  "data": {
    "reference": "ojk8k0bimgftf0x",
    "status": "failed",
    "message": "Insufficient funds"
  }
}
```

**200 Bank Auth**:

```json
{
  "status": true,
  "message": "Charge attempted",
  "data": {
    "reference": "apg8mnjf5yg7zp8",
    "url": "https://standard.paystack.co/close",
    "status": "open_url"
  }
}
```

**200 Phone**:

```json
{
  "status": true,
  "message": "Charge attempted",
  "data": {
    "reference": "0t4gwo2ft6q0n9h",
    "status": "send_phone",
    "display_text": "Please send phone"
  }
}
```

**200 Birthday**:

```json
{
  "status": true,
  "message": "Charge attempted",
  "data": {
    "reference": "0t4gwo2ft6q0n9h",
    "status": "send_birthday",
    "display_text": "Please send your birthday"
  }
}
```

**400 Bad Request**:

```json
{
  "status": true,
  "message": "Charge attempted",
  "data": {
    "reference": "0t4gwo2ft6q0n9h",
    "status": "send_phone",
    "display_text": "Please send phone"
  }
}
```

## Conclusion

This documentation covers the core endpoints and error handling for integrating
Paystack’s API. For advanced use cases (e.g., webhooks, subscriptions, or bulk
transactions), refer to the official Paystack documentation below.

### Version Information

This documentation is current as of **April 2024** and aligns with Paystack's official API specifications. Since Paystack maintains a continuously updated API:

- Always refer to the [official Paystack API documentation](https://paystack.com/docs/api) for the most current reference
- Check their [changelog](https://paystack.com/docs/changelog) for updates
- API base URL: `https://api.paystack.co` (versionless endpoint)
  
**Need Help?**

Contact [Paystack Support](https://paystack.com/contact) or ask the developer community (e.g., [Stack Overflow](https://stackoverflow.com/questions/tagged/paystack)).  

> **Note**: Paystack typically rolls out changes gradually and maintains backward compatibility. Significant changes are announced via their [blog](https://paystack.com/blog) and documentation.
