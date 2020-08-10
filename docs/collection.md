## Collection API

The collection API lives on `https://api.hsreplay.net/v1/collection/` and is available for OAuth2 users that request the `collection:read` and `collection:write` scopes, for reading and writing respectively.

### `GET /v1/collection/?account_hi=<hi>&account_lo=<lo>`

Get the collection for an account hi/lo pair. The hi/lo pair must exist and be owned by the authenticated user.

This API respects [ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) cache controls.

### `GET /v1/collection/upload_request/`

Request a collection upload URL for an account hi/lo pair. The following arguments are required:

* `account_hi` (int): The account hi of  the Hearthstone account.
* `account_lo` (int): The account lo of  the Hearthstone account.

The hi/lo pair must exist and be owned by the authenticated user. You can obtain these through the accounts API.

Returns:
- `method`: The HTTP method to use for the collection upload. Will always be PUT.
- `url`: A pre-authenticated HTTP URL to be used for the collection upload.
- `content_type`: The expected content-type of the body of the upload. Will always be `application/json`
- `expires_in`: The time in seconds that the URL authentication is valid for.
- `account_hi`: The specified account_hi.
- `account_hi`: The specified account_lo.

**Usage**: The flow of uploading the collection goes like this:

1. Request an upload URL. This URL is valid for 3 minutes.
2. HTTP PUT to the URL with the collection JSON. The only expected header is `content-type: application/json`. You may gzip the upload.
3. The collection is now available at /v1/collection/.

The upload flow is two-step like the replay upload API in order to defer most of the load to Amazon S3.

#### Expected format of the collection JSON

```js
{
    "collection": {
        "<dbf id>": [int, int],  // key: dbf id of the card; values: normal count, golden count
        ...
    },
    "favorite_heroes": {
        "<playerclass id>": int, // key: playerclass enum of the class; values: dbf id of the favorite hero for that class
        ...
    },
    "cardbacks": [
        int, ... // List of acquired cardbacks. Values are cardback IDs.
    ]
    "favorite_cardback": int,  // favorited cardback id
    "dust": int, // amount of dust on the account
    "gold": int, // amount of gold on the account
}
```
