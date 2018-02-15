Webhooks
========

> Example JSON Payload

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
    -   `{ "name": "subscriber.tag_add", "tag_id": 4 }`

These are the available event types:

-   `"subscriber.form_subscribe"`
-   `"subscriber.course_subscribe"`
-   `"subscriber.course_complete"`
-   `"subscriber.link_click"`
-   `"subscriber.product_purchase"`
-   `"subscriber.subscriber_activate"`
-   `"subscriber.subscriber_unsubscribe"`
-   `"subscriber.tag_add"`
-   `"subscriber.tag_remove"`


Destroy webhook
---------------

> Example request: Delete webhook automation rule with ID #456.

```shell
curl -X DELETE https://api.convertkit.com/v3/automations/hooks/456
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

DELETE /v3/automations/hooks/<rule_id>

### Required parameters

-   `api_secret` - Your api secret key.
