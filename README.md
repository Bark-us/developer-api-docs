The Bark Partner API
====================

As a Bark Partner, please use the following API to submit messages.


Making a request
----------------

All URLs start with `https://partner.bark.us/api/v1/`. **SSL only**. If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the old URLs.

Please include the `Content-Type` header and the JSON data:

```shell
curl \
-H 'Content-Type: application/json; charset=utf-8' \
-H 'X-Token-Auth: mysecrettoken' \
-d "{ \"message_id\": \"asdf867as6879s\", \"author_id\": \"12345\", \"posted_at\": \"1456114270.11952\", \"message\": \"Sameple message\" }" \
https://partner.bark.us/api/v1/activities
```

Params
-------------

`message_id` - message ID -- A unique ID for the message (required)

`author_id` - ID identifying the sending user (optional)

`posted_at` - string representation of the activity timestamp (in sec.) as a float (required)

`message` - message text (required)


Authentication
--------------

You'll be given an integration token for which you can supply in 2 ways when
communicating with the API:

1. Provide the `X-Token-Auth` header with the value being your integration
   token
2. Include the query string param `token` (ie.
   `https://partner.bark.us/api/v1/activities?token=mysecrettoken`)

No XML, just JSON
-----------------

We only support JSON for serialization of data. Our format is to have no root element and we use snake\_case to describe attribute keys. This means you have to send `Content-Type: application/json; charset=utf-8` when you're POSTing data to Bark.


Handling errors
---------------

If Bark is having trouble, you might see a 5xx error. `500` means the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these cases to retry your request later.


Activities
========

Create activity
--------------

* `POST /activities` will create a new activity.

```json
{
  "message_id": "asdf867as6879s",
  "author_id": "12356643",
  "posted_at": "1456114270.11952",
  "message": "Sameple message"
}
```

This will return `201 Created`, with the response body `{ "success": true }` if the creation was a success.

Responses
=========

In order to prioritize incoming messages, we will notify you when an activity
is problematic through a `POST` webhook to the URL of your choice.

_Note: Not all activities will result in a webhook response -- only those flagged as problematic._

The payload `POST`ed to your webhook URL will be JSON serialized with the
following:

```json
{
  "message_id": "asdf867as6879s",
  "abuse_types: ["drug-related", "cyberbullying"]
}
```
