Tags
====

List tags
---------

> Example request

```shell
curl https://api.convertkit.com/v3/tags?api_key=<your_public_api_key>
```

> Example response

```json
{
  "tags": [
    {
      "id": 1,
      "name": "House Stark",
      "created_at": "2016-02-28T08:07:00Z"
    },
    {
      "id": 2,
      "name": "House Lannister",
      "created_at": "2016-02-28T08:07:00Z"
    }
  ]
}
```

Returns a list of tags for the account.

### Endpoint

GET /v3/tags

### Required parameters

-   `api_key` - Your account API key.


Create a tag
------------

> Example request

```shell

Single tag

curl -X POST https://api.convertkit.com/v3/tags
     -H 'Content-Type: application/json'\
     -d '{ "api_key": "<your_public_api_key>",\
           "tag": {\
             "name": "Example Tag"\
           } }'

Multiple tags

curl -X POST https://api.convertkit.com/v3/tags
     -H 'Content-Type: application/json'\
     -d '{ "api_key": "<your_public_api_key>",\
           "tag": [{\
             "name": "Example Tag"\
           }, {\
             "name": "Example Tag 2"\
           }] }'

```

> Example response

```json
{
  "account_id": 1,
  "created_at": "2017-04-12T11:10:32Z",
  "deleted_at": null,
  "id": 1,
  "name": "Example Tag",
  "state": "available",
  "updated_at": "2017-04-12T11:10:32Z"
}

A request to create multiple tags will receive a JSON array with the same type of objects:

[{
  "account_id": 1,
  "created_at": "2017-04-12T11:10:32Z",
  "deleted_at": null,
  "id": 1,
  "name": "Example Tag",
  "state": "available",
  "updated_at": "2017-04-12T11:10:32Z"
},
{
  "account_id": 1,
  "created_at": "2017-04-12T11:11:566Z",
  "deleted_at": null,
  "id": 1,
  "name": "Example Tag 2",
  "state": "available",
  "updated_at": "2017-04-12T11:11:566Z"
}]
```

### Endpoint

POST /v3/tags

### Required parameters

-   `api_key` - Your account API key.
-   `tag` - a JSON object or an array of JSON objects, respectively, that include the tag name
    -   `{ "name": "Example Tag" }`



Tag a subscriber
----------------

> Example request

```shell
curl -X POST https://api.convertkit.com/v3/tags/<tag_id>/subscribe\
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
    "id": 3,
    "state": "inactive",
    "created_at": "2016-02-28T08:07:00Z",
    "source": null,
    "referrer": null,
    "subscribable_id": 1,
    "subscribable_type": "tag",
    "subscriber": {
      "id": 1
    }
  }
}
```


Tags are handled as subscriptions. Subscribe an email address to a tag to have that tag applied to the subscriber with that email address.

### Endpoint

POST /v3/tags/#{tag_id}/subscribe

### Required parameters

-   `api_key` - Your account API key.
-   `email` - Subscriber email address.

### Optional parameters

-   `first_name` - Subscriber first name.
-   `fields` - Object of key/value pairs for custom fields (the custom field must exist before you can use it here).
-   `tags` - Array of tag ids to subscribe to.

### Deprecated parameters
-   `courses` - Array of sequence ids to subscribe to. You should [add the subscriber to the sequence](#add-subscriber-to-a-sequence) directly.
-   `forms` - Array of form ids to subscribe to. You should [add the subscriber to the form](#add-subscriber-to-a-form) directly.
-   `name` - Subscriber first name. You should prefer using `first_name` listed above.


Remove tag from a subscriber
----------------------------

> Example request

```shell
curl -X DELETE https://api.convertkit.com/v3/subscribers/<subscriber_id>/tags/<tag_id>?api_secret=<your_secret_api_key>
```

> Example response

```json
{
  "id": 1,
  "name": "House Stark",
  "created_at": "2016-02-28T08:07:00Z"
}
```

### Endpoint

DELETE /v3/subscribers/#{subscriber_id}/tags/#{tag_id}

### Required parameters

-   `api_secret` - Your api secret key.


Remove tag from a subscriber by email
-------------------------------------

> Example request

```shell
curl -X POST https://api.convertkit.com/v3/tags/<tag_id>/unsubscribe\
    -H "Content-Type: application/json; charset=utf-8"\
    -d '{ \
          "api_secret": "<your_secret_api_key>",\
          "email": "jonsnow@example.com"\
        }'
```

> Example response

```json
{
  "id": 1,
  "name": "House Stark",
  "created_at": "2016-02-28T08:07:00Z"
}
```

### Endpoint

POST /v3/tags/#{tag_id}/unsubscribe

### Required parameters

-   `api_secret` - Your api secret key.
-   `email` - Subscriber email address.


List subscriptions to a tag
---------------------------

> Example request

```shell

curl https://api.convertkit.com/v3/tags/<tag_id>/subscriptions?api_secret=<your_secret_api_key>

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
      "subscribable_type": "tag",
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
      "subscribable_type": "tag",
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

List subscriptions to a tag including subscriber data.

### Endpoint

GET /v3/tags/#{tag_id}/subscriptions

### Required parameters

-   `api_secret` - your api secret key

### Optional parameters

-   `sort_order` - `asc` or `desc`: `asc` to list subscribers added oldest to newest, `desc` to list subscribers added newest to oldest. `asc` is the default order.
-   `subscriber_state` - `active` or `cancelled`: receive only active subscribers or cancelled subscribers
-   `page` - the page to retrieve. 50 results are returned per page.
