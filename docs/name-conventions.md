# Naming Conventions

Naming conventions are fundamental to harmonise the API consumers' experience.

Companies usually do not have one API, but can have several hundreds or thousands of APIs, with multiple versions of the same API.

Some of the below sections are based on conventions from the [JSON API Specification](https://jsonapi.org).

## Language

APIs must be written in [Plain English](https://en.wikipedia.org/wiki/Plain_English).

This includes:
- URLs (including Path Parameters and Query Parameters)
- HTTP Headers
- HTTP Bodies
- Documentation

This rule does not apply for APIs that make use of international characters (for example, TSS API using Chinese or French vocabulary).

To make sure SWIFT APIs are internationalised, [UTF-8](https://en.wikipedia.org/wiki/UTF-8) encoding should be used.

The default encoding for application/json payloads is UTF-8.

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Should | Must | Must | Must |

## Uniform Resource Locator (URL)

An API is a set of resources uniquely identified by their URLs.

Having a logical structure for URLs helps to organise APIs and to make them easy to understand and easy to consume.

The structure of a URL (for example, https://api.example.com/user-management/users?country=BE) is:
- Protocol (https)
- Resource Name (api.example.com/user-management/users?country=BE)

The structure of a Resource Name (for example, api.example.com/user-management/users?country=BE) is:
- Hostname (api.swift.com)
- Path (/user-management/users)
- Query String (country=BE)

The structure of a Hostname (for example, api.swift.com) is:
- Subdomain (api)
- Domain (swift)
- Top-Level Domain/TLD (com)

### Protocol

HTTPS that is, HTTP over TLS must always be used, because it provides:
- **Confidentiality** of the data exchanged between the Client Application and the API Gateway.
- **Integrity** of the data exchanged between the Client Application and the API Gateway.
- **Authentication** of the API Gateway.

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Must | Must |

### Hostname

The hostname identifies the targeted environment.

#### Public Cloud Environments

| Environment | Hostname |
| --- | --- |
| Production | api.swift.com |
| Developer Portal | sandbox.swift.com |
| Test | swift-dev-dev.apigee.net |

#### Private On-Premises Environments

| Environment | Hostname |
| --- | --- |
| Production | api.swiftnet.sipn.swift.com |
| Test | api-test.swiftnet.sipn.swift.com |

### Path

The path is the unique identifier of a resource.

Paths must contain **NOUNS** and not **VERBS** (for example, /ibans/{iban}/validity is valid, but /get-iban-validity/{iban} is not valid).

There are sometimes exceptions to this rule, and **VERBS** may be used if they make the API's intent **clearer** (for example, /swift-translation-api/v1/translate instead of /swift-translation-api/v1/translation-result).

| Type of Resource | Path |
| --- | --- |
| Collection (for example, all customers) | /customers |
| Single (for example, one specific customer) | /customers/{customerId} |
| Sub-Collection (for example, all accounts of one specific customer) | /customers/{customerId}/accounts |

Your API consumers do not necessarily speak SWIFT's language.

When possible, try to avoid using SWIFT-specific terms.

Following are some sample URLs:
- https://api.example.com/user-management/users/123
- https://api.example.com/user-management/users/123/orders
- https://api.example.com/user-management/users/123/orders/32/prepare

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Must | Must |

### Query String

The query string is usually used for filtering and/or sorting a collection of resources.

Filtering techniques are used to reduce the amount of data returned by the API, which results in:
- Reduced bandwidth consumption
- Reduced latency
- Improved response time

Filtering and sorting are done based on some resource attributes (for example, region, year, month, day, and so on).

For example, https://api.example.com/user-management/users?country=BE&age%3e40&sort=lastName will return all users:
- Located in Belgium
- Whose age is greater than 40
- Sorted by last name

### Best Practices

The following best practices will ensure that all SWIFT APIs are harmonised.

#### Use Nouns to Represent Resources

Resources are "things" that is, nouns, not actions that is, verbs.

Think of resources as being *objects*. They contain *properties* (to describe their *state*) and *methods* (to describe their *behaviour*).

REST APIs are composed of four distinct resource archetypes:
- Document
- Collection
- Store
- Controller

For the moment, only Document and Collection should be used.

If later there is a clear use case for either Store or Controller, SWIFT will update the document accordingly.

#### Use Forward Slash to Indicate a Hierarchical Relationship

The forward slash character is used in the path portion of the URL to indicate a hierarchical relationship between resources.

For example, the "/customers/{customerId}/accounts/{accountId}" URL uniquely identifies the account {accountId} of the customer {customerId}.

#### Do Not Use a Trailing Forward Slash in URLs

As last character of a URL's path, a forward slash does not add any semantic value and causes confusion.

```text
Valid:          https://api.example.com/user-management/users
Invalid:        https://api.example.com/user-management/users/
```

#### Use Hyphens to Improve the Readability of URLs

Use hyphens to split words in order to improve the readability of URLs.

```text
Valid:          https://api.example.com/user-management
Invalid:        https://api.example.com/usermanagement
```

#### Never Use Underscores in URLs

Using underscores instead of hyphens is not recommended.

Depending on the font used, underscores can either become partially obscured or completely hidden, which dramatically reduces the readability of URLs.

```text
Valid:          https://api.example.com/user-management
Invalid:        https://api.example.com/user_management
```

#### Always Use Lower-case Letters in URLs

[RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) defines URLs as case-sensitive except for:
- The scheme (for example, https is the same as HTTPS)
- The host (for example, api.example.com is the same as API.EXAMPLE.COM)

Having a part of the URL being case-sensitive and another part of the same URL being case-insensitive creates confusion.

To avoid confusion, always use lower-case letters in URLs.

```text
Valid:          https://api.example.com/my-folder/my-document
Invalid:        https://api.example.com/myFolder/myDocument
```

#### Never Use File Extensions in URLs

If you want to highlight the media type of a resource in an API, do not use file extensions in URLs, but use the Content-Type HTTP response header instead.

Resources should always be independent of their representations (for example, application/json, application/xml).

```text
Valid:          https://api.example.com/my-folder/my-document
Invalid:        https://api.example.com/my-folder/my-document.json
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Should | Should | Should | Should |

## JSON

Always use [snake case](https://en.wikipedia.org/wiki/Snake_case) for objects and properties.

Business requests and responses use the [ISO 20022 Universal Financial Industry Message Scheme](https://www.iso20022.org/).

```text
Example of an ISO 20022 JSON message:

{
  "activity_report": {
    "related_message_reference": {
      "creation_date_time": "2009-09-09T11:37:00",
      "identification": "ARRMessage24"
    },
    "report": [ {
      "transaction_identification": "01190799181-6940-48",
      "reported_entity": [ {
        "bic": "ADIABE22"
      } ],
      "reported_item": [ {
        "activity": {
          "message_name": "tsmt.011.001.02"
        },
        "initiator": {
          "bic": "SWHQBEBB"
        },
        "date_time": "2009-09-06T08:54:00"
      }, {
        "activity": {
          "message_name": "tsmt.020.001.02"
        },
        "initiator": {
          "bic": "ADIABE22"
        },
        "date_time": "2009-09-06T08:52:00"
      } ]
    } ],
    "report_identification": {
      "creation_date_time": "2009-09-09T11:38:00",
      "identification": "ARPMessage25"
    }
  },
  "@xmlns": "urn:iso:std:iso:20022:tech:xsd:tsmt.002.001.04"
}
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Could | Must | Should | Must |
