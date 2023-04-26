# The MoonClerk API

This is a **READ ONLY** REST-style API that uses JSON for serialization and an API key for authentication.

## Making a request

All URLs start with `https://api.moonclerk.com/`. **SSL only**. The API version and the authentication token are both sent in the header.

To make a request for all the payment forms, you'd append the forms index path to the base url to form something like https://api.moonclerk.com/forms. In curl, that looks like:

```shell
curl -H "Authorization: Token token=[API Key]" -H "Accept: application/vnd.moonclerk+json;version=1" https://api.moonclerk.com/forms
```

## Authentication

You authenticate using your API key. Please keep this private. Anyone with this key can access your account data through the API. To generate and obtain an API key, visit: https://app.moonclerk.com/settings/api-key. Once you have that key, you'll pass it in the **Authorization** header as an authorization token.

`Authorization: Token token=[API Key]`

## Versioning

You will pass both the version and the format (JSON only) in the **Accept** header as well. The header should look like this:

`Accept: application/vnd.moonclerk+json;version=1`

## Handling errors

If there is a problem, MoonClerk will render a 5xx error. `500` means that the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these cases to retry your request later.

## Paging

By default all lists will return 10 rows. You can pass the `count` parameter to determine the number of rows to return (from 1 to 100) and the `offset` parameter to determine the starting position of the set.

For example, to get the first 20 customers looks like this:

    https://api.moonclerk.com/customers?count=20

Then to get the next page of 20:

    https://api.moonclerk.com/customers?count=20&offset=20

## Field Conventions

- All dates are serialized in the ISO 8601 format and are shown in the UTC time zone.
- All money fields integers denominated in cents. Any object with amount fields should have a `currency` field as well.
- Any key ending in `_reference` corresponds to a related Stripe ID

## Available API Endpoints

- [Customers](/api/v1/customers.md) (known as "Plans" in the MoonClerk UI)
- [Payments](/api/v1/payments.md)
- [Forms](/api/v1/forms.md)

## API libraries

- [Unofficial PHP Library](https://github.com/JobBrander/moonclerk-php)
- [Unofficial Ruby Gem](https://github.com/TrevorHinesley/moonclerk)

## Throttling

We expect that everyone using our API will be a good internet citizen and not be abusive to our servers. In an effort to avoid server issues we have throttling controls in place. If you hit a throttling limit you will receive a `429 - Too Many Requests` error. If you get this error, please reduce the number and/frequency of calls.

## Help us make it better

Please tell us how we can make the API better. If you have a specific feature request or if you found a bug, please use GitHub issues. Fork these docs and send a pull request with improvements.
