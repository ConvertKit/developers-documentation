Purchases
=========================

List Purchases
--------------

Show all purchases for an account

### Endpoint

    GET /v3/purchases

### Required parameters

-   `api_secret` - Your api secret key.

### Optional parameters

-   `page` - The page of results being requested. Default value is `1`. Each page of results will contain up to 50 purchases.

> Example request: Retrieve all purchases for an account

```shell
curl -X GET https://api.convertkit.com/v3/purchases \
      -H 'Content-Type: application/json' \
      -d '{ "api_secret": "<your_secret_api_key>", "page": 1 }'
```

> Success response:
    `HTTP/1.1 200 OK`

```json
{
    "total_purchases": 2,
    "page": 1,
    "total_pages": 1,
    "purchases": [
        {
            "id": 3,
            "transaction_id": "123-abcd-456-efgh",
            "status": "paid",
            "email_address": "x@example.com",
            "currency": "JPY",
            "transaction_time": "2018-03-20 12:38",
            "subtotal": 20.0,
            "shipping": 2.0,
            "discount": 3.0,
            "tax": 2.0,
            "total": 21.0,
            "products": [
                {
                    "unit_price": 5.0,
                    "quantity": 2,
                    "sku": "7890-ijkl",
                    "name": "Floppy Disk (512k)"
                },
                {
                    "unit_price": 10.0,
                    "quantity": 1,
                    "sku":"mnop-1234",
                    "name":"Telephone Cord (data)"
                }
            ]
        },
        {
            "id": 4,
            "transaction_id": "123-abcd-457-efgh",
            "status": "paid",
            "email_address": "x@example.com",
            "currency": "USD",
            "transaction_time": "2018-03-20 13:38",
            "subtotal": 20.0,
            "shipping": 2.0,
            "discount": 3.0,
            "tax": 2.0,
            "total": 21.0,
            "products": [
                {
                    "unit_price": 5.0,
                    "quantity": 2,
                    "sku": "7890-ijkl",
                    "name": "Floppy Disk (512k)"
                },
                {
                    "unit_price": 10.0,
                    "quantity": 1,
                    "sku": "mnop-1234",
                    "name": "Telephone Cord (data)"
                }
            ]
        }
    ]
}
```

> Failure response. For example, when `api_secret` is not provided:
    `HTTP/1.1 401 Unauthorized`

```json
{
    "error":"Authorization Failed",
    "message":"You do not have sufficient permissions to access this resource"
}
```

Retrieve a specific Purchase
----------------------------

### Endpoint

GET /v3/purchases/#{id}

### Required parameters

-   `api_secret` - Your api secret key.
-   `id` - A purchase ID

> Example request: get purchases with ID `8`

```shell
curl -X GET https://api.convertkit.com/v3/purchases/8 \
     -H 'Content-Type: application/json' \
     -d '{ "api_secret": "<your_secret_api_key>" }
```

> Success response:
    `HTTP/1.1 200 OK`

```json
{
    "id": 8,
    "transaction_id": "123-abcd-456-efgh",
    "status": "paid",
    "email_address": "crashoverride@hackers.com",
    "currency": "JPY",
    "transaction_time": "2018-03-20 14:31",
    "subtotal": 20.0,
    "shipping": 2.0,
    "discount": 3.0,
    "tax": 2.0,
    "total": 21.0,
    "products": [
        {
            "unit_price": 5.0,
            "quantity": 2,
            "sku": "7890-ijkl",
            "name": "Floppy Disk (512k)"
        },
        {
            "unit_price": 10.0,
            "quantity": 1,
            "sku": "mnop-1234",
            "name": "Telephone Cord (data)"
        }
    ]
}
```

> Failure response. For example, when `api_secret` is not provided:
    `HTTP/1.1 401 Unauthorized`

```json
{
    "error":"Authorization Failed",
    "message":"You do not have sufficient permissions to access this resource"
}
```

Create a Purchase
-----------------

### Endpoint

POST /v3/purchases

### Required parameters

-   `api_secret` - Your api secret key.
-   `transaction_id` - A unique ID for the purchase
-   `email_address` - The subscriber that the purchase belongs to
-   `products.sku` - Each product provided in the 'products' array must have a unique sku.

### Optional parameters

