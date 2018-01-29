Forms
=====

List forms
----------

> Example Request

```shell

curl https://api.convertkit.com/v3/forms?api_key=<your_public_api_key>
```

> Example Response

```shell

{
  "forms": [
    {
      "id": 1,
      "name": "A Form",
      "created_at": "2016-02-28T08:07:00Z",
      "type": "embed",
      "url": "https://app.convertkit.com/landing_pages/1",
      "embed_js": "http://api.convertkit.dev/v3/forms/1.js?api_key=<your_public_api_key>",
      "embed_url": "http://api.convertkit.dev/v3/forms/1.html?api_key=<your_public_api_key>",
      "title": "Join the newsletter",
      "description": "Form description text.",
      "sign_up_button_text": "Subscribe",
      "success_message": "Success! Now check your email to confirm your subscription."
    },
    {
      "id": 2,
      "name": "A Landing Page",
      "created_at": "2016-02-28T08:07:00Z",
      "type": "hosted",
      "url": "https://app.convertkit.com/r4ndom_url/TWWDNTHT",
      "embed_js": "http://api.convertkit.dev/v3/forms/2.js?api_key=<your_public_api_key>",
      "embed_url": "http://api.convertkit.dev/v3/forms/2.html?api_key=<your_public_api_key>",
      "title": "Join the newsletter",
      "description": "<p>Landing page description text.</p>",
      "sign_up_button_text": "Subscribe",
      "success_message": "Success! Now check your email to confirm your subscription."
    }
  ]
}
```
Get a list of all the forms for your account.

#### Endpoint

GET /v3/forms

#### Required parameters

-   `api_key` - Your account API key.

Add subscriber to a form
------------------------
> Example Request

```shell

curl -X POST https://api.convertkit.com/v3/forms/1/subscribe\
     -H "Content-Type: application/json; charset=utf-8"\
     -d
```
> Example Response

```shell

{
  "subscription": {
    "id": 1,
    "state": "inactive",
    "created_at": "2016-02-28T08:07:00Z",
    "source": null,
    "referrer": null,
    "subscribable_id": 1,
    "subscribable_type": "form",
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
Subscribe an email address to one of your forms.

#### Endpoint

POST /v3/forms/<form_id>/subscribe

#### Required parameters

-   `api_key` - Your account API key.
-   `email` - Subscriber email address.

#### Optional parameters

-   `first_name` - Subscriber first name.
-   `fields` - Object of key/value pairs for custom fields (the custom field must exist before you can use it here).
-   `name` - Subscriber first name. *legacy behavior, discouraged*
-   `tags` - Comma-separated list of tag ids to subscribe to.
-   `courses` - Comma-separated list of sequence ids to subscribe to. *discouraged*
-   `forms` - Comma-separated list of form ids to subscribe to. *strongly discouraged*


List subscriptions to a form
----------------------------

> Example Request

```shell

curl https://api.convertkit.com/v3/forms/<form_id>/subscriptions?api_secret=<your_secret_api_key>
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
      "subscribable_type": "form",
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
      "subscribable_type": "form",
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

List subscriptions to a form including subscriber data.

#### Endpoint

GET /v3/forms/<form_id>/subscriptions

#### Required parameters

-   `api_secret` - your api secret key

#### Optional parameters

-   `sort_order` - `asc` or `desc`: `asc` to list subscribers added oldest to newest, `desc` to list subscribers added newest to oldest. `asc` is the default order.
-   `subscriber_state` - `active` or `cancelled`: receive only active subscribers or cancelled subscribers
