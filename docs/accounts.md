## Accounts API

The accounts API is available at `https://api.hsreplay.net/v1/account/` and will return the account details for the authenticated user.

### `GET /v1/account/`

Get the authenticated user's account details.

* `id` (int): The users unique id.
* `username` (string): The username the user should recognize.
* `blizzard_accounts` (array of objects): The region-local battletags claimed by this user.
  * `battletag` (string): The user's full battlag (including the #1234-suffix)
  * `region` (int): This battletag's region.
  * `account_hi` (int): An alternate encoding of the player's region, equivalent to the `region` field.
       Note that while account_hi is an integer, it does exceeds the `INT_MAX` for Javascript and possibly also other languages.
  * `account_lo` (int): The user's unique, region-local id as reported by Blizzard.

