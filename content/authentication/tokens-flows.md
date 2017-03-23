---
menu:
  main:
    identifier: tokens-flows
    parent : authentication
    weight: 10
title: Tokens and Flows
---
Before you can start using the Parcel2Go APIs you need to configure an authentication method.  Parcel2Go APIs use the [OAuth2](https://tools.ietf.org/html/rfc6749) protocol for authentication which conforms to the [OpenID Connect](http://openid.net/connect/) specifcation.  There are several types of OAuth 2.0 authentication scenarios, the common ones are detailed below.  There are also several levels of access which are controlled using the [scope parameter](#Scopes) which is specified when requesting access tokens. 

### Client Credentials
You will be assigned a client id and sercret which you can then exchange for an access token using the authorisation endpoint; The access token is then used in the Authorization header when making HTTP calls to the API.


* Clients using this method must be able to maintain a secret (not suitable for client side code).
* Does not provide payment scope for customers account
* Payments can be configured against clients account
* Tokens have an expiry time

This method is ideal when the client will be communication server to server and the payment scope is not required or all payments will be comining from the hosts account.


### Example Token Request 

``` curl
curl --request POST \
  --url https://www.parcel2go.com/auth/connect/token \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'grant_type=client_credentials&scope=public-api&client_id={YOUR CLIENTID}&client_secret={YOUR CLIENT SECRET}'
```

### Example Response 

``` json
{
	"access_token": "abef.....",
	"expires_in": 7200,
	"token_type": "Bearer"
}

```
The access_token here can now be used in HTTP requests to the api by specifying a bearer token header.

``` curl
curl -X GET --header 'Accept: application/json' 'Authorization: Bearer abef..' 'https://www.parcel2go.com/api/services'


```


### Implicit
You will redirect the user to our authorisation endpoint where the user will login and get redirected back to your site.  The access token will be included in the redirect url.


* Provides greater level of access (payment scope)
* Return URLs are part of the authentication so must be registered
* No secret
* Users have to log in
* Users can revoke the access token
* Tokens have a short lived expiry date

This flow is ideal for clients which want to intergrate with a customers parcel2go account and allow the customer to pay for their own orders.

### Example authorize redirect

``` http

GET /auth/connect/authorize
?client_id=example
&redirect_uri=http%3A%2F%2Fwww.example.com%2Freturn 
&response_type=token
&scope=openid+public-api 
HTTP/1.1

Host: www.parcel2go.com

```

### Example authorize return url
``` http
GET /return
?access_token=xxxxxxx...
&token_type=Bearer
&expires_in=7200
HTTP/1.1

Host:www.example.com

```
## Scopes

The following custom scopes are provided in addition to the default OAuth 2.0 scopes.

* public-api - This is basic scope required for any api call
* payment - This scope is required in order to make payments on behalf of a customer using their stored payment methods.

## Requesting a Client Id 
To request access credentials please send an email to [apihelp@parcel2go.com](mailto:apihelp@parcel2go.com) with details on how you wish to use the API and which authentication method above you would like to use.  If you want to use the implicit flow, please give a list of URLS which you would like to use for the return url.