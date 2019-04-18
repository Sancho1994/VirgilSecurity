# JSON Web Tokens

### What is JWT?
**JWT** _(JSON Web Token)_ is an open, industry standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained method for securely transmitting information between parties as a JSON object. It can be used by a client and a server to authenticate and share information easily. This information can be verified and trusted because it is digitally signed.

JWTs can be signed using a secret (with the _HMAC_ algorithm) or a public/private key pair using _RSA_ or _ECDSA_.

See this the following diagram to figure JWT scheme of work out.

![Diagram](Diagram.png)

### What is JWT structure?
JSON Web Tokens consist of three parts separated by dots ```(.)```, which are:
* Header
* Payload
* Signature

Hence, a JWT typically looks like the following:
`xxxxx.yyyyy.zzzzz`

Let's break down the different parts.

#### Header
The header typically consists of two parts:
1. The type of the token _(JWT)_ ;
1. The signing algorithm being used (such as _HMAC SHA256_ or _RSA_).

For example:
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
The header is then  **Base64Url** encoded to form the first part of the JWT.
#### Payload
The second part of the token is the payload, which contains the claims. Claims are statements about an entity (typically, the user) and additional data.

An example of the payload:

```json
{
  "iss": "7626d4b2-695c-46f8-a8f3-8f53b79c6d85",
  "sub": {
    "company": "Virgil Security",
    "name": "Sasha Kovalchuk"
  },
  "iat": 1516238035
}
```
The payload is then **Base64Url** encoded to form the second part of the JWT.
#### Signature
To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

For instance, if you want to use the **HMAC SHA256** algorithm, the signature will be created in the following way:
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

The following example shows a JWT that has the previous header and payload encoded, and it is signed with a secret:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiI3NjI2ZDRiMi02OTVjLTQ2ZjgtYThmMy04ZjUzYjc5YzZkODUiLCJzdWIiOnsiY29tcGFueSI6IlZpcmdpbCBTZWN1cml0eSIsIm5hbWUiOiJTYXNoYSBLb3ZhbGNodWsifSwiaWF0IjoxNTE2MjM4MDM1fQ.rAaAplaMMl6ysbFrCtcPxAh7Mik2og2eLnKq8rqsgMA
```
### How to implement JSON Web token-based authentication?

#### Server-side
1. Generate a secret and store it in a safe environment;
1. Set JWT generation up. If the login is verified, the server must return a generated JWT to the client. If the login is **not** verified, the server must return an error.
1. Set JWT verification up. If the JWT is verified, the server must satisfy the request. If the JWT is **not** verified, the server must return an error.

#### Client-side
1. Choose a storage for JWTs, received from the server (cookies, local storage, session storage);
1. Write "Get JWT from the storage" function;
1. Pass JWT in “Authorization” header for each server request.
