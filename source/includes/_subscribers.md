Subscribers
===========

List subscribers
----------------

> Example request

```shell
curl https://api.convertkit.com/v3/subscribers?api_secret=<your_secret_api_key>&from=2016-02-01&to=2015-02-28
```

> Example response

```json
{
  "total_subscribers": 3,
  "page": 1,
  "total_pages": 1,
  "subscribers": [
    {
      "id": 1,
      "first_name": "Jon",
      "email_address": "jonsnow@example.com",
      "state": "active",
      "created_at": "2016-02-28T08:07:00Z",
      "fields": {
        "last_name": "Snow"
      }
    },
    {
      "id": 2,
      "first_name": "Arya",
      "email_address": "aryastark@example.com",
      "state": "active",
      "created_at": "2016-02-28T08:07:00Z",
      "fields": {
        "last_name": "Stark"
      }
    }
  ]
}
```

Returns a list of your subscribers. For unsubscribes only, use the `cancelled_at` value for `sort_field` param (currently the only supported extra sort field). Search subscribers by email address by providing the `email_address` param.

### Endpoint

GET /v3/subscribers

### Required parameters

-   `api_secret` - Your api secret key.

### Optional parameters

-   `page` - Page for paginated results.
-   `from` - Filter subscribers added on or after this date (format yyyy-mm-dd).
-   `to` - Filter subscribers added on or before this date (format yyyy-mm-dd).
-   `updated_from` - Filter subscribers who have been updated after this date (format yyyy-mm-dd)
-   `updated_to` - Filter subscribers who have been updated before this date (format yyyy-mm-dd)
-   `sort_order` - Sort order for results (`asc` or `desc`).
-   `sort_field` - Field to sort by (`cancelled_at`).
-   `email_address` - Search subscribers by email address.


View a single subscriber
------------------------

> Example request

```shell
curl https://api.convertkit.com/v3/subscribers/<subscriber_id>?api_secret=<your_secret_api_key>
```

> Example response

```json
{
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
```

Returns data for a single subscriber

### Endpoint

GET /v3/subscribers/#{subscriber_id}

### Required parameters

-   `api_secret` - Your api secret key.



Update subscriber
-----------------

> Example request

```shell
curl -X PUT https://api.convertkit.com/v3/subscribers/<subscriber_id>\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",\
           "first_name": "Jon",\
           "email_address": "jonsnow@example.com",\
           "fields": {\
             "last_name": "Snow"\
           } }'
```

> Example response: Up to 10 custom fields, status `200`

```json
{
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
```

> Example response: 11 to 125 custom fields, status `202`

```json
{
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
```

Updates the information for a single subscriber.

### Endpoint

PUT /v3/subscribers/#{subscriber_id}

### Required parameters

-   `api_secret` - Your api secret key.

### Optional parameters

-   `first_name` - Updated first name for the subscriber.
-   `email_address` - Updated email address for the subscriber.
-   `fields` - Updated custom fields for your subscriber as object of key/value pairs (the custom field must exist before you can use it here).

_NOTE: The API response returned when updating custom fields is dependent on the number of custom fields in the request, as shown by the examples at right. **A maximum of 125 custom fields are allowed.** Requests that exceed this limit will return a response of_ `413`.


Unsubscribe subscriber
----------------------

> Example request

```shell
curl -x PUT https://api.convertkit.com/v3/unsubscribe
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",\
           "email": "jonsnow@example.com" }'
```

> Example response

```json
{
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
```

Unsubscribe an email address from all your forms and sequences.

### Endpoint

PUT /v3/unsubscribe

### Required parameters

-   `api_secret` - Your api secret key.
-   `email` - Subscriber email address.


List tags for a subscriber
--------------------------

> Example request

```shell
curl https://api.convertkit.com/v3/subscribers/<subscriber_id>/tags?api_key=<your_public_api_key>
```

> Example response

```json
{
  "tags": [
    {
      "id": 1,
      "name": "Email Newsletter",
      "created_at": "2016-06-09T17:54:22Z"
    }
  ]
}
```

Lists all the tags for a subscriber.

### Endpoint

GET /v3/subscribers/#{subscriber_id}/tags

### Required parameters

-   `api_key` - Your account API key.
