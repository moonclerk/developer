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
  "payment_source": {    
    "type": "card",
    "last4": "4242",
    "exp_month": 12,
    "exp_year": 2018,
    "brand": "Visa",
    "bank_name": "Chase",
  }, 
  "custom_id": "GHS430",
  "charge_reference": "ch_3ohpsF8ra5rqjj",
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
* The `custom_id` field is populated if you use one of our [integration methods](https://github.com/moonclerk/developer/blob/master/integration.md).
* `brand`, `exp_month` and `exp_year` are only present when the payment method used is card
* `bank_name`is only present when the payment method used is ach


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

### Filters

The following list filters are supported on payments.

Parameter     | Description
--------------|------------
`form_id`     | the associated MoonClerk form ID (/forms/123)
`customer_id` | the associated MoonClerk customer ID (/plans/123)
`date_from`   | payments charged on or after this date
`date_to`     | payments charged on or before this date
`status`      | valid options are: successful, refunded, or failed

#### Notes

* All date parameters should be in the form of "YYYY-MM-DD"
* `date_from` parameter will start at the beginning of the day
* `date_to` will go through the end of that day
* All times are UTC based
* You can combine any of the filter parameters

#### Examples:

* `GET /payments?form_id=5346`
* `GET /payments?date_from=2014-10-01&date_to=2014-10-31`
* `GET /payments?customer_id=12742&status=active`


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
