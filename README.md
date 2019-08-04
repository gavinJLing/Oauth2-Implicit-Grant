# Oauth2-Implicit-Grant
A React login screen that adopts Oauth2 - implicit grant flow.

## Oauth2

This version is based on the popular [OAuth2](https://tools.ietf.org/html/rfc6749) secuity protocol. Where Oauth2 supports the following grant models.
- Authorization code grant - [secton 4.1](https://tools.ietf.org/html/rfc6749#section-4.1)
- Implicit grant -[section 4.2](https://tools.ietf.org/html/rfc6749#section-4.2)  
- Resource owner credentials grant - [section 4.3](https://tools.ietf.org/html/rfc6749#section-4.3)
- Client credentials grant - [section 4.4](https://tools.ietf.org/html/rfc6749#section-4.4)
- Refresh/Extension token grant - [section 4.5](https://tools.ietf.org/html/rfc6749#section-4.5)


Quick glossary of [OAuth terms](https://tools.ietf.org/html/rfc6749#section-1.1):

| Core Term | Description |
|:----------:| ------------|
| Resource owner | (a.k.a. the User) - *An entity capable of granting access to a protected resource. When the resource owner is a person, it is referred to as an end-user.* |
| Resource server | (a.k.a. the API server) - *The server hosting the protected resources, capable of accepting and responding to protected resource requests using access tokens.* |
| Client | *An application making protected resource requests on behalf of the resource owner and with its authorization. The term client does not imply any particular implementation characteristics (e.g. whether the application executes on a server, a desktop, or other devices).* |
| Authorization server |  *The server issuing access tokens to the client after successfully authenticating the resource owner and obtaining authorization.* |


## Implementation

This implementation is adopting the '**Implicit grant**' model, within a simple React SPA. Which is similar to the 'Authorization code grant' with two distinct differences.

1) It is intended to be used for user-agent-based clients (e.g. single page web apps) that can’t keep a client secret because all of the application code and storage is easily accessible.
 
2) Instead of the authorization server returning an authorization code which is exchanged for an access token, the authorization server returns an access token.

**The Flow:**

The client will redirect the user to the authorization server with the following parameters in the query string:

- `response_type` with the value `token`
- `client_id` with the client identifier
- `redirect_uri` with the client redirect URI. This parameter is optional, but if not sent the user will be redirected to a pre-registered redirect URI.
- `scope` a space delimited list of scopes
- `state` with a [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) token. This parameter is optional but highly recommended. You should store the value of the CSRF token in the user’s session to be validated when they return.
All of these parameters will be validated by the authorization server.

The user will then be asked to login to the authorization server and approve the client.

If the user approves the client they will be redirected back to the authorization server with the following parameters in the query string:

- `token_type` with the value `Bearer`
- `expires_in` with an integer representing the TTL of the access token
- `access_token` the access token itself
- `state` with the state parameter sent in the original request. You should compare this value with the value stored in the user’s session to ensure the authorization code obtained is in response to requests made by this client rather than another client application.

**Note:** this grant does not return a refresh token because the browser has no means of keeping it priv


