---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://convertkit.com'>Visit ConvertKit.com</a>

includes:

search: true
---

Planning your integration
=========================

Sweet! You're building an integration with ConvertKit. We're thrilled that you want to add-on to our platform. But before you dive in and start slinging code, let's talk about the best way to integrate with ConvertKit.

How you setup your integration depends on what type of provider you are. Let's run through a few examples:

-   **Email Capture.** If your product is mostly for capturing new subscribers then you should integrate with forms in ConvertKit. So in OptinMonster when a subscriber opts-in they are subscriber to a form in ConvertKit.
-   **E-Commerce.** The most important thing for e-commerce customers is the ability to add a tag when a purchase is made. So when SamCart wrote their integration they have the ability to add or remove a subscriber from a tag after a purchase or refund.
-   **Full Integration.** If you want a more full integration with ConvertKit you can add support for forms, courses, and tags.

If you aren't sure how best to structure your integration, just reach out to our team, we'll be happy to help you design it.

Overview
========

Calls for ConvertKit API v3 are relative to the url <https://api.convertkit.com/v3/>

API v3 is in active development. Currently it allows you to:

-   Get a list of forms for an account
-   Add subscriber to a form
-   Add subscriber to an email sequence
-   Create a tag
-   Tag a subscriber
-   Remove tags from a subscriber
-   Add, update, and list custom fields

API Basics
==========

#### API Key

All API calls require the `api_key` parameter. You can find your API Key in the ConvertKit Account page.

#### API Secret

Some API calls require the `api_secret` parameter. All calls that require `api_key` also work with `api_secret`, there's no need to use both. This key grants access to sensitive data and actions on your subscribers, so you should treat it as your password, and do not use it in any client-side code (usually that means JavaScript).

#### Responses

When an API call goes well the API will return a 200 or 201 HTTP response, along with a JSON response body.

If there's some error the API will return an HTTP response in the 400 or 500 range, and a response body indicating what the error was, for example:

`{ "error": "Authorization Failed", "message": "API Key not present" }` with a 401 error.

##### Bad data

When you create or update a field, you may receive an HTTP 422 if any fields contain bad data or required fields are missing.

##### Rate limiting

If your request rate exceeds our limits, you will receive an HTTP status of 429 with the message "Too Many Requests".

-   For the subscribe endpoints for forms, sequences, and tags, the rate is no more than 600 requests over any 5 minute period.

##### Internal server errors

If the server is overloaded or you encounter a bug, you will get a 500 error. Try again after a short period, and if you continue to encounter an error, please raise the issue with support.

Account
=======

Show the current account
------------------------

> Example Request

```shell
curl https://api.convertkit.com/v3/account?api_secret=<your_secret_api_key>
```

> Example response

```shell
{
    "name":"Acme Corp.",
    "primary_email_address":"you@example.com"
}
```

#### Endpoint

GET /v3/account

#### Required parameters

-   `api_secret` - Your account API key.



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



Sequences
=========

*NOTE: *Sequences* were formerly referred to as *Courses* API v3 retains the previous naming conventions, but will accept requests to `sequences` as the endpoint as well.*

List sequences
--------------

> Example request

```shell

curl https://api.convertkit.com/v3/sequences?api_key=<your_public_api_key>

```

> Example response

```shell
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

#### Endpoint

GET /v3/courses

#### Required parameters

-   `api_key` - Your account API key.



Add subscriber to a sequence
----------------------------

> Example request

```shell
curl -X POST https://api.convertkit.com/v3/courses/1/subscribe\
     -H "Content-Type: application/json; charset=utf-8"\
     -d
```
> Example response

```shell
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

Subscribe an email address to one of your sequences.

#### Endpoint

POST /v3/courses/<course_id>/subscribe

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

List subscriptions to a sequence
--------------------------------

> Example request

```shell
curl https://api.convertkit.com/v3/sequences/<sequence_id>/subscriptions?api_secret=<your_secret_api_key>
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

#### Endpoint

GET /v3/sequences/<sequence_id>/subscriptions

#### Required parameters

-   `api_secret` - your api secret key

#### Optional parameters

-   `sort_order` - `asc` or `desc`: `asc` to list subscribers added oldest to newest, `desc` to list subscribers added newest to oldest. `asc` is the default order.
-   `subscriber_state`?-?`active`?or??`cancelled`: receive only active subscribers or cancelled subscribers

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


Subscriber
==========

Subscriber list
===============

> Example request

```shell
curl https://api.convertkit.com/v3/subscribers?api_secret=<your_secret_api_key>&from=2016-02-01&to=2015-02-28
```

> Example response

```shell
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

Returns a list of your subscribers. For unsubscribes only, use the `cancelled_at` value for `sort_field`param (currently the only supported extra sort field). Search subscribers by email address by providing the `email_address` param.

#### Endpoint

GET /v3/subscribers

#### Required parameters

-   `api_secret` - Your api secret key.

#### Optional parameters

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
curl https://api.convertkit.com/v3/subscribers/1?api_secret=<your_secret_api_key>
```

> Example response

```shell
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

#### Endpoint

GET /v3/subscribers/<subscriber_id>

