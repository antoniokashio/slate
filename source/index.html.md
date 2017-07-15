---
title: KashIO Payments 1.0.0.a

language_tabs:
  - shell
  - javascript
  
toc_footers:
  - <a href='#'>Sign Up for a Developer Key using KCMS</a>
  
includes:
  - errors

search: true
---

# Introduction

Welcome to the KashIO API ! You can use our API to access KashIO Payments API endpoints, which can get information on how to create Invoices, get notified on events, etc.

We have language bindings in Shell, PHP, Javascript and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "//v1/payments"
  -H "Authorization: bearer your_private_api_key"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('your_private_api_key');
```

> Make sure to replace `your_private_api_key` with your API key.

KashIO Uses HTTP Basic authentication. You can register a new KashIO API key at our KCMS.

KashIO  expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: bearer your_private_api_key`

<aside class="notice">
You must replace <code>your_private_api_key</code> with your personal API key.
</aside>

# Invoices

## Get All Invoices

```shell
curl "https://api.kashio.net/v1/payments/invoices/list"
  -H "Authorization: your_private_api_key"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all invoices.

### HTTP Request

`GET https://api.kashio.net/v1/payments/invoices/<id>`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | false | If set to true, the result will also include cats.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific Invoice.


### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

