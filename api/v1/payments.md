# Payments

## The Payment Object

```jsonc
{
  "id": 1348394,
  "date": "2014-04-08T18:57:26Z",
  "status": "successful",
  "currency": "USD",
  "amount": 1000,
  "fee": 59,
  "amount_refunded": 0,
  "amount_description": "Option A",
  "name": "Jim Customer",
  "email": "customer@example.com",
  "payment_method": {
    "type": "card",
    "last4": "4242",
    "brand": "Visa"
  },
  "charge_reference": "ch_3ohpsF8ra5rqjj",
  // customer_id and customer_reference are available only if the payment was from a recurring checkout
  "customer_id": 53453,
  "customer_reference": "cus_4SOZuEc4cxP5L7",
  "invoice_reference": "in_1La8pLqS2UnhPZ",
  "custom_fields": {
    "shirt_size": {
      "id": 23452,
      "type": "string",
      "response": "XL"
    },
    "shipping_address": {
      "id": 23453,
      "type": "address",
      "response": {
        "line1": "123 Main St.",
        "line2": "Ste. 153",
        "city": "Greenville",
        "state": "SC",
        "postal_code": "29651",
        "country": "United States"
      }
    }
  },
  "form_id": 112,
  "custom_id": "GHS430",
  // Checkout data is available only if the payment was from a one-time checkout
  "checkout": {
    "amount_due": 1000,
    "coupon_amount": 1000,
    "coupon_code": "10off",
    "date": "2014-07-23T13:44:12Z",
    "fee": 59,
    "subtotal": 1000,
    "token": "YUAf1PJDe1Uho7CnQ1BVPuCz",
    "total": 1000,
    "trial_period_days": null,
    "upfront_amount": 0
  },
  "coupon": {
    "code": "10off",
    "duration": "once",
    "amount_off": 1000,
    "currency": "USD",
    "percent_off": null,
    "duration_in_months": null,
    "max_redemptions": null,
    "redeem_by": null
  }
}
```

Notes:

- All amounts are in cents
- `fee` is the Stripe Fee
- `customer_reference` will be present if the payment was attached to a Stripe customer (vs. a one time payment). Otherwise, it will be null.
- `invoice_reference` will be present if the payment was generated from a Stripe invoice (vs. a one off payment). Otherwise, it will be null.
- The `custom_id` field is populated if you use one of our [integration methods](/integration.md).

## List Payments

- `GET /payments` will return all payments

`https://api.moonclerk.com/payments`

```jsonc
{
  "payments": [
    {
      "id": 1348445,
      "date": "2014-04-08T18:58:35Z",
      "status": "successful"
      // ...
    },
    {
      "id": 1348394,
      "date": "2014-04-08T18:57:26Z",
      "status": "successful"
      // ...
    }
    // ...
  ]
}
```

### Filters

The following list filters are supported on payments.

| Parameter     | Description                                        |
| ------------- | -------------------------------------------------- |
| `form_id`     | the associated MoonClerk form ID (/forms/123)      |
| `customer_id` | the associated MoonClerk customer ID (/plans/123)  |
| `date_from`   | payments charged on or after this date             |
| `date_to`     | payments charged on or before this date            |
| `status`      | valid options are: successful, refunded, or failed |

#### Notes

- All date parameters should be in the form of "YYYY-MM-DD"
- `date_from` parameter will start at the beginning of the day
- `date_to` will go through the end of that day
- All times are UTC based
- You can combine any of the filter parameters

#### Examples:

- `GET /payments?form_id=5346`
- `GET /payments?date_from=2014-10-01&date_to=2014-10-31`
- `GET /payments?customer_id=12742&status=active`

## Get a Payment

- `GET /payments/:id` will return the specified payment.

`https://api.moonclerk.com/payments/1348394`

```jsonc
{
  "payment": {
    "id": 1348394,
    "date": "2014-04-08T18:57:26Z",
    "status": "successful"
    // ...
  }
}
```