#### Required parameters

-   `api_secret` - Your api secret key.



Update subscriber
-----------------

> Example request

```shell
curl -X PUT https://api.convertkit.com/v3/subscribers/1\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",\
           "first_name": "Jon",\
           "email_address": "jonsnow@example.com",\
           "fields": {\
             "last_name": "Snow"\
           } }'
```

> Example response

```shell
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

#### Endpoint

PUT /v3/subscribers/<subscriber_id>

#### Required parameters

-   `api_secret` - Your api secret key.

#### Optional parameters

-   `first_name` - Updated first name for the subscriber.
-   `email_address` - Updated email address for the subscriber.
-   `fields` - Updated custom fields for your subscriber as object of key/value pairs (the custom field must exist before you can use it here).


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

```shell
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

#### Endpoint

PUT /v3/unsubscribe

#### Required parameters

-   `api_secret` - Your api secret key.
-   `email` - Subscriber email address.


List tags for a subscriber
--------------------------

> Example request

```shell
curl -X GET https://api.convertkit.com/v3/subscribers/1/tags\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>" }'
```

> Example response

```shell
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

#### Endpoint

GET /v3/subscribers/`<subscriber_id>`/tags

#### Required parameters

-   `api_key` - Your account API key.

Webhooks
========

Webhooks are automations that will receive subscriber data when a subscriber event is triggered, such as when a subscriber completes a sequence.

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


```shell
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

```shell
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

#### Endpoint

POST /v3/automations/hooks

#### Required parameters

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

```shell
{
  "success": true
}
```

Deletes a webhook.

#### Endpoint

DELETE /v3/automations/hooks/<rule_id>

#### Required parameters

-   `api_secret` - Your api secret key.


Custom fields
=============

A custom field allows you to collect subscriber information beyond the standard fields of first name and email address. An example would be a custom field called last name so you can get the full names of your subscribers. You create a custom field, and then you're able to use that in your forms or with the API (see the subscribers endpoint for adding custom field values to a subscriber.)

Note that you must create a custom field before you can use it with the subscribe methods on the forms, sequences, and tags endpoints.

List fields
-----------

> Example request

```shell
curl -X GET 'https://api.convertkit.com/v3/custom_fields?api_key=<your_public_api_key>'
```

> Example response

```shell
{
  "custom_fields":
  [
    {
      "id": 1,
      "name": "ck_field_1_last_name",
      "key": "last_name",
      "label": "Last Name"
    },
    {
      "id": 2,
      "name": "ck_field_2_occupation",
      "key": "occupation",
      "label": "Occupation"
    },
  ]
}
```

List all of your account's custom fields.

#### Endpoint

GET /v3/custom_fields

#### Required parameters

-   `api_key` - Your account API key.



Create field
------------

> Example request

```shell

Single label

curl -X POST https://api.convertkit.com/v3/custom_fields\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",
           "label": "Occupation" }'

Multiple labels

curl -X POST https://api.convertkit.com/v3/custom_fields\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",
           "label": ["Occupation", "Location"] }'
```           

> Example response

```shell

Single custom field

{
  "id": 1,
  "name": "ck_field_1_occupation",
  "key": "occupation",
  "label": "Occupation"
}

Multiple custom fields

[{
  "id": 1,
  "name": "ck_field_1_occupation",
  "key": "occupation",
  "label": "Occupation"
},
{
  "id": 2,
  "name": "ck_field_2_occupation",
  "key": "location",
  "label": "Location"
}]
```


Create a custom field for your account. The label field must be unique to your account. Whitespace will be removed from the beginning and the end of your label.

Additionally, a key field and a name field will be generated for you. The key is an ASCII-only, lowercased, underscored representation of your label. This key must be unique to your account. Keys are used in personalization tags in sequences and broadcasts. Names are unique identifiers for use in the HTML of custom forms. They are made up of a combination of ID and the key of the custom field prefixed with "ck_field".

#### Endpoint

POST /v3/custom_fields

#### Required parameters

-   `api_secret` - Your api secret key.
-   `label` or `labels`- The label(s) of the custom field.


Update field
------------

> Example request

```shell
curl -X PUT https://api.convertkit.com/v3/custom_fields/1\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>",
           "label": "Profession" }'
```
> Example response

```shell
No content will be returned.
```

Updates a custom field label (see create field above for more information on labels). Note that the key and the name do not change even when the label is updated.

#### Endpoint

PUT /v3/custom_fields/<your custom field ID>

#### Required parameters

-   `api_secret` - Your api secret key.
-   `id` - The ID of your custom field.
-   `label` - The label of the custom field.


Destroy field
-------------

> Example request

```shell
curl -X DELETE https://api.convertkit.com/v3/custom_fields/1\
     -H 'Content-Type: application/json'\
     -d '{ "api_secret": "<your_secret_api_key>" }'
```

> Example response

```shell
No content will be returned.
```

Destroys a custom field. Note that this will remove all data in this field from your subscribers.

#### Endpoint

DELETE /v3/custom_fields/<your custom field ID>

#### Required parameters

-   `api_secret` - Your api secret key.
-   `id` - The ID of your custom field.


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
