Purchases
=========================

> Example request

```shell
 curl -X POST https://api.convertkit.com/v3/purchases\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",
           "purchase": {
             "transaction_id": "123-abcd-456-efgh",
             "email_address": "crashoverride@hackers.com",
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
> Example response

```shell

Success:

 { message: "Purchase created successfully." }

Failure:

{
  "error": "Error creating purchase",
  "message": "transaction_id can't be blank, Sku can't be blank for product: Floppy Disk (512k)"
 }
```

Manage purchases made by a subscriber.

### Create a Purchase

#### Endpoint

 POST /v3/purchases

#### Required parameters

-   `api_secret` - Your api secret key.
-   `transaction_id` - A unique ID for the purchase
-   `email_address` - The subscriber that the purchase belongs to
-   `products.sku` - Each product provided in the 'products' array must have a unique sku.

#### Optional parameters

-   `subtotal` - The subtotal of the purchase
-   `tax` - Tax applied to purchase
-   `shipping` - Shipping amount applied to purchase
-   `discount` - Discount amount applied to purchase
-   `total` - Total cost of the purchase
-   `status` - Status of the purchase, i.e. "paid", "refund", etc.
-   `products` - Array of purchased products
    -   `name` - Product name
    -   `sku` - Product sku
    -   `unit_price` - Product price
    -   `quantity` - Product quantity
