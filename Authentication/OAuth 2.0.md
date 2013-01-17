#  OAuth 2.0 Authentication

Harvest provides an OAuth 2.0 based authentication option (detailed in the [OAuth 2.0 Authorization Protocol Draft](http://tools.ietf.org/html/draft-ietf-oauth-v2) and [OAuth 2.0 Bearer Tokens Draft](http://tools.ietf.org/html/draft-ietf-oauth-v2-bearer)). Though still a draft, OAuth 2.0 is already an industry standard way to perform authorization for access to web resources. HTTPS is required for accessing the Harvest API.

To enable OAuth2 support you need to create a key via Manage > Account Settings > Third Party Integrations > Edit Authentication Tokens. This page is accessible to Harvest admins only.

The easiest way to consume Harvest's OAuth 2.0 API is with an OAuth 2.0 client library. OAuth 2.0 is still in the draft stage, and our implementation may differ slightly from other implementations. Please keep this in mind as you build on the OAuth 2.0 interface.

Harvest implements the [OAuth 2.0 Implicit Grant flow](http://tools.ietf.org/html/draft-ietf-oauth-v2-22#section-4.2) for client-side api usage and the [Authorization Code](http://tools.ietf.org/html/draft-ietf-oauth-v2-22#section-4.1) flow for server-side API authorization.

## OAuth 2.0 Implicit Grant flow

The Implicit Grant flow is used for client-side API consumption. Implicit Grant is a bearer token flow. A Harvest user is prompted to authenticate themselves and authorize the client to access their account. Upon success, an access token is return with which the client can make requests on that account's behalf. The token expires in 18 hours, and to fetch a new token the user must again authenticate at Harvest.

*Implicit Grant is only appropriate for client-side API access. Please use the Authorization Code flow for server-side API access.*

To make an authorization request, direct the user to a url including your app's client_id and the proper response_type. An example URL:

    https://api.harvestapp.com/oauth2/authorize ?client_id=NMBEWl3h0r4KKNhfOsmPJw%3D%3D &redirect_uri=https%3A%2F%2Fmyapp.com%2Fsome_path &state=optional-csrf-token &response_type=token

Because this request is against api.harvestapp.com, any Harvest company can authorize access. To limit the ability to authorize access to only one subdomain, just provide that subdomain instead:

    https://onlymycompany.harvestapp.com/oauth2/authorize?client_id=NMB...

Upon authentication and authorization, the user will be redirected back with a URL containing the access token. With an access token, you can make authorized requests against api.harvestapp.com regardless of the user's subdomain.

The URL the user is redirected back to after authorization is based on the redirect_uri parameter you passed initially. An example URL:

    https://myapp.com/some_path?access_token=Ao%2ByCqyGInOKuHVIMkwZGlk%2Fvq9Kt3eDGBpKvZnvWP4latLD6umv2dT76C100YbSABOEwUFqieosQRjNH7qvsA%3D%3D&expires_in=64799&state=optional-csrf-token&token_type=bearer

Parse the access_token from this URL, and then requests can be made with the access token:

    curl 'https://api.harvestapp.com/account/who_am_i?access_token=Jjv5cUAnQx7R9jEECHNRxan7iMprt0ySncJhDdzQbtc%2FQXhMZcNVPQtJuBiDajPqNUz79o7S0FNvWc2WwIDcMA%3D%3D' -H 'Accept: application/json'

And the response:

    {
      "company": {
        "base-uri": "https://subdomain.harvestapp.com"
      },
      "user": {
        "admin": true,
        "timestamp_timers":true,
        "timezone":"Eastern Time (US & Canada)",
        "id":1,
        "email":"jim@smallbizbigplans.com" 
      }
    }

Access tokens are valid for 18 hours, after which the use must again visit the authorization URL to authenticate and authorize access to their account.

## OAuth 2.0 Authorization Code flow

The Authorization Code flow allows server-side applications to make requests on the behalf of a user without their password or username. The user authorizes access by following a URL, and is redirected back to the client application with an authorization code. This code is exchanged on the server-side for an access and refresh token. The access token allows authorized requests to Harvest on that user's behalf for 18 hours, and the refresh token allows new access tokens to be requested for up to a month. If the tokens are lost or destroyed, a fresh code must be requested.

*The Authorization Code flow is only appropriate for server-side API access. Please use the Implicit Grant flow for client-side API access.*

To make an authorization request, direct the user to a url including your app's client_id and the proper response_type. An example URL:

    https://api.harvestapp.com/oauth2/authorize?client_id=NMBEWl3h0r4KKNhfOsmPJw%3D%3D&redirect_uri=https%3A%2F%2Fmyapp.com%2Fsome_path&state=optional-csrf-token&response_type=code

Because this request is against api.harvestapp.com, any Harvest company can authorize access. To limit the ability to authorize access to only one subdomain, just provide that subdomain instead:

    https://onlymycompany.harvestapp.com/oauth2/authorize?client_id=NMB...

Upon authentication and authorization, the user will be redirected back with a URL containing an authorization code. This code must then be exchanged for a refresh and access token using a server-side request and the client's secret key.

The user is redirected back to the redirect_uri parameter you passed after authorization. An example URL:

    https://myapp.com/some_path?code=Ao%2ByCqyGInOKuHVIMkwZGlk%2Nvq9Kt3eDGBpKvZnvWP4latLD6umv2dD76C100YbSABOEwUFqieosQRjNH7qvsA%3D%3D&state=optional-csrf-token

Parse the code from this URL, then make a request for tokens. The request should be a POST request to https://api.harvestapp.com/oauth2/token with the following parameters:

    code:          #{the code returned from the authorization request}
    client_id:     #{your app's client id}
    client_secret: #{your app's secret}
    redirect_uri:  #{the same redirect_uri passed during the authorization request}
    grant_type:    authorization_code

This request will return an access and refresh token for you to make authorized requests with.

    {
      "token_type": "bearer",
      "expires_in": 64799,
      "access_token": "MDt6UyMCrY4h0tAQTxvynShtjddS64fyVcSYge2S7rSUT4vPy9Ny5TWa1sltXS2BjsF+uJgDKof+V2yQwdhI9Q==",
      "refresh_token": "GOk4bZ4bVcP4nqo071H9qxbX+c+UtLa1jMB7q1lpc1Y9/Me9GHlsQr8zm1VNSlS7lgm/DKjXdgFlwgj2WI6zCg=="
    }

The access token can be used for requests as described in the Implicit Grant flow.

Refresh tokens can be used to request new access and refresh tokens with later expiration times. Getting fresh tokens is similar to exchanging your authorization code for tokens. Make a POST request to https://api.harvestapp.com/oauth2/token with the following paramenters:

    refresh_token: #{the current refresh token}
    client_id:     #{your app's client id}
    client_secret: #{your app's secret}
    grant_type:    refresh_token

And the response is the same as when exchanging a code for tokens:

    {
      "token_type": "bearer",
      "expires_in": 64799,
      "access_token": "ZWg6ru2KFzWv/fT9emMRlIvhADN85OWjeIKLdXZwlMZ7YUvgyVjdJZN8f2ydIfJhNhrJPBGvOtxYd3lHkvTWZg==",
      "refresh_token": "dwxM/Cf8E1mkiWtKHQEk7qEW4vXGzUH/JBm5Sra4Bnn6KVcGaqy6D7QipGe3OhelK66lYPnjLFSKc5BMvEVjRw=="
    }

Refresh tokens are valid for 1 month after the date they are issued.