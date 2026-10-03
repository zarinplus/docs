# Collateral Registration API — Partner Documentation

## Overview

The Collateral Registration API enables authorized partners to register a collateral record on the ZarinPlus platform on behalf of a specific user.

Once the request is accepted, a collateral record is created and linked to the user's profile. The collateral remains active until it is released by ZarinPlus.

## Endpoint

| Item | Value |
|---|---|
| URL | `https://api.zarinplus.com/credit/partner/collateral-registration/` |
| Method | `POST` |
| Content-Type | `application/json` |
| Authentication | `AccessToken` in the request body |
| Access Restriction | The endpoint is restricted to a predefined IP whitelist |

## Authentication and Access Control

Two conditions must be met for the request to be processed:

1. **Whitelisted IP** — the request must originate from an IP address that has been previously registered with ZarinPlus. Requests from non-whitelisted IPs are rejected with HTTP `401`.
2. **Valid Access Token** — a valid, active `AccessToken` must be provided in the request body. Tokens are issued by ZarinPlus per partner.

## Request Parameters

| Field | Type | Required | Description |
|---|---|---|---|
| `AccessToken` | string | Yes | Partner access token issued by ZarinPlus. Must be active. |
| `ContractNumber` | string | Yes | Contract number on the partner side. Stored as the unique reference for this collateral. |
| `AmountGold` | number | Yes | Collateral amount in grams of gold. Must be a numeric value greater than zero (e.g. `2.5`). |
| `AmountRial` | number | No | Reserved field. Currently not validated and ignored by the API. |
| `NationalCode` | string | Yes | National identification code of the user. |
| `PhoneNumber` | string | Yes | Mobile number of the user, in either `09xxxxxxxxx` or `989xxxxxxxxx` format. |

## Prerequisites

- The user must already be registered on the ZarinPlus platform with the exact combination of the provided `NationalCode` and `PhoneNumber`. Otherwise, the API returns `Object user does not exist!`.
- The user must have an active profile. Otherwise, the API returns `Object profile does not exist!`.

## Sample Request

```bash
curl --location 'https://api.zarinplus.com/credit/partner/collateral-registration/' \
--header 'Content-Type: application/json' \
--data '{
  "AccessToken": "PARTNER_ACCESS_TOKEN",
  "ContractNumber": "123456",
  "AmountGold": "2.5",
  "AmountRial": "",
  "NationalCode": "0012345678",
  "PhoneNumber": "09121234567"
}'
```

## Success Response

```json
{
  "status": true,
  "message": "successful",
  "data": {}
}
```

Upon success:

- A collateral record is created with `Active` status.

## Error Responses

Errors follow this structure:

```json
{
  "status": false,
  "message": "Invalid AccessToken",
  "data": ""
}
```

| HTTP Code | message | Description |
|---|---|---|
| 401 | `you do not have permission` | Requesting IP is not whitelisted. |
| 400 | `Send AccessToken` | `AccessToken` is missing from the request body. |
| 400 | `Invalid AccessToken` | Token does not exist or is inactive. |
| 400 | `Send ContractNumber` | `ContractNumber` is missing. |
| 400 | `Send AmountGold` | `AmountGold` is missing. |
| 400 | `Invalid AmountGold` | `AmountGold` is not a valid number. |
| 400 | `Invalid amount` | `AmountGold` is less than or equal to zero. |
| 400 | `Send NationalCode` | `NationalCode` is missing. |
| 400 | `Send PhoneNumber` | `PhoneNumber` is missing. |
| 200 | `Object user does not exist!` | No user found with the given national code and phone number combination. |
| 200 | `Object profile does not exist!` | The user has no active profile. |

## Important Notes

- **Success detection:** Always determine the outcome of the call from the `status` field in the response body (`true` = success). Some validation errors are returned with HTTP `200`.
- **`AmountRial`:** This field is reserved for future use and is currently ignored.
- **`ContractNumber`:** Partners must keep this value, as it serves as the mutual reference between both systems.