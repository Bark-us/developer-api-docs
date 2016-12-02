The Bark.us Partner API
=======================

![Bark Logo](https://www.bark.us/bark-logo-sm.png)

Welcome to the Bark.us Partner API. Looking to get messages scored for cyberbullying, profanity and sentiment? You've come to the right
place!

Making a request
----------------

All URLs start with `https://partner.bark.us/api/v1/`. **HTTPS only**. If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the old URLs.

Please include the `Content-Type` header and the JSON data:

```shell
curl -H 'Content-Type: application/json; charset=utf-8' -H 'X-Token-Auth: mysecrettoken' -d "{ \"message\": \"Sample message\" }" https://partner.bark.us/api/v1/messages
```

Throughout this guide we've included "Copy as cURL" examples. If you'd like to try this out in your shell, export the following ENV variable:

``` shell
export TOKEN=[paste token here]
```

Using the same shell session, you should be able to easily copy + paste any
example from the docs.

Authentication
--------------

You'll be given an access token for which you can supply in 2 ways when
communicating with the API:

1. Provide the `X-Token-Auth` header with the value being your integration token
2. Include the query string param `token` (ie. `https://partner.bark.us/api/v1/messages?token=mysecrettoken`)

English Only (for now)
---------

We currently only support English messages. In the future, we will be
supporting additional languages and will update the documentation at that time.

JSON Only
---------

We only support JSON for serialization of data. Our format is to have no root element and we use snake\_case to describe attribute keys. This means you have to send `Content-Type: application/json; charset=utf-8` when you're POSTing data to Bark.

You'll receive a `415 Unsupported Media Type` response code if you attempt to use another content type.

Handling errors
---------------

If Bark is having trouble, you might see a 5xx error. `500` means the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these cases to retry your request later.

A typical error response will include the keys `success` and `error` detailing the error. For instance:

```json
{
  "success": false,
  "error": "Please provide a valid message"
}
```

Rate limiting
-------------

You can perform up to 10 requests per 2 second period from the same IP address. If you exceed this limit,
you'll get a [429 Too Many Requests](http://tools.ietf.org/html/draft-nottingham-http-new-status-02#section-4)
response for subsequent requests.

We recommend baking 429 response handling in to your HTTP handling at a low level so your integration gracefully and automatically handles retries.

API endpoints
-------------
- [Messages](https://github.com/Bark-us/partner-api-docs/blob/master/messages.md)

Support
-------

If you have a specific feature request or if you found a bug, [please open a GitHub issue](https://github.com/Bark-us/partner-api-docs/issues). We encourage forking these docs for local reference, and will happily accept pull request with improvements.

Alternatively, you can reach us via email at <help@bark.us>.

Unofficial Wrappers
-------------------

* [PyBark](https://github.com/metruption/pybark): An unofficial Python Wrapper for the Bark Partner API.