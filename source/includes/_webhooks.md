Webhooks
========

> Example JSON Payload for "subscriber.#{hook_name}"

```json
{
  "subscriber": {
    "id": 1,
    "first_name": "John",
    "email_address": "John@example.com",
    "state": "active",
    "created_at": "2018-02-15T19:40:24.913Z",
    "fields": {
      "My Custom Field": "Value"
    }
  }
}

```

> Example JSON Payload for "purchase.#{hook_name}"

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

Webhooks are automations that will receive subscriber data when a subscriber event is triggered, such as when a subscriber completes a sequence.

When a webhook is triggered, a `POST` request will be made to your url with a JSON payload.

Create a webhook
----------------

> Example request: Create a webhook automation to receive subscriber data at `http://example.com/incoming` when a subscriber is activated.

```shell

curl -X POST https://api.convertkit.com/v3/automations/hooks
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",\
           "target_url": "http://example.com/incoming",\
           "event": { "name": "subscriber.subscriber_activate" } }'

```

> Example response


```json
{
  "rule": {
    "id": 1,
    "account_id": 2,
    "event": {
      "name": "subscriber_activate"
    }
  }
}

```

> Example request: Create a webhook automation to receive subscriber data at `http://example.com/incoming` when a sequence is completed.

```shell

curl -X POST https://api.convertkit.com/v3/automations/hooks
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",\
           "target_url": "http://example.com/incoming",\
           "event": { "name": "subscriber.course_complete", "sequence_id": 18" } }'

```

> Example response

```json
{
  "rule": {
    "id": 43,
    "account_id":2,
    "event": {
      "name": "course_complete",
      "sequence_id": 18
    },
    "target_url":"http://example.com/"
  }
}
```

Create a webhook that will be called when a subscriber event occurs.

### Endpoint

POST /v3/automations/hooks

### Required parameters

-   `api_secret` - Your api secret key.
-   `target_url` - The URL that will receive subscriber data when the event is triggered.
-   `event` - JSON object that includes the trigger name and extra information when needed. For example:
    -   `{ "name": "subscriber.subscriber_activate" }`
    -   `{ "name": "purchase.purchase_create" }`
    -   `{ "name": "subscriber.tag_add", "tag_id": 4 }`

These are the available event types:

-   `"subscriber.subscriber_activate"`
-   `"subscriber.subscriber_unsubscribe"`
-   `"subscriber.form_subscribe"`, *required parameter* `:form_id [Integer]`
-   `"subscriber.course_subscribe"`, *required parameter* `:course_id [Integer]`
-   `"subscriber.course_complete"`, *required parameter* `:course_id [Integer]`
-   `"subscriber.link_click"`, *required parameter* `:initiator_value [String]` as a link URL
-   `"subscriber.product_purchase"`, *required parameter* `:product_id [Integer]`
-   `"subscriber.tag_add"`, *required parameter* `:tag_id [Integer]`
-   `"subscriber.tag_remove"`, *required parameter* `:tag_id [Integer]`
-   `"purchase.purchase_create"`


Destroy webhook
---------------

> Example request

```shell
curl -X DELETE https://api.convertkit.com/v3/automations/hooks/<rule_id>
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>" }'
```

> Example response

```json
{
  "success": true
}
```

Deletes a webhook.

### Endpoint

DELETE /v3/automations/hooks/#{rule_id}

### Required parameters

-   `api_secret` - Your api secret key.
