# Version Strategy and Support

*Change is the only constant in life. One's ability to adapt to those changes will determine your success in life.*

[Benjamin Franklin](https://en.wikipedia.org/wiki/Benjamin_Franklin)

## Semantic Versioning

*source: https://semver.org*

Given a version number MAJOR.MINOR.PATCH, increment the:
- MAJOR version when you make incompatible API changes.
- MINOR version when you add functionality in a backward-compatible manner.
- PATCH version when you make backward-compatible bug fixes.

## What to Version

It is fundamental to make the distinction between three types of versioning in the SWIFT API world:
- API versioning (what we are describing here).
- API specification versioning that is, the version of the API specification in SwaggerHub.
- API proxy versioning that is, the version of the source code deployed in the API gateway to support the API.

## When to Version

When an incompatible change is made to an API, its MAJOR version number must be incremented (for example, from v1 to v2).

Backward-compatible changes (either adding new functionality or fixing a bug) do not require you to increment the major version of the API.

## How to Version

Increment the MAJOR version number when a "breaking change" is made to the API (either functional breaking change or bug-fix breaking change).

Increment the MINOR version number when a functional "non-breaking change" is made to the API.

Increment the PATCH version number when a bug-fix "non-breaking change" is made to the API.

### Breaking versus Non-Breaking Changes

In general:
- **Breaking changes** change the API contract. The API cannot be used in the same way as it was used before the change. Something was removed or something existing was changed.
- **Non-breaking changes** are changes that do not change the API contract. The API can still be used in the way it was used before from the consumers' perspective. Often something is added, but nothing is removed.

Examples of breaking changes:
- Change the format of a response (for example, from application/xml to application/json).
- Change to the structure of an error response (API consumers usually implement some logic for parsing error responses).
- Change a type in the response (for example, from int to float).
- Change the name of a property (for example, from name to firstName).
- Mandatory resource added to an existing workflow.
- Remove an endpoint/resource from an existing API.
- Remove support for a Content-Type (in requests and/or responses).

Examples of non-breaking changes:
- New properties added to a response (new properties should just be skipped by the existing API consumers).
- New supported Content-Type formats (in requests and/or responses).
- New supported Content-Language formats (in requests and/or responses).
- New link to other resources (HATEOAS).
- Change the technical working that is, the implementation of an API without changing the contract/interface (for example, changing the sort algorithm to improve performance should NOT change the API specification).
- Change API Proxy configuration (for example, rate limiting rules, quota policy, and so on).

### URL Versioning

Because there are no standards for API versioning, SWIFT has decided to adopt the URL versioning approach that is, specifying the API version in the URL.

The version is specified using the vXXX syntax where the version tag is added **after** the service name.

The format of a SWIFT API URL is:

```text
https://.../<service-name>/v<major-version>/<entity-name>.
```

```text
Valid:          https://api.swiftnet.sipn.swift.com/swift-preval/v1/accounts/verification
Invalid:        https://api.swiftnet.sipn.swift.com/v1/swift-preval/accounts/verification
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Should | Must |

When changing the MAJOR version of an API, ensure that **ALL** of the URLs are updated to the new MAJOR version, even if the endpoint behind some of these URLs has not been changed.

```text
v1 URLs:        https://api.swiftrefdata.com/v1/ibans/{iban}/validity
                https://api.swiftrefdata.com/v1/ibans/{iban}/bic
                https://api.swiftrefdata.com/v1/ibans/{iban}

v2 URLs:        https://api.swiftrefdata.com/v2/ibans/{iban}/validity
                https://api.swiftrefdata.com/v2/ibans/{iban}/bic
                https://api.swiftrefdata.com/v2/ibans/{iban}

v3 URLs:        https://api.swiftrefdata.com/v3/ibans/{iban}/validity
                https://api.swiftrefdata.com/v3/ibans/{iban}/bic
                https://api.swiftrefdata.com/v3/ibans/{iban}

Invalid URLs:   https://api.swiftrefdata.com/v1/ibans/{iban}/validity
                https://api.swiftrefdata.com/v2/ibans/{iban}/bic
                https://api.swiftrefdata.com/v1/ibans/{iban}
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Should | Must |

The MINOR version of the API may be specified in the X-API-Minor-Version HTTP response header.

```text
X-API-Minor-Version: v2.3
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Could | Could | Could | Could |

### Versioning and Release Guidelines

In a [recent survey ran by SmartBear](https://smartbear.com/resources/ebooks/the-state-of-api-2019-report), participants have responded that **versioning** is the second-most important challenge in the API world, after standardisation.

This an increase of 34%, compared to a similar survey done in 2016.

An API is a living contract, but it should be controlled.

Every SWIFT service must define a versioning strategy.

This is typically addressed during the initial design of the service.

Failure to provide a clear version-strategy communication may lead to frustrations.

Consumers and providers need to invest effort in migrating to an updated API version.

```text
Convention:     Only support the two latest MAJOR versions of the API and deprecate the older versions.
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Should | Must |

```text
Convention:     Provide at least six months (preferably one year) notice of a new API MAJOR version.
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Should | Must |

```text
Convention:     Never run more than two MAJOR versions of an API at the same time to avoid technical debts and reduce the risk of accidental incompatibilities.
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Should | Must |

```text
Convention:     Document your API version strategy as early as possible and share it with your consumers.
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Must | Must | Should | Must |
