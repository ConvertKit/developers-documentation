Sequences
=========

_NOTE: **Sequences** were formerly referred to as **Courses** API v3 retains the previous naming conventions, but will accept requests to `sequences` as the endpoint as well._

List sequences
--------------

> Example request

```shell

curl https://api.convertkit.com/v3/sequences?api_key=<your_public_api_key>

```

> Example response

```json
{
  "courses": [
    {
      "id": 1,
      "name": "My First Sequence",
      "created_at": "2016-02-28T08:07:00Z"
    },
    {
      "id": 2,
      "name": "My Second Sequence",
      "created_at": "2016-02-28T08:07:00Z"
    }
  ]
}
```

Returns a list of sequences for the account.

### Endpoint

GET /v3/sequences

### Required parameters

-   `api_key` - Your account API key.



Add subscriber to a sequence
----------------------------

> Example request

```shell
curl -X POST https://api.convertkit.com/v3/sequences/<sequence_id>/subscribe\
     -H "Content-Type: application/json; charset=utf-8"\
     -d '{ \
            "api_key": "<your_public_api_key>",\
            "email": "jonsnow@example.com"\
         }'
```
> Example response

```json
{
  "subscription": {
    "id": 2,
    "state": "inactive",
    "created_at": "2016-02-28T08:07:00Z",
    "source": null,
    "referrer": null,
    "subscribable_id": 1,
    "subscribable_type": "course",
    "subscriber": {
      "id": 1
    }
  }
}
```

Subscribe an email address to one of your sequences.

### Endpoint

POST /v3/sequences/#{sequence_id}/subscribe

### Required parameters

-   `api_key` - Your account API key.
-   `email` - Subscriber email address.

### Optional parameters

-   `first_name` - Subscriber first name.
-   `fields` - Object of key/value pairs for custom fields (the custom field must exist before you can use it here).
-   `tags` - Array of tag ids to subscribe to.

### Deprecated parameters
-   `courses` - Array of sequence ids to subscribe to. You should add the subscriber to each course individually.
-   `forms` - Array of form ids to subscribe to. You should [add the subscriber to the form](#add-subscriber-to-a-form) directly.
-   `name` - Subscriber first name. You should prefer using `first_name` listed above.

List subscriptions to a sequence
--------------------------------

> Example request

```shell
curl https://api.convertkit.com/v3/sequences/<sequence_id>/subscriptions?api_secret=<your_secret_api_key>
```

> Example response

```json
{
  "total_subscriptions": 2,
  "page": 1,
  "total_pages": 1,
  "subscriptions": [
    {
      "id": 1,
      "state": "active",
      "created_at": "2016-02-28T08:07:00Z",
      "source": null,
      "referrer": null,
      "subscribable_id": 1,
      "subscribable_type": "course",
      "subscriber": {
        "id": 1,
        "first_name": "Jon",
        "email_address": "jonsnow@example.com",
        "state": "active",
        "created_at": "2016-02-28T08:07:00Z",
        "fields": {
          "last_name": "Snow"
        }
      },
    },
    {
      "id": 2,
      "state": "active",
      "created_at": "2016-02-27T08:07:00Z",
      "source": null,
      "referrer": null,
      "subscribable_id": 1,
      "subscribable_type": "course",
      "subscriber": {
        "id": 2,
        "first_name": "Arya",
        "email_address": "arya@example.com",
        "state": "active",
        "created_at": "2016-02-27T08:07:00Z",
        "fields": {
          "last_name": "Stark"
        }
      },
    }
  ]
}
```


List subscriptions to a sequence including subscriber data.

### Endpoint

GET /v3/sequences/#{sequence_id}/subscriptions

### Required parameters

-   `api_secret` - your api secret key

### Optional parameters

-   `sort_order` - `asc` or `desc`: `asc` to list subscribers added oldest to newest, `desc` to list subscribers added newest to oldest. `asc` is the default order.
-   `subscriber_state`?-?`active`?or??`cancelled`: receive only active subscribers or cancelled subscribers
