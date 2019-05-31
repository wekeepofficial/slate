---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

In order for WeKeep to record your sales, you must expose an API which WeKeep will poll at regular intervals (typically once a day). The format of the request and response for each type of data is specified in this documentation.

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
curl "https://yourcompany.com/wekeep-api/sales?from_date=2019-01-01&to_date=2019-01-31"
  -H "Authorization: PRIVATE_KEY"
```

> Returns an array of Sales objects:

```json
[
  {
    "id": 1,
    "customer_name": "Alpha & Co",
    "place_of_supply": "DUBAI",
    "date": "2019-03-23",
    "invoice_number": "AE00123",
    "currency_code": "AED",
    "amount_total": 105,
    "tax_amount": 5,
    "status": "PAID",
    "description": "",
    "line_items": [
      {
        "description": "Some description",
        "quantity": 1,
        "price": 100,
        "tax_rate": 0.05,
        "tax_amount": 5,
      },
    ],
    "payments": [
      {
        "reference": "ABCD1234",
        "date": "2019-01-01",
        "amount": 105,
        "currency_rate": 1,
        "paid_through_account_id": "xyz123",

      }
    ]
  },
  ...
]
```

This endpoint must return all sales transactions.

### HTTP Request

`GET https://yourcompany.com/wekeep-api/sales`

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
place_of_supply | <code>string</code> Place of supply. Can be any of <code>DUBAI</code>, <code>ABU_DHABI</code>, <code>FUJAIRAH</code>, <code>SHARJAH</code>, <code>UMM_AL_QUWAIN</code>, <code>RAS_AL_KHAIMAH</code>, <code>AJMAN</code>, <code>NON_UAE</code>
date | <code>string</code> Date of the sale, in format <code>YYYY-MM-DD</code>
invoice_number | <code>string</code> Invoice number.
currency_code | <code>string</code> Currency code as per [ISO 4217](https://www.iban.com/currency-codes).
amount_total | <code>float</code> Total invoice amount including taxes.
tax_amount | <code>float</code> Total tax amount.
status | <code>string</code> Invoice status. Must be either of <code>PAID</code>, <code>UNPAID</code>, <code>DELETED</code>, <code>TEST</code>
line_items | <code>JSON array</code> Array of [`Line Item`](#line-items) objects.
payments | <code>JSON array</code> Array of `Payments` objects.
description | <code>(optional) string</code> Description of the sale.



# Line Items

Line items of a Sales object.

### Fields

Parameter | Description
--------- | -----------
id | <code>string</code> Unique id internal to your systems.
customer_name | <code>string</code> Name of the customer.

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
id | <code>string</code> Unique id internal to your systems.
customer_name | <code>string</code> Name of the customer.

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