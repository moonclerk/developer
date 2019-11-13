# Customers

The customer endpoint is used to obtain a list of customers ("Plans" in the MoonClerk web interface) and their subscription info.

_There is a slight difference in terminology between the MoonClerk web UI and the API. The web UI uses the term "Plan" to refer to the combination of a customer, subscription, and plan. The MoonClerk API is much more in line with the underlying Stripe contructs. In order to query the list of "plans" that one sees in the MoonClerk web UI, you would use the `/customers` endpoint._

## The Customer Object

```json
{
  "id": 523425,
  "account_balance": 0,
  "name": "Ryan Wood",
  "email": "ryan@moonclerk.com",
  "payment_source": {    
    "type": "card",
    "last4": "4242",
    "exp_month": 12,
    "exp_year": 2018,
    "brand": "Visa",
    "bank_name": "Chase",
  },
  "custom_id": "GHS430",
  "customer_reference": "cus_4SOZuEc4cxP5L7",
  "discount": {
    "coupon": {
        "code": "10off",
        "duration": "once",
        "amount_off": 1000,
        "currency": "USD",
        "percent_off": null,
        "duration_in_months": null,
        "max_redemptions": null,
        "redeem_by": null
    },
    "starts_at": "2013-04-12T20:05:37Z",
    "ends_at": "2013-05-12T20:05:37Z"
  },
  "delinquent": false,
  "management_url": "https://app.moonclerk.com/manage/xyz1234567",
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
  "form_id": 101,
  "checkout": {
    "date": "2014-07-23T13:44:12Z",
    "subtotal": 1000,
    "fee": 200,
    "upfront_amount": 500,
    "total": 1700,
    "coupon_code": "10off",
    "coupon_amount": 1000,
    "amount_due": 1700,
    "trial_period_days": null
  },
  "subscription": {
    "id": 98,
    "subscription_reference": "sub_3oLgqlp4MgTZC3",
    "status": "active",
    "start": "2014-07-23T13:44:16Z",
    "first_payment_attempt": "2014-07-23T13:44:16Z",
    "next_payment_attempt": "2014-08-23T13:44:16Z",
    "current_period_start": "2014-07-23T13:44:16Z",
    "current_period_end": "2014-08-23T13:44:16Z",
    "trial_start": null,
    "trial_end": null,
    "trial_period_days": null,
    "expires_at": null,
    "canceled_at": null,
    "ended_at": null,
    "plan": {
      "id": 131,
      "plan_reference": "131",
      "amount": 1200,
      "currency": "USD",
      "interval": "month",
      "interval_count": 1
    }
  }
}
```

Notes:

* `discount` may be null if there is no current discount
* Custom field keys are configured in the *Additional Information* section of the payment form builder
* Any JSON key in the customer object ending in `_reference` corresponds to a related Stripe ID
* The `custom_id` field is populated if you use one of our [integration methods](https://github.com/moonclerk/developer/blob/master/integration.md).
* `brand`, `exp_month` and `exp_year` are only present when the payment method used is card
* `bank_name`is only present when the payment method used is ach

## List Customers

* `GET /customers` will return all customers

`https://api.moonclerk.com/customers`

```json
{
  "customers": [
    {
      "id": 523425,
      "name": "Ryan Wood",
      "email": "ryan@moonclerk.com",
      ...
    },
    {
      "id": 523458,
      "name": "Dodd Caldwell",
      "email": "dodd@moonclerk.com",
      ...
    },
    ...
  ]
}
```

### Filters

The following list filters are supported on customers.


* `form_id`
* `checkout_from`
* `checkout_to`
* `next_payment_from`
* `next_payment_to`
* `status`

Parameter             | Description
-------------------   |------------
`form_id`             | the associated MoonClerk form ID (/forms/123)
`checkout_from`       | customers created on or after this date
`checkout_to`         | customers created on or before this date
`next_payment_from`   | subscription due to bill on or after this date
`next_payment_to`     | subscription due to bill on or before this date
`status`              | valid options are: active, canceled, expired, past_due, pending, or unpaid

#### Notes

* The date parameters should be in the form of "YYYY-MM-DD"
* `*_from` parameter will start at the beginning of the day
* `*_to` will go through the end of that day
* All times are UTC based
* You can combine any of the filter parameters

#### Examples:

* `GET /customers?form_id=5346`
* `GET /customers?checkout_from=2014-10-01`
* `GET /customers?next_payment_from=2014-10-01&next_payment_to=2014-10-31`


## Get a Customer

* `GET /customers/:id` will return the specified customer.

`https://api.moonclerk.com/customers/523425`

```json
{
  customer: {
    "id": 523425,
    "name": "Ryan Wood",
    "email": "ryan@moonclerk.com",
    ...
  }
}
```
