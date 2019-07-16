Purchases
=========================

List Purchases
--------------

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
            "transaction_time": "2018-03-17T11:28:04Z",
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
            "transaction_time": "2018-03-17T11:28:04Z",
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

Show all purchases for an account

### Endpoint

    GET /v3/purchases

### Required parameters

-   `api_secret` - Your API secret key.

### Optional parameters

-   `page` - The page of results being requested. Default value is `1`. Each page of results will contain up to 50 purchases.

Retrieve a specific Purchase
----------------------------

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
    "transaction_time": "2018-03-17T11:28:04Z",
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

Show specific purchase by ID

### Endpoint

GET /v3/purchases/#{id}

### Required parameters

-   `api_secret` - Your api secret key.
-   `id` - A purchase ID

Create a Purchase
-----------------

> Example request: get all purchases

```shell
curl -X POST https://api.convertkit.com/v3/purchases \
     -H 'Content-Type: application/json' \
     -d '{ "api_secret": "<your_secret_api_key>",
           "purchase": {
                "transaction_id": "123-abcd-456-efgh",
                "email_address": "john@example.com",
                "first_name": "John",
                "currency": "jpy",
                "transaction_time": "2018-03-17 11:28:04",
                "subtotal": 20.00,
                "tax": 2.00,
                "shipping": 2.00,
                "discount": 3.00,
                "total": 21.00,
                "status": "paid",
                "products": [{
                    "pid": 9999,
                    "lid": 7777,
                    "name": "Floppy Disk (512k)",
                    "sku": "7890-ijkl",
                    "unit_price": 5.00,
                    "quantity": 2
                }, {
                    "pid": 5555,
                    "lid": 7778,
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
    "transaction_time": "2018-03-17T11:28:04Z",
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

### Endpoint

POST /v3/purchases

### Required parameters

-   `api_secret` - Your api secret key.
-   `transaction_id` - A unique ID for the purchase
-   `email_address` - The subscriber that the purchase belongs to
-   `products.pid` - This is your identifier for a product. Each product provided in the 'products' array must have a unique pid. Variants of the same product should have the same pid.
-   `products.lid` - Each product should have an lid that is unique to the product for this purchase. If you have 'line items', lid is where you would put your identifier for each line item.

### Required for third party integrations

Are you building an integration? <a href="mailto:engineers@convertkit.com?subject=Purchases integration">Contact us</a> and we will help you get set up.

-   `purchase.integration` - The name of your integration (i.e. eBay)
-   `integration_key` - An access token for authenticating integrations.

### Optional parameters

-   `first_name` - The first name of the subscriber
-   `subtotal` - The subtotal of the purchase
-   `tax` - Tax applied to purchase
-   `shipping` - Shipping amount applied to purchase
-   `discount` - Discount amount applied to purchase
-   `total` - Total cost of the purchase
-   `currency` - 3 letter currency code, default **USD**
-   `transaction_time` - date and time of purchase as ISO string, default **CURRENT_TIMESTAMP**
-   `status` - We currently support a status of "paid"
-   `products` - Array of purchased products
    -   `name` - Product name
    -   `pid` - Your unique product identifier. Product variants should have the same pid
    -   `lid` - Your identifier for the product in this specific purchase. i.e. A line item identifier
    -   `sku` - Product sku
    -   `unit_price` - Product price
    -   `quantity` - Product quantity

Update a specific Purchase
--------------------------

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
    "transaction_time": "2018-03-17T11:28:04Z",
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

Note: `status` is only changeable field

### Endpoint

PUT /v3/purchases/#{id}

### Required parameters

-   `api_secret` - Your api secret key.
-   `id` - A purchase ID

#### Optional parameters

-   `status` - Status of the purchase, i.e. "paid", "refund", etc.

Build an official integration
--------------------------

Are you building an official integration? Contact us (engineering@convertkit.com) to set up an integration and let us know the name we should use for the integration. I.e. Stripe. We will send you an integration_key. Collect the api_secret for the ConvertKit account and send it, the integration_key, and the rest of the information for the purchase in each request. <a href="https://developers.convertkit.com/#create-a-purchase">https://developers.convertkit.com/#create-a-purchase</a>

When passing product information, it is important to choose a “pid” that is unique for a product but not for a variant. Variants of the same product should have the same “pid”. The “lid” should be your identifier for the line item of the purchase. In the future, this will allow for more fine-grained control over updates. For example, when one item from a purchase is returned this would identify which one.

Currently, the status of a purchase is only recorded and we do not take any action on the status. You should only send us purchases that have been completed and paid.

We suggest importing your transaction history when a user first sets up a connection with your integration. Previous purchases can be sent with a transaction_time in the past.
