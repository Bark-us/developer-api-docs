Classify a message
---------------

* `POST /messages` will classify a single message with or without media

**Required parameters**:

* `message` - text content to be classified

**Optional parameters**:

* `media_url` - the public URL of a media file (photo/video) to be analyzed
* `media` - a base64-encoded string (4 MB max) representing the binary data of the media (photo/video) to be analyzed

_Note: `media_url` and `media` should not be used together. Choose one format
or the other when requesting analysis of a message containing media._

This endpoint will return `200 Success` with the JSON representation of the score for the provided message.

###### Example JSON Request

```json
{
  "message":   "This is a bullying message: I hate you",
  "media_url": "https://www.bark.us/bark-logo.png"
}
```

###### Copy as cURL

``` shell
curl -s -H "Content-Type: application/json" -d "{ \"message\": \"I hate you\", \"media_url\": \"https://www.bark.us/bark-logo.png\" }" https://www.bark.us/api/v3/developers/messages?token=$TOKEN
```

###### Example JSON Response

```json
{
    "success": true,
    "abusive": false,
    "guid": "40132b3d-cb93-4337-943b-41e4eb9d536d",
    "results": {
        "media": {
            "abusive": true,
            "score": 0.71
        },
        "cyberbullying": {
            "abusive": false,
            "score": 0.0
        },
        "profanity": {
            "abusive": true,
            "score": 0.89
        },
        "depression": {
            "abusive": false,
            "score": 0.0
        },
        "suicide": {
            "abusive": false,
            "score": 0.0
        },
        "drug_related": {
            "abusive": false,
            "score": 0.0
        },
        "hate_speech": {
            "abusive": false,
            "score": 0.0
        },
        "sexual_content": {
            "abusive": false,
            "score": 0.0
        },
        "violence": {
            "abusive": false,
            "score": 0.0
        }
    }
}
```

- `success` defines the overall success of the request. If there's an error,
    `success` will be `false` and an `error` will accompany the response (see
    [Handling
    Errors](https://github.com/Bark-us/developer-api-docs#handling-errors) for an
    example)
- the root level `abusive` attribute will be `true` if ANY of the classifier
    features are `abusive`
- the `media` key will be present in the `results` if a `media_url` is passed
    in the payload
- the `abusive` boolean in each classifier feature defines whether or not the
    `message` contents were positive for that feature (ie. if `abusive` is
    `true` within the `cyberbullying` block, the message was found to be
    cyberbullying)
