# ZarinPlus Cashback Integration API

**Base URL** `https://api.zarinplus.com`

This reference describes requests in the supplied Postman collection. Fields marked **disabled** are saved in Postman but omitted from the exported request. Response formats and status codes were checked against the backend source. Example values are illustrative, not captured production responses.

| Method | Path | Purpose | Authorization shown |
| --- | --- | --- | --- |
| GET | `/store/source-users-transactions` | Retrieve user transactions | `Token <token>` |
| POST | `/cards/` | Submit a card number | `Token <token>` |
| POST | `/store/around` | Search for nearby stores | No header configured |

The examples use `ZARINPLUS_TOKEN` and `CARD_NUMBER` as environment variables. Both POST requests use `multipart/form-data`.

## Retrieve user transactions

`GET /store/source-users-transactions`

The exported request sends no query parameters and has no body. The endpoint supports these optional query parameters:

| Query parameter | Example |
| --- | --- |
| `last_id` | `2` |
| `user_id` | `396850` |
| `date` | `2022-07-28` |

```bash
curl 'https://api.zarinplus.com/store/source-users-transactions' \
  --header "Authorization: Token ${ZARINPLUS_TOKEN}"
```

### Responses

**HTTP 200 — Success**

```json
{
  "status": true,
  "message": "successful",
  "data": [
    {
      "id": 123456,
      "user": {
        "id": 396850,
        "phone_number": "09120000000",
        "first_name": "علی",
        "last_name": "رضایی"
      },
      "user_card": "603799******1234",
      "user_card_hash": "EXAMPLE_SHA256_HASH",
      "store": {
        "id": 100,
        "name": "فروشگاه نمونه"
      },
      "amount": 1000000,
      "time": 1659004200.0,
      "date": "2022-07-28T12:00:00",
      "cashback": 50000.0
    }
  ]
}
```

`cashback` is calculated as `amount × store_reward_percent / 100`. When no transactions match, `data` is an empty array.

**HTTP 404 — No source associated with the authenticated user**

```json
{
  "status": false,
  "message": "Object user_source does not exist!",
  "data": ""
}
```

## Submit a card

`POST /cards/`

| Form field | Example | Postman state |
| --- | --- | --- |
| `card_number` | `${CARD_NUMBER}` | Enabled |

```bash
curl 'https://api.zarinplus.com/cards/' \
  --header "Authorization: Token ${ZARINPLUS_TOKEN}" \
  --form-string "card_number=${CARD_NUMBER}"
```

### Responses

**HTTP 200 — New card added**

The current implementation places the new card ID inside `message`:

```json
{
  "status": true,
  "message": {
    "id": 12345
  },
  "data": ""
}
```

**HTTP 200 — Existing card reassigned or restored**

Depending on the branch taken, a successful response can instead be:

```json
{
  "status": true,
  "message": "Card added successfully.",
  "data": {
    "id": 12345
  }
}
```

Restoring an archived card already belonging to the user can return `"Card information has been updated."` as the message. Some source-specific success branches return `"Card added successfully."` with an empty string in `data`.

**HTTP 200 — Invalid card**

```json
{
  "status": false,
  "message": "Enter a valid card number",
  "data": ""
}
```

**HTTP 200 — Card already registered**

```json
{
  "status": false,
  "message": "Card is already exists for you",
  "data": {
    "id": 12345
  }
}
```

Other business failures also return HTTP 200 with `status: false`:

| Message | Meaning |
| --- | --- |
| `Enter a valid card number (not owner of the card)` | Card ownership verification failed |
| `card already exists!` | The existing card could not be reassigned |
| `You have more than 10 cards!` | Card limit check failed |

**HTTP 404 — Card source not found**

```json
{
  "status": false,
  "message": "Object source does not exist!",
  "data": ""
}
```

Clients must inspect the JSON `status` field: HTTP 200 alone does not indicate that the card was added.

## Search for nearby stores

`POST /store/around`

| Form field | Example | Notes |
| --- | --- | --- |
| `location` | `POINT(51.432796 35.730940)` | WKT point with longitude followed by latitude |
| `query` | `غذایی` | Search text |
| `sortby` | `distance` | The collection lists `distance` and `reward` |
| `credit` | `true` | Sent as text. The collection also mentions null without specifying how to send it. |

The following URL query parameters are disabled in the collection. The enabled `location` field is sent in the multipart body.

| Query parameter | Example |
| --- | --- |
| `category` | `1` |
| `city` | `1` |
| `name__contains` | `غذایی` |
| `location` | `POINT(51.432796 35.730940)` |
| `radius` | `1` |

```bash
curl 'https://api.zarinplus.com/store/around' \
  --form-string 'location=POINT(51.432796 35.730940)' \
  --form-string 'query=غذایی' \
  --form-string 'sortby=distance' \
  --form-string 'credit=true'
```

### Responses

**HTTP 200 — Success**

The response contains an array of store objects. This example represents no matching stores:

```json
{
  "status": true,
  "message": "successful",
  "data": []
}
```

For matching results, each array element contains store fields including `id`, `name`, `address`, `geo_point`, `reward_percent`, and `credit`, plus nested `category`, `sub_category`, `city`, and `store_pos` information.

The supplied request uses the default response version (`v=1`), which serializes all store model fields except `wallet_id`. Therefore, the complete store object is broader than the fields listed here.

Results default to 30 stores per page. Additional multipart fields `page` and `page_size` control pagination; `page_size` is capped at 100. The response has no total-count or next-page envelope.

