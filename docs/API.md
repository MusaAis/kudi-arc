# Kudi Arc API Reference

Base URL: `http://129.80.199.123` (testnet)

---

## Rates

### GET /api/rates
Returns live FX rates for USDC/EURC pairs and NGN equivalent.

**Response:**
```json
{
  "pairs": {
    "USDC/EURC": { "rate": 0.8638, "display": "1 USDC = 0.8638 EURC" },
    "EURC/USDC": { "rate": 1.1576, "display": "1 EURC = 1.1576 USDC" },
    "USDC/NGN":  { "rate": 1382.5, "display": "1 USDC ≈ ₦1,382" },
    "EURC/NGN":  { "rate": 1592.5, "display": "1 EURC ≈ ₦1,592" }
  },
  "last_updated": 1774663084,
  "contract": "0x8a10D0e61201000B5456EC725165892B08832C5f"
}
```

---

## Countries

### GET /api/countries
Returns all supported payout countries with live rates.

**Response:**
```json
{
  "countries": [
    {
      "code": "NG",
      "name": "Nigeria",
      "currency": "NGN",
      "symbol": "₦",
      "flag": "🇳🇬",
      "partner": "Bitnob",
      "rate_per_usd": 1382.5,
      "rate_per_eurc": 1592.5,
      "display_usd": "1 USDC = ₦1,382.5",
      "display_eur": "1 EURC = ₦1,592.5"
    }
  ]
}
```

---

## Banks

### GET /api/banks
Returns list of supported Nigerian banks.

---

## Send

### POST /api/send/payout
Universal payout endpoint for all countries.

**Request:**
```json
{
  "wallet": "0x7B77...",
  "amount_usdc": 10,
  "token": "USDC",
  "country_code": "NG",
  "currency": "NGN",
  "recipient_name": "John Doe",
  "account_number": "0123456789",
  "bank_name": "GTBank",
  "bank_code": "058",
  "signature": "0x...",
  "approve_tx": "0x...",
  "method": "bank"
}
```

**Response:**
```json
{
  "status": "mock_success",
  "reference": "KUDI-NGN-ABCD1234",
  "currency": "NGN",
  "payout_amount": 13795.2,
  "fee_usdc": 0.03,
  "rate_used": 1382.5,
  "message": "[SANDBOX] ₦13,795 would be sent to John Doe"
}
```

---

## History

### GET /api/history/:wallet
Returns transaction history for a wallet address.

**Response:**
```json
{
  "wallet": "0x7B77...",
  "count": 5,
  "transactions": [
    {
      "tx_type": "swap",
      "from_token": "USDC",
      "to_token": "EURC",
      "amount_in": 1.32,
      "amount_out": 1.14,
      "tx_hash": "0xecc2...",
      "created_at": "2026-03-27 11:19:28"
    }
  ]
}
```

---

## Yield

### GET /api/yield/stats
Returns USYC yield statistics.

### GET /api/yield/balance/:wallet
Returns user's USYC position and earnings.

---

## Health

### GET /api/health
Returns service health status.
