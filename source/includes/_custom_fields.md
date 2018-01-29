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
