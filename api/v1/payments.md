# Payments

## The Payment Object

```json
{
  "id": 1348394,
  "date": "2014-04-08T18:57:26Z",
  "status": "successful",
  "currency": "USD",
  "amount": 1000,
  "fee": 59,
  "amount_refunded": 0,
  "name": "Jim Customer",
  "email": "customer@example.com",
  "card_last4": "4242",
  "card_type": "Visa",
  "card_exp_month": 12,
  "card_exp_year": 2018,
  "charge_reference": "ch_3ohpsF8ra5rqjj",
  "customer_reference": "cus_4SOZuEc4cxP5L7",
  "invoice_reference": "in_1La8pLqS2UnhPZ",
  "custom_fields": {
    "shirt_size": {
      "type": "string",
      "response": "XL"
    },
    "shipping_address": {
      "type": "address",
      "response": {
        "id": 32,
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

* All amounts are in cents
* `fee` is the Stripe Fee
* `customer_reference` will be present if the payment was attached to a Stripe customer (vs. a one time payment). Otherwise, it will be null.
* `invoice_reference` will be present if the payment was generated from a Stripe invoice (vs. a one off payment). Otherwise, it will be null.


## List Payments

* `GET /payments` will return all payments

`https://api.moonclerk.com/payments`

```json
{
  "payments": [
    {
      "id": 1348445,
      "date": "2014-04-08T18:58:35Z",
      "status": "successful",
      ...
    },
    {
      "id": 1348394,
      "date": "2014-04-08T18:57:26Z",
      "status": "successful",
      ...
    },
    ...
  ]
}
```

## List Payments from a Specific Form

* `GET /forms/:form_id/payments` will return all payments for the specified form.

`https://api.moonclerk.com/forms/1234/payments`

```json
{
  "payments": [
    {
      "id": 1348445,
      "date": "2014-04-08T18:58:35Z",
      "status": "successful",
      ...
    },
    {
      "id": 1348394,
      "date": "2014-04-08T18:57:26Z",
      "status": "successful",
      ...
    },
    ...
  ]
}
```

## Get a Payment

* `GET /payments/:id` will return the specified payment.

`https://api.moonclerk.com/payments/1348394`

```json
{
  payment: {
    "id": 1348394,
    "date": "2014-04-08T18:57:26Z",
    "status": "successful",
    ...
  }
}
```
