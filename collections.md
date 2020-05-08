# Collections

## Sorting and Filtering

Sometimes, you need to filter and/or sort a collection of resources.

You can do it in a very inefficient way by getting the full collection of resources and doing the filtering and/or sorting on the client side or you can do the filtering and/or sorting on the server side, which is much more efficient in terms of reduced bandwidth cost, reduced latency, improved response time.

The filtering and/or sorting should be done based on some resource attributes (for example, color, region, brand, year, month, day, and so on) which should be added to the query as query parameters.

```text
Convention: 	Sorting and filtering should be done server-side by providing parameters in the query string.

Example(s): 	https://api.example.com/user-management/users?country=BE&age%3e40&sort=lastName
                Returns all users located in Belgium, older than 40, sorted by last name.
```

Some remarks about the previous URL in the example:
- BE is in upper case which seems to violate rule 4.2.5.6 Always use Lowercase Letters in URLs. It is not, because BE is a value of an attribute named country in the /users resource and not the URL of a resource.
- %3e is the URL-escaped value of the greater than symbol.
- lastName is in camel case, which seems to violate rules 4.2.5.4 Use Hyphens to improve Readability of URLs and 4.2.5.6 Always use lower-case letters in URLs. It is not, because lastName is an attribute in the /users resource and not the URL of a resource.

Query parameters must not be used for retrieving a resource by ID. That should always be done by using path parameters: https://api.example.com/user-management/users/{userId}.

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Should | Should | Should | Should |

## Pagination

When making an API call to a resource such as **/users**, which returns all users of a system, the number of results returned can be very large.

To make results easier to handle and to reduce latency and the amount of bandwidth consumed, results should be paginated.

Typically, when a request is made to a resource such as **/users** without these query parameters, an **HTTP 303 - See Other** response should be sent with a **Location** header set to **/users?limit=25,offset=0**, for example.

This will make sure that pagination is always used.

When a paginated response is sent, the representation of the Page resource should contain:
- One **self** key containing the **/users?limit=25,offset=0** URL of this Page resource.
- One **pageOf** key containing the original/full **/users** URL that this Page is a page of.
- One **next** key containing the URL of the next Page, if available.
- One **previous** key containing the URL of the previous Page, if available.
- One **first** key containing the URL to the first Page of this collection.
- One **last** key containing the URL of the last Page of this collection.

The IANA keeps a list of the [link relationships](https://www.iana.org/assignments/link-relations/link-relations.xml).

Following is an example of pagination in action:
```text
Convention:     Use pagination by adding two query parameters to the URL:
                - <limit> to limit the number of items returned (for example, limit=25).
                - <offset> to specify the offset/starting point in the list of items (for example, offset=0).

-----------------------------------------------------------------------------------------------
HTTP REQUEST: 	GET /users HTTP/1.1
                host: usertracker.com
                accept: application/json
-----------------------------------------------------------------------------------------------

-----------------------------------------------------------------------------------------------
HTTP RESPONSE: 	HTTP/1.1 303 See Other
                location: https://usertracker.com/users?limit=25,offset=0
-----------------------------------------------------------------------------------------------

-----------------------------------------------------------------------------------------------
HTTP REQUEST:   GET /users?limit=25,offset=0 HTTP/1.1
                host: usertracker.com
                accept: application/json
-----------------------------------------------------------------------------------------------

-----------------------------------------------------------------------------------------------
HTTP RESPONSE:  HTTP/1.1 200 OK
                content-type: application/json
                content-location: https://usertracker.com/users?limit=25,offset=0
                content-length: 23456
                {
                  "self": "https://usertracker.com/users?limit=25,offset=0",
                  "kind": "Page",
                  "pageOf": "https://usertracker.com/users",
                  "first": "https://usertracker.com/users?limit=25,offset=0",
                  "last": "https://usertracker.com/users?limit=4,offset=825",
                  "next": "https://usertracker.com/users?limit=25,offset=25",
                  "contents": [
                    {
                      "self": "https://usertracker.com/users/12344",
                      "kind": "User",
                      "name": "Steve Jobs",
                      "email": "stevejobs@apple.com"
                    },
                    {
                      "self": "https://usertracker.com/users/12345",
                      "kind": "User",
                      "name": "Tim Cook",
                      "email": "timcook@apple.com"
                    },
                    ...
                    (23 more)
                  ]
                }
-----------------------------------------------------------------------------------------------
```

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Should | Should | Should | Should |
