# Security

This chapter describes the security features used by APIs.

## North-bound Security

End-users must always be authenticated and authorised before using APIs.

**Authentication** involves validating end-users' credentials (for example, username and password) to verify their identity. It answers the question *are you who you say you are*.

**Authentication** can be implemented either using a swift.com username and password or Public Key Infrastructure (PKI) identification (for example, using digital certificates).

**Authorisation** is the process to determine whether the authenticated user should have access to the particular resources. It answers the question *are you allowed to do what you want to do*.

**Authorisation** can be implemented using [OAuth 2.0](https://oauth.net/2) and [HTTP Bearer Authentication](https://swagger.io/docs/specification/authentication/bearer-authentication).

See *3.3. Authentication and Authorisation (auth/auth)* and *3.4. Tokens* for more details.

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Must | Must |

## South-bound Security

No one should be allowed to bypass the security of SWIFT's API Gateway (for example, OAuth 2.0 and HTTP Bearer Authentication).

Back-end data and functionalities exposed should only be accessed via SWIFT's API Gateway. Every other connection should be rejected and logged.

To achieve this, implement a [two-way TLS connection](https://en.wikipedia.org/wiki/Mutual_authentication) between SWIFT's API Gateway and each one of the back-end servers.

For *production* and *developer portal* environments, the digital certificate must be signed by a Trusted Certificate Authority (CA) such as SWIFTNet CA, for example.

For *test* environments, the digital certificate can be a [self-signed certificate](https://en.wikipedia.org/wiki/Self-signed_certificate).

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Must | Must |

## Authentication and Authorisation (AuthN/AuthZ)

OAuth 2.0 access tokens are sent as bearer tokens through [HTTP Bearer Authentication](https://swagger.io/docs/specification/authentication/bearer-authentication/).

OAuth 2.0 provides to Client Applications a **Secure Delegated Access** to Resource Servers on behalf of a Resource Owner.

OAuth 2.0 involves **Authorisation**, not **Authentication**.

OAuth 2.0 access tokens should be JSON Web Tokens (JWTs) and should contain the following information:
- "sub" (**subject** claim) containing information about the Resource Owner.
- "cid" (**client** identifier claim) containing information about the Client Application.
- "iss" (**issuer** claim) containing information about the Authorisation Server.
- "aud" (**audience** claim) containing information about the Resource Server.
- "exp" (**expiration time** claim) containing the absolute expiration time (for example, EPOCH/POSIX time).

OAuth 2.0 access tokens are delivered by the **/token** endpoint and can be inactivated by the **/revoke** endpoint.

The mechanism used to acquire an OAuth 2.0 access token varies, depending on the type of API Gateway that is used.

### APIs Exposed through the (Public) Cloud API Gateway

```text
POST /token HTTP/1.1
Host: api.swift.com
Authorization: Basic <base64Encode(<clientId> + ":" + <clientSecret>)>
Content-Type: application/x-www-form-urlencoded

grant_type=password&username=<username>&password=<password>
```

### APIs Exposed through the (Private) On-Premises (SWIFTNet/MV-SIPN) API Gateway

```text
POST /token HTTP/1.1
Host: api.swiftnet.sipn.swift.com
Authorization: Basic <base64Encode(<clientId> + ":" + <clientSecret>)>
Content-Type: application/x-www-form-urlencoded
X-SRCNW: MV_SIPN

grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer&scope=<scope>&assertion=<assertion>
```

## Tokens

There are two types of tokens.

| Token Type | Description |
| --- | --- |
| Opaque | Token is encrypted and consists of a random sequence of alphanumeric characters with no inherent meaning and/or structure. Its contents cannot be viewed by other parties. |
| Transparent | Token is not encrypted, but can be encoded and digitally signed. Its contents can be read by anyone (for example, JSON Web Token/JWT). |

SWIFT currently uses opaque tokens as bearer tokens, but may move to transparent tokens by using [JWT](https://jwt.io).
