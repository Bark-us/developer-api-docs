# Bark.us Partner API

This is the official documentation for the Bark.us Partner
API.


## Making a request

All URLs start with `https://partner.bark.us/api/v1/`. **SSL only**. If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the old URLs.

Please include the `Content-Type` header and the JSON data:

```shell
curl \
-H 'Content-Type: application/json; charset=utf-8' \
-H 'X-Token-Auth: mysecrettoken' \
-d "{ \"message\": \"Sample message\" }" \
https://partner.bark.us/api/v1/rate
```

### Params

`message` - message text (required)


### Authentication

You'll be given an integration token for which you can supply in 2 ways when
communicating with the API:

1. Provide the `X-Token-Auth` header with the value being your integration
   token
2. Include the query string param `token` (ie.
   `https://partner.bark.us/api/v1/activities?token=mysecrettoken`)

### No XML, just JSON

We only support JSON for serialization of data. Our format is to have no root element and we use snake\_case to describe attribute keys. This means you have to send `Content-Type: application/json; charset=utf-8` when you're POSTing data to Bark.


### Handling errors

If Bark is having trouble, you might see a 5xx error. `500` means the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these cases to retry your request later.


## Rating API

### Rating a message

* `POST /rate` will provide the rating of a single message

```json
{
  "message": "Sample message"
}
```

This will return `200 Success`, with the response body detailed below.

### Responses
