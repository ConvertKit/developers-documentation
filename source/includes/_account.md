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
