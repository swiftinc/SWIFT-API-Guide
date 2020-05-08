# HTTP Headers

HTTP requests and HTTP responses have an HTTP header section.

HTTP headers enable HTTP clients and HTTP servers to communicate with each other (for example, allow a server to inform a client that it needs to provide authentication details to access a protected resource).

Header field names are [case-insensitive](https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html#sec4.2).

Apart from that there are no clear rules, standards or RFCs define naming conventions for HTTP headers.

Either use what the majority of the industry is using, or adhere to PascalCase (also known as Upper Camel Case) naming for standard and non-standard HTTP header field names.

```text
Convention: 	Use PascalCase for HTTP headers.

Example(s): 	Accept
                Content-Type
                Authorization
```

The most common HTTP headers are described below.

Detailed information about HTTP headers can be found in [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Should | Should | Should | Should |

## Content-Type versus Accept

Consumers should always send an Accept header to tell the API which format/representation they prefer to receive in the response that is, it drives the Content-Type of the response.

Consumers should send a Content-Type header when the request includes an HTTP body (to specify the format/representation of the request).

Providers should send a Content-Type header when the response includes an HTTP body (to specify the format/representation of the response).

Standard media type names (such as [MIME types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)) should be used.

For JSON, it should be "application/json" with [UTF-8](https://en.wikipedia.org/wiki/UTF-8) as the default encoding.

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Could | Must | Should | Must |

## Authorization

Most APIs require an authenticated user for security/auditing/analytics/monetising purposes.

Consumers must authenticate by providing their credentials and requesting an OAuth2 access token from the **/token** endpoint.

The acquired OAuth2 access token can then be used to authenticate and authorise the user to the other SWIFT APIs.

The OAuth2 access token must be sent by setting a "Bearer token" in the "Authorization" header:

```text
Authorization: Bearer <OAuthAccessToken>
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Could | Must | Must | Must |

## Host

The Host request header specifies the domain name of the server for [virtual hosting](https://en.wikipedia.org/wiki/Virtual_hosting) and, optionally, the TCP port number on which the server is listening.

If no port is given, the default port for the service requested is implied (for example, 443 for HTTPS).

A Host header field **must** be sent in **all** HTTP/1.1 request messages.

An HTTP 400 Bad Request status code will be sent to any HTTP/1.1 request message that lacks a Host header field or that contains more than one.

## Location

The Location response header indicates the URL to redirect to.

It only provides a meaning when served with a 3xx Redirection or 201 Created status code.

## Custom Headers

Custom headers are prefixed with 'X-', like 'X-Custom-Header'.

```text
Convention: 	Prefix custom headers with 'X-'.

Example(s): 	X-Request-ID, X-API-Minor-Version
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Should | Must | Should | Must |

## Trace Identifier

The ability to trace each API call that is, an API request and its associated API response is very important for troubleshooting and support purposes.

### API Call Trace ID for SWIFT

The API Gateway generates a Universally Unique IDentifier (UUID) that is copied in the API request and its associated API response.

This trace identification process is done automatically by the API Gateway and requires no user and/or developer action.

This trace ID is used by SWIFT internally and not by API consumers.

### API Call Trace ID for API Consumers

Using a trace ID enables API consumers to reconcile an API request with its associated API response that is, to unambiguously associate a request with its response.

This can be very useful in high concurrency/parallelism environments where multiple requests and responses can be simultaneously "in-flight".

API consumers can use the *optional* **X-Request-ID** header for this purpose.

This trace identification process works as follows:
1. The API consumer generates a UUID and assigns it to the **X-Request-ID** request header of the API request.
2. The **X-Request-ID** request header is passed unmodified to the back-end server/service that is, the API provider.
3. The back-end server/service copies the value of the **X-Request-ID** from the API request into its associated API response, as an **X-Request-ID** response header.
4. The API consumer receives the API response with an **X-Request-ID** response header containing the UUID of the initial API request, which enables it to correlate the API request with the API response.
