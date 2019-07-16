Broadcasts
==========

A Broadcast is a one-time email blast that can either be sent right away or scheduled for a future time and date.

List broadcasts
---------------

> Example Request

```shell

curl https://api.convertkit.com/v3/broadcasts?api_secret=<your_secret_api_key>
```

> Example Response

```json

{
  "broadcasts": [
    {
      "id": 1,
      "created_at": "2014-02-13T21:45:16.000Z",
      "subject": "Welcome to my Newsletter!"
    },
    {
      "id": 2,
      "created_at": "2014-02-20T11:40:11.000Z",
      "subject": "Check out my latest blog posts!"
    },
    {
      "id": 3,
      "created_at": "2014-02-29T08:21:18.000Z",
      "subject": "How to get my free masterclass"
    }
  ]
}
```

Returns a list of all the broadcasts for your account.

### Endpoint

GET /v3/broadcasts

### Required parameters

-   `api_secret` - Your API secret key.

Get stats
---------

Get the stats (recipient count, open rate, click rate, unsubscribe count, total clicks, status, send progress) from a specific broadcast.

### Endpoint

GET /v3/broadcasts/#{broadcast_id}/stats

> Example Request

```shell

curl https://api.convertkit.com/v3/broadcasts/<broadcast_id>/stats?api_secret=<your_secret_api_key>
```

> Example Response

```json

{
  "broadcast": [
    {
      "id":1,
      "stats":
      {
        "recipients": 82,
        "open_rate": 60.97560975609756,
        "click_rate": 23.170731707317074,
        "unsubscribes": 9,
        "total_clicks": 15,
        "show_total_clicks": false,
        "status": "completed",
        "progress": 100.0
      }
    }
  ]
}
```

### Required parameters

-   `api_secret` - Your account API secret key.
-   `broadcast_id` - the id number of the broadcast you want to target
