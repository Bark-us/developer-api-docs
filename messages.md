Messages
=======

Endpoints:

- [Score a message](#score-a-message)

Score a message
---------------

* `POST /messages` will provide the score for a single message

**Required parameters**:

* `message` - the text content of the message to be scored

This endpoint will return `200 Success` with the JSON representation of the score for the provided message.

###### Example JSON Request

```json
{
  "message": "This is a bullying message: I hate you"
}
```

###### Copy as cURL

``` shell
curl -s -H "Content-Type: application/json" -d "{ \"message\": \"I hate you\" }" https://partner.bark.us/api/v1/messages?token=$TOKEN
```

###### Example JSON REsponse

```json
{
  "success": true,
  "abusive": true,
  "cyberbullying": {
    "abusive": true,
    "likelihood": "VERY_LIKELY",
  },
  "profanity": {
    "abusive": false,
    "context": false,
    "severity": 3,
    "likelihood": "VERY_UNLIKELY",
  },
  "sentiment": {
    "polarity": "NEUTRAL"
  }
}
```

- `success` defines the overall success of the request. If there's an error,
    `success` will be `false` and an `error` will accompany the response (see
    [Handling
    Errors](https://github.com/Bark-us/partner-api-docs#handling-errors) for an
    example)
- the root level `abusive` attribute will be `true` if ANY of the classifier
    features are `abusive`
- the `abusive` boolean in each classifier feature defines whether or not the
    `message` contents were positive for that feature (ie. if `abusive` is
    `true` within the `cyberbullying` block, the message was found to be
    cyberbullying)
- `likelihood` values can be `VERY_UNLIKELY`, `UNLIKELY`,`NEUTRAL`, `LIKELY`, `VERY_LIKELY` 
- `polarity` values can be `VERY_NEGATIVE`, `NEGATIVE`, `NEUTRAL`, `POSITIVE`, `VERY_POSITIVE`
- `severity` in the `profanity` block is an `integer` from `0` (no profanity)
    to `4` (severe profanity)
- `context` in the `profanity` block is whether the profanity found requires context to be considered profanity

