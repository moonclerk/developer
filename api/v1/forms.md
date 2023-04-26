# Forms

## The Form Object

```json
{
  "id": 234456,
  "title": "Monthly Subscription",
  "access_token": "1vqvyh3tco",
  "currency": "USD",
  "payment_volume": 7665,
  "successful_checkout_count": 6,
  "created_at": "2014-04-07T19:45:28Z",
  "updated_at": "2014-07-20T00:52:31Z"
}
```

## List Forms

- `GET /forms` will return all payment forms

`https://api.moonclerk.com/forms`

```jsonc
{
  "forms": [
    {
      "id": 234456,
      "title": "Monthly Subscription"
      // ...
    },
    {
      "id": 234484,
      "title": "Annual Subscription"
      // ...
    }
  ]
}
```

## Get a Form

- `GET /forms/:id` will return the specified payment form.

`https://api.moonclerk.com/forms/234456`

```jsonc
{
  "form": {
    "id": 234456,
    "title": "Monthly Subscription"
    // ...
  }
}
```
