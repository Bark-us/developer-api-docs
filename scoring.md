Scoring
=======

Endpoints:

- [Score a message](#score-a-message)

Score a message
---------------

* `POST /score` will provide the score for a single message

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
curl -s -H "Content-Type: application/json" -d "{ \"message\": \"I hate you\" }" https://partner.bark.us/api/v1/score?token=$TOKEN
```

###### Example JSON REsponse

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

