# API Testing

A [Postman](https://www.getpostman.com) collection must be made available with every API.

The Postman collection should at least contain the following:
- Calls for all of the happy paths/flows in the API (for example, create a new resource, retrieve an existing resource, update an existing resource, delete an existing resource).
- Calls to test the North-bound Security that is, HTTP Bearer Authentication.
- Calls to check for invalid/unsupported HTTP Verbs.

| Internal | Private | Partner | Open |
| --- | --- | --- | --- |
| Should | Must | Must | Must |
