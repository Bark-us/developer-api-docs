# Bark.us Partner API

![Bark Logo](https://www.bark.us/bark-logo-sm.png)

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
https://partner.bark.us/api/v1/messages
```

### Params

`message` - message text (required)


### Authentication

You'll be given an integration token for which you can supply in 2 ways when
communicating with the API:

1. Provide the `X-Token-Auth` header with the value being your integration
   token
2. Include the query string param `token` (ie.
   `https://partner.bark.us/api/v1/messages?token=mysecrettoken`)

### No XML, just JSON

We only support JSON for serialization of data. Our format is to have no root element and we use snake\_case to describe attribute keys. This means you have to send `Content-Type: application/json; charset=utf-8` when you're POSTing data to Bark.


### Handling errors

If Bark is having trouble, you might see a 5xx error. `500` means the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these cases to retry your request later.

A typical error response will include the keys `success` and `message`
detailing the error. For instance:

```json
{
  "success": false,
  "message": "Please provide a valid message"
}
```

## Messages API

### Analyze a message

* `POST /messages` will provide the rating for a single message

```json
{
  "message": "Sample message"
}
```

This will return `200 Success`, with the response body detailed below.

### Responses

```json
{
  "success": true,
  "abusive": true,
  "cyberbullying": {
    "abusive": true
  },
  "profanity": {
    "abusive": false
  },
  "sentiment": {
    "polarity": "NEUTRAL"
  }
}
```
