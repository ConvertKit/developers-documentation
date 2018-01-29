Tags
====

List tags
---------

> Example request

```shell
curl https://api.convertkit.com/v3/tags?api_key=<your_public_api_key>
```

> Example response

```shell
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

#### Endpoint

GET /v3/tags

#### Required parameters

-   `api_key` - Your account API key.


Create a tag
------------

Endpoint
--------

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

```shell
{
  "account_id": 1,
  "created_at": "2017-04-12T11:10:32Z"
  "deleted_at": null,
  "id": 1,
  "name": "Example Tag",
  "state": "available",
  "updated_at": "2017-04-12T11:10:32Z"
}

A request to create multiple tags will receive a JSON array with the same type of objects:

[{
  "account_id": 1,
  "created_at": "2017-04-12T11:10:32Z"
  "deleted_at": null,
  "id": 1,
  "name": "Example Tag",
  "state": "available",
  "updated_at": "2017-04-12T11:10:32Z"
},
{
  "account_id": 1,
  "created_at": "2017-04-12T11:11:566Z"
  "deleted_at": null,
  "id": 1,
  "name": "Example Tag 2",
  "state": "available",
  "updated_at": "2017-04-12T11:11:566Z"
}]
```

POST /v3/tags

#### Required parameters

-   `api_key` - Your account API key.
-   `tag` - a JSON object or an array of JSON objects, respectively, that include the tag name
    -   `{ "name": "Example Tag" }`



Tag a subscriber
----------------

> Example request

```shell
curl -X POST https://api.convertkit.com/v3/tags/1/subscribe\
     -H "Content-Type: application/json; charset=utf-8"\
     -d
```

> Example response

```shell
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
      "id": 1,
      "first_name": "Jon",
      "email_address": "jonsnow@example.com",
      "state": "active",
      "created_at": "2016-02-28T08:07:00Z",
      "fields": {
        "last_name": "Snow"
      }
    }
  }
}
```


Tags are handled as subscriptions. Subscribe an email address to a tag to have that tag applied to the subscriber with that email address.

#### Endpoint

POST /v3/tags/<tag_id>/subscribe

#### Required parameters

-   `api_key` - Your account API key.
-   `email` - Subscriber email address.

#### Optional parameters

-   `first_name` - Subscriber first name.
-   `fields` - Object of key/value pairs for custom fields (the custom field must exist before you can use it here).
-   `name` - Subscriber first name. *legacy behavior, deprecated*
-   `tags` - Comma-separated list of tag ids to subscribe to.
-   `courses` - Comma-separated list of sequence ids to subscribe to. *discouraged*
-   `forms` - Comma-separated list of form ids to subscribe to. *strongly discouraged*



Remove tag from a subscriber
----------------------------

> Example request

```shell
curl -X DELETE https://api.convertkit.com/v3/subscribers/1/tags/1?api_secret=<your_secret_api_key>
```

> Example response

```shell
{
  "id": 1,
  "name": "House Stark",
  "created_at": "2016-02-28T08:07:00Z"
}
```

#### Endpoint

DELETE /v3/subscribers/<subscriber_id>/tags/<tag_id>

#### Required parameters

-   `api_secret` - Your api secret key.


List subscriptions to a tag
---------------------------

> Example request

```shell

curl https://api.convertkit.com/v3/tags/<tag_id>/subscriptions?api_secret=<your_secret_api_key>

```

> Example response

```shell
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

#### Endpoint

GET /v3/tags/<tag_id>/subscriptions

#### Required parameters

-   `api_secret` - your api secret key

#### Optional parameters

-   `sort_order` - `asc` or `desc`: `asc` to list subscribers added oldest to newest, `desc` to list subscribers added newest to oldest. `asc` is the default order.
-   `subscriber_state` - `active` or `cancelled`: receive only active subscribers or cancelled subscribers