-   `subtotal` - The subtotal of the purchase
-   `tax` - Tax applied to purchase
-   `shipping` - Shipping amount applied to purchase
-   `discount` - Discount amount applied to purchase
-   `total` - Total cost of the purchase
-   `currency` - 3 letter currency code, default **USD**
-   `transaction_time` - date and time of purchase as ISO string, default **CURRENT_TIMESTAMP**
-   `status` - Status of the purchase, i.e. "paid", "refund", etc.
-   `products` - Array of purchased products
    -   `name` - Product name
    -   `sku` - Product sku
    -   `unit_price` - Product price
    -   `quantity` - Product quantity

> Example request: get all purchases

```shell
curl -X POST https://api.convertkit.com/v3/purchases \
     -H 'Content-Type: application/json' \
     -d '{ "api_secret": "<your_secret_api_key>",
           "purchase": {
                "transaction_id": "123-abcd-456-efgh",
                "email_address": "crashoverride@hackers.com",
                "currency": "jpy",
                "transaction_time": "2018-03-20 14:31:10",
                "subtotal": 20.00,
                "tax": 2.00,
                "shipping": 2.00,
                "discount": 3.00,
                "total": 21.00,
                "status": "paid",
                "products": [{
                    "name": "Floppy Disk (512k)",
                    "sku": "7890-ijkl",
                    "unit_price": 5.00,
                    "quantity": 2
                }, {
                    "name": "Telephone Cord (data)",
                    "sku": "mnop-1234",
                    "unit_price": 10.00,
                    "quantity": 1
                }]
           }
     }'
```

> Success response:
    `HTTP/1.1 201 Created`

```json
{
    "id": 8,
    "transaction_id": "123-abcd-456-efgh",
    "status": "paid",
    "email_address": "crashoverride@hackers.com",
    "currency": "JPY",
    "transaction_time": "2018-03-20 14:31",
    "subtotal": 20.0,
    "discount": 3.0,
    "tax": 2.0,
    "shipping": 2.00,
    "total": 21.0,
    "products": [
        {
            "unit_price": 5.0,
            "quantity": 2,
            "sku": "7890-ijkl",
            "name": "Floppy Disk (512k)"
        },
        {
            "unit_price": 10.0,
            "quantity": 1,
            "sku": "mnop-1234",
            "name": "Telephone Cord (data)"
        }
    ]
}
```

> Failure response:
    `HTTP/1.1 400 Bad Request`

```json
{
    "error": "Your request is missing parameters",
    "message": "transaction_id can't be blank, Sku can't be blank for product: Floppy Disk (512k)"
}
```

Update a specific Purchase
--------------------------

### Endpoint

PUT /v3/purchases/#{id}

### Required parameters

-   `api_secret` - Your api secret key.
-   `id` - A purchase ID

#### Optional parameters

-   `status` - Status of the purchase, i.e. "paid", "refund", etc.

Note: `status` is only changeable field

> Example request: update purchases with ID `8`

```shell
curl -X PUT https://api.convertkit.com/v3/purchases/8 \
     -H 'Content-Type: application/json' \
     -d '{ "api_secret": "<your_secret_api_key>",
           "purchase": {
                "status": "refund"
           }
        }'

```
> Success response:
    `HTTP/1.1 200 OK`

```json
{
    "id": 8,
    "transaction_id": "123-abcd-456-efgh",
    "status": "refund",
    "email_address": "crashoverride@hackers.com",
    "currency": "JPY",
    "transaction_time": "2018-03-20 14:31",
    "subtotal": 20.0,
    "discount": 3.0,
    "tax": 2.0,
    "shipping": 2.00,
    "total": 21.0,
    "products": [
        {
            "unit_price": 5.0,
            "quantity": 2,
            "sku": "7890-ijkl",
            "name": "Floppy Disk (512k)"
        },
        {
            "unit_price": 10.0,
            "quantity": 1,
            "sku": "mnop-1234",
            "name": "Telephone Cord (data)"
        }
    ]
}
```

> Failure response:
    `HTTP/1.1 400 Bad Request`

```json
{
    "error": "Your request is missing parameters",
    "message": "transaction_id can't be blank, Sku can't be blank for product: Floppy Disk (512k)"
}
```
