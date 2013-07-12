#  OAuth 2.0 Authentication

The [OAuth 2.0](http://tools.ietf.org/html/draft-ietf-oauth-v2) standard provides your users with a secure way to access Harvest data without providing sensitive information like usernames and passwords.

### Registering Your Application
You'll need to [register your application](https://platform.harvestapp.com/oauth2_clients) with Harvest before using OAuth 2.0 for authorization. After registering, we'll provide you with credentials that your application can use to communicate with Harvest.

### Access Tokens
Access tokens allow your application to communicate with Harvest on behalf of your users. Each user in Harvest is granted a token that expires after 18 hours. Users also have refresh tokens that can be used to request new access tokens for up to 30 days.

## For Server-Side Applications

Harvest uses the [Authorization Code flow](http://tools.ietf.org/html/draft-ietf-oauth-v2-22#section-4.1) for server-side authorization.

1. *Redirect users to Harvest* to authorize their accounts with your application.

GET `https://api.harvestapp.com/oauth2/authorize ?
    client_id=NMBEWl3h0r4KKNhfOsmPJw%3D%3D &
    redirect_uri=https%3A%2F%2Fyourapp.com%2Fredirect_path &
    state=optional-csrf-token &
    response_type=code`

To limit access to a single Harvest account, you can specify its web address instead of api.harvestapp.com.

2. *Get the authorization code* when Harvest redirects back to your application. Harvest sends it to your redirect URI as a query parameter.

GET `https://yourapp.com/redirect_path ?
    code=Ao%2ByCqyGInOKuHVIMkwZGlk%2Nvq9Kt3eDGBpKvZnvWP4latLD6umv2dD76C100YbSABOEwUFqieosQRjNH7qvsA%3D%3D &
    state=optional-csrf-token`

3. *Request an access token* using the authorization code.

POST `https://api.harvestapp.com/oauth2/token`


```json
{
  code:          [authorization code from Harvest]
  client_id:     [your application's client ID]
  client_secret: [your application's client secret]
  redirect_uri:  [your application's redirect URI]
  grant_type:    authorization_code
}
```
4. *Get the access and refresh tokens* from the response.

```json
{
    "token_type": "bearer",
    "expires_in": 64799,
    "access_token": "MDt6UyMCrY4h0tAQTxvynShtjddS64fyVcSYge2S7rSUT4vPy9Ny5TWa1sltXS2BjsF+uJgDKof+V2yQwdhI9Q==",
    "refresh_token": "GOk4bZ4bVcP4nqo071H9qxbX+c+UtLa1jMB7q1lpc1Y9/Me9GHlsQr8zm1VNSlS7lgm/DKjXdgFlwgj2WI6zCg=="
}
```

5. *Use the access token* to send authorized requests to the Harvest API.

GET `https://api.harvestapp.com/account/who_am_i ?
    access_token=Jjv5cUAnQx7R9jEECHNRxan7iMprt0ySncJhDdzQbtc%2FQXhMZcNVPQtJuBiDajPqNUz79o7S0FNvWc2WwIDcMA%3D%3D`

6. *Request a new access token* after 18 hours using the refresh token within 30 days.

POST `https://api.harvestapp.com/oauth2/token`

```json
{
  refresh_token: [user's refresh token]
  client_id:     [your application's client ID]
  client_secret: [your application's client secret]
  grant_type:    refresh_token
}
```
7. *Get the new tokens* from the response.

```json
{
  "token_type": "bearer",
  "expires_in": 64799,
  "access_token": "ZWg6ru2KFzWv/fT9emMRlIvhADN85OWjeIKLdXZwlMZ7YUvgyVjdJZN8f2ydIfJhNhrJPBGvOtxYd3lHkvTWZg==",
  "refresh_token": "dwxM/Cf8E1mkiWtKHQEk7qEW4vXGzUH/JBm5Sra4Bnn6KVcGaqy6D7QipGe3OhelK66lYPnjLFSKc5BMvEVjRw=="
}
```

## For Client-Side Applications

Harvest uses the [Implicit Grant flow](http://tools.ietf.org/html/draft-ietf-oauth-v2-22#section-4.2) for client-side authorization.

1. *Redirect users to Harvest* to authorize their accounts with your application.

GET `https://api.harvestapp.com/oauth2/authorize ?
    client_id=NMBEWl3h0r4KKNhfOsmPJw%3D%3D &
    redirect_uri=https%3A%2F%2Fyourapp.com%2Fredirect_path &
    state=optional-csrf-token &
    response_type=token`

To limit access to a single Harvest account, you can specify its web address instead of api.harvestapp.com.

2. *Get the access token* when Harvest redirects back to your application. Harvest sends it to your redirect URI as a query parameter.

GET `https://yourapp.com/redirect_path ?
    access_token=Ao%2ByCqyGInOKuHVIMkwZGlk%2Fvq9Kt3eDGBpKvZnvWP4latLD6umv2dT76C100YbSABOEwUFqieosQRjNH7qvsA%3D%3D &
    expires_in=64799 &
    state=optional-csrf-token &
    token_type=bearer`

3. *Use the access token* to send authorized requests to the Harvest API.

GET `https://api.harvestapp.com/account/who_am_i ?
    access_token=Jjv5cUAnQx7R9jEECHNRxan7iMprt0ySncJhDdzQbtc%2FQXhMZcNVPQtJuBiDajPqNUz79o7S0FNvWc2WwIDcMA%3D%3D`