# OAuth2

HSReplay.net supports OAuth2 access to the API.

A guide to the basics of OAuth2 is [available here](https://aaronparecki.com/2012/07/29/2/oauth2-simplified#authorization).

We ask that you only use existing battle-tested libraries when using our OAuth2 API. Our users' security is extremely important and authentication APIs are sensitive; we *will revoke* API keys that are not taking this seriously.

**OAuth2 keys are handed out on a case-by-case basis, you may request one by emailing us at contact@hsreplay.net.**


## Managing the API key

You will need a user on HSReplay.net to manage the API key. You'll be able to access the OAuth2 management screen from your account settings, at https://hsreplay.net/oauth2/applications/ where you can obtain your `client_id` and `client_secret`.

From there, you can also set the name, description and homepage of your app - all three will be shown on the authorization screen, so make sure your users can recognize them.

You should also specify at least one redirect URL, which will host the callback endpoint. The first URL will be used as the default one if you do not specify one during authorization; if you do, it will be validated against that list. **HTTPS is required for the callback URL.**

## OAuth2 flow

The authorization URL for HSReplay.net is `https://hsreplay.net/oauth2/authorize/`.

Here are the valid scopes you may request:

- `collection:read`: View the Hearthstone collection
- `collection:write`: Update or delete the Hearthstone collection

Your API key has to be authorized to request the scopes. Make sure to mention which scopes you want access to when requesting your API key.

After obtaining an authorization code, you can exchange it for an `access_token` and a `refresh_token` at `https://hsreplay.net/oauth2/token/`.

The lifetime of the tokens is as follow:

* Authorization code: 10 minutes
* Access token: 10 hours
* Refresh token: Unlimited

You should not and do not need to constantly request new access tokens every 10 hours. You should only do so as needed for new API calls.


### Compromised tokens

If your client secret or your tokens have been compromised, please let us know immediately. We will be able to assist with recovery.

We also allow you to revoke all tokens or reset your client secret from the management page, which you should immediately do should the situation arise. These are destructive actions, make sure to understand their implications!
