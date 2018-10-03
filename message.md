Classify a message
---------------

* `POST /message` will classify a single message with or without media

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
curl -s -H "Content-Type: application/json" -d "{ \"message\": \"I hate you\", \"media_url\": \"https://www.bark.us/bark-logo.png\" }" https://www.bark.us/api/v3/developers/message?token=$TOKEN
```

###### Example JSON Response

```json
{
    "success": true,
    "abusive": false,
    "results": {
        "media": {
            "media_type": "image/png",
            "likelihood": {
                "medical": "VERY_UNLIKELY",
                "violence": "UNLIKELY",
                "spoof": "VERY_UNLIKELY",
                "adult": "VERY_UNLIKELY"
            },
            "image_classifier": "Google Vision API",
            "text": "rk\n"
        },
        "sentiment": {
            "polarity": "VERY_NEGATIVE"
        },
        "cyberbullying": {
            "abusive": false,
            "likelihood": "UNLIKELY"
        },
        "profanity": {
            "media_profanity": false,
            "terms": {},
            "severity": 0,
            "context": true,
            "token_count": 3,
            "abusive": false,
            "likelihood": "VERY_UNLIKELY",
            "user_profanity": null
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
- `likelihood` values can be `VERY_UNLIKELY`, `UNLIKELY`,`NEUTRAL`, `LIKELY`, `VERY_LIKELY`
- `polarity` values can be `VERY_NEGATIVE`, `NEGATIVE`, `NEUTRAL`, `POSITIVE`, `VERY_POSITIVE`
- `severity` in the `profanity` block is an `integer` from `0` (no profanity)
    to `4` (severe profanity)
- `context` in the `profanity` block is whether the profanity found requires context to be considered profanity
- the response above illustrates the default classifiers. Additional classifiers (e.g. `sexual_content`, `depression`, `drug_related`,
    etc.) are available by special request.

