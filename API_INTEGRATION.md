# Polyflow API Integration Guide

## Overview

Polyflow provides a REST API for submitting and tracking orders to Polymarket with built-in:
- ✅ Rate limiting and burst handling
- ✅ Automatic retries with exponential backoff
- ✅ Order lifecycle tracking
- ✅ Webhook notifications
- ✅ 0.6% processing fee

**Base URL**: `https://api.polyflow.io` (or your deployment URL)

---

## Authentication

All API requests require an API key in the header:

```http
X-Polyflow-API-Key: your-platform-api-key
```

### Getting Your API Key

Contact Polyflow admin to get your platform registered and receive:
- Platform ID
- API Key
- Webhook URL configuration

---

## Platform Endpoints

### 1. Submit Order

Submit an order to be processed through Polymarket.

**Endpoint**: `POST /v1/orders`

**Headers**:
```http
X-Polyflow-API-Key: your-api-key
Content-Type: application/json
```

**Request Body**:
```json
{
  "client_order_id": "your-unique-order-id-123",
  "market_id": "POLYMARKET-MARKET-ID",
  "side": "buy",
  "amount": "100.0",
  "price": "0.55",
  "time_in_force": "GTC"
}
```

**Parameters**:
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `client_order_id` | string | ✅ | Your unique order identifier (for idempotency) |
| `market_id` | string | ✅ | Polymarket market ID |
| `side` | string | ✅ | `"buy"` or `"sell"` |
| `amount` | string | ✅ | Order size (shares) |
| `price` | string | ✅ | Price per share (0-1) |
| `time_in_force` | string | ✅ | `"GTC"`, `"FOK"`, or `"IOC"` |

**Response** (202 Accepted):
```json
{
  "order_id": "550e8400-e29b-41d4-a716-446655440000",
  "client_order_id": "your-unique-order-id-123",
  "status": "queued",
  "notional": "55.0",
  "fee_bps": 60,
  "fee_amount": "0.33",
  "created_at": "2026-01-17T03:00:00.000Z"
}
```

**Idempotency**: Submitting the same `client_order_id` multiple times returns the existing order.

---

### 2. Get Order by ID

Retrieve order status by Polyflow order ID.

**Endpoint**: `GET /v1/orders/{order_id}`

**Example**:
```bash
curl -H "X-Polyflow-API-Key: your-key" \
  https://api.polyflow.io/v1/orders/550e8400-e29b-41d4-a716-446655440000
```

**Response**:
```json
{
  "order_id": "550e8400-e29b-41d4-a716-446655440000",
  "client_order_id": "your-unique-order-id-123",
  "status": "confirmed",
  "polymarket_order_id": "pm-order-789",
  "last_error_code": null,
  "last_error_message": null,
  "notional": "55.0",
  "fee_bps": 60,
  "fee_amount": "0.33",
  "timestamps": {
    "created_at": "2026-01-17T03:00:00.000Z",
    "updated_at": "2026-01-17T03:00:05.000Z",
    "sent_at": "2026-01-17T03:00:02.000Z",
    "confirmed_at": "2026-01-17T03:00:05.000Z",
    "failed_at": null
  }
}
```

---

### 3. Get Order by Client ID

Retrieve order using your own `client_order_id`.

**Endpoint**: `GET /v1/orders/by-client-id/{client_order_id}`

**Example**:
```bash
curl -H "X-Polyflow-API-Key: your-key" \
  https://api.polyflow.io/v1/orders/by-client-id/your-unique-order-id-123
```

**Response**: Same as Get Order by ID

---

### 4. Check Fee Balance

View accumulated fees and payment info.

**Endpoint**: `GET /v1/billing/fees`

**Response**:
```json
{
  "platformId": "uuid",
  "totalOrders": 1250,
  "totalOwed": 750.00,
  "totalPaid": 500.00,
  "outstanding": 250.00,
  "paymentAddress": "0xeA40a224c2037163b59E55b8FA0F980670334553",
  "network": "polygon",
  "token": "USDC"
}
```

---

### 5. Get Payment Instructions

Get instructions for paying accumulated fees.

**Endpoint**: `GET /v1/billing/payment-instructions`

**Response**:
```json
{
  "address": "0xeA40a224c2037163b59E55b8FA0F980670334553",
  "network": "polygon",
  "token": "USDC",
  "contract": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
  "amount": 250.00,
  "instructions": "Send 250.00 USDC on Polygon to 0xeA40a224c2037163b59E55b8FA0F980670334553..."
}
```