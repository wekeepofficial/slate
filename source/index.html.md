---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

In order for WeKeep to record your sales, you must expose an API which WeKeep will poll at regular intervals (typically once a day). The format of the request and response for each type of data is specified in this documentation.

# API endpoint format and versioning

Your endpoints base url must be in the following format:  
`<base_domain>/wekeep-api/<wekeep_version_number>`  

The current `wekeep_version_number` is `v1`.  

Example of a base url with `v1`:  
`https://www.yourcompany.com/wekeep-api/v1`

# Setup

You must enter your endpoint urls in your [WeKeep account settings](https://www.wekeep.co/c/account/). You must also enter the authentication key as explained in [Authentication](#authentication).

# Authentication

> To authorize, use this code:


```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: PRIVATE_KEY"
```

> Make sure to replace `PRIVATE_KEY` with your API key.

Your API must authenticate WeKeep using an authorization header and private key in each request like following:

`Authorization: PRIVATE_KEY`

Make sure to enter the private key in your [WeKeep account settings](https://www.wekeep.co/c/account/).

<aside class="notice">
You must replace <code>PRIVATE_KEY</code> with your personal API key.
</aside>

# Sales

## Get All Sales


```shell
curl "https://www.yourcompany.com/wekeep-api/v1/sales?from_date=2019-01-01&to_date=2019-01-31"
  -H "Authorization: PRIVATE_KEY"
```

> Returns an array of Sales objects:

```json
[
  {
    "id": 1,
    "customer_name": "Alpha & Co",
    "date": "2019-03-23",
    "invoice_number": "AE00123",
    "currency_code": "AED",
    "currency_rate": 1,
    "total_amount": 105,
    "tax_amount": 5,
    "status": "PAID",
    "line_items": [
      {
        "description": "Some description",
        "quantity": 1,
        "price": 100,
        "tax_rate": 0.05,
        "tax_group": "SR_DUBAI",
        "tax_amount": 5,
      },
    ],
    "payments": [
      {
        "reference1": "ABCD1234",
        "reference2": "xyz123",
        "date": "2019-01-01",
        "currency_code": "AED",
        "amount": 105,
        "currency_rate": 1,
        "paid_from_account_id": "xyz123",

      }
    ]
  },
  ...
]
```

This endpoint must return all sales transactions.

### HTTP Request

`GET /sales?from_date=2019-01-01&to_date=2019-01-31`

### Query Parameters

Parameter | Description
--------- | -----------
from_date | Sales transactions starting from date. In the format <code>YYYY-MM-DD</code>.
to_date | Sales transactions up to and including date. In the format <code>YYYY-MM-DD</code>.


### Response

Parameter | Description
--------- | -----------
id | <code>string</code> Unique id internal to your systems.
customer_name | <code>string</code> Name of the customer.
date | <code>string</code> Date of the sale, in format <code>YYYY-MM-DD</code>
invoice_number | <code>string</code> Invoice number.
currency_code | <code>string</code> Currency code as per [ISO 4217](https://www.iban.com/currency-codes).
currency_rate | <code>float</code> Exchange rate if invoice is in a non-base currency. E.g., if your base currency is `USD` and the invoice is in `AED`, the `currency_rate` would be `3.6725`.
total_amount | <code>float</code> Total invoice amount including taxes.
tax_amount | <code>float</code> Total tax amount.
status | <code>string</code> Invoice status. Must be one of <code>PAID</code>, <code>UNPAID</code>, <code>DELETED</code>, <code>TEST</code>
line_items | <code>JSON array</code> Array of [`Line Item`](#line-items) objects.
payments | <code>JSON array</code> Array of `Payments` objects.



# Line Items

Line items of a Sales object.

### Fields

Parameter | Description
--------- | -----------
description | (optional) <code>string</code> Description of the line item.
quantity | <code>float</code> Quantity.
price | <code>float</code> Unit price.
tax_rate | <code>float</code> The tax rate (`0.05` is 5%).
tax_amount | <code>float</code> The tax amount.
tax_group | <code>string</code> UAE-only. Must be one of <code>SR_DUBAI</code>, <code>SR_ABU_DHABI</code>, <code>SR_FUJAIRAH</code>, <code>SR_SHARJAH</code>, <code>SR_UMM_AL_QUWAIN</code>, <code>SR_RAS_AL_KHAIMAH</code>, <code>SR_AJMAN</code>, <code>ZERO_RATED</code>, <code>EXEMPT</code>

> Line item object:

```json
{
  "description": "Some description",
  "quantity": 1,
  "price": 100,
  "tax_rate": 0.05,
  "tax_amount": 5,
}
```
# Payment

Payments are applied to Sales objects.

### Fields

Parameter | Description
--------- | -----------
reference1 | <code>string</code> Reference of the payment.
reference2 | <code>string</code> Additional reference of the payment.
amount | <code>float</code> Amount paid.
currency_code | <code>string</code> Currency code as per [ISO 4217](https://www.iban.com/currency-codes).
currency_rate | <code>float</code> Exchange rate when payment is received if the payment was made in the non-base currency. E.g., if your base currency is `USD` and the payment was made in `AED`, the `currency_rate` would be `3.6725`.
customer_name | <code>string</code> Name of the customer.
paid_from_account_id | <code>string</code> The id of the account where the payment was received. Contact your account manager to obtain the value.

> Line item object:

```json
{
  "reference": "Some description",
  "amount": 105,
  "currency_code": "AED",
  "currency_rate": 1,
  "date": "2019-03-01",
  "paid_from_account_id": "a12098x098vkmhas0980123098",
}
```
