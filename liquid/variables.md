# Liquid Variables & Filters for MoonClerk

## Liquid Variable Reference

### Account
- `account.name` -  Your MoonClerk account name
- `account.email` - You MoonClerk account email (may be different from your sign in email)
- `account.currency` - The currency your MoonClerk and Stripe account are using

### Plan (Customer)
- `plan.id` - MoonClerk Plan ID
- `plan.reference` - Stripe Customer ID for this plan
- `plan.url` - a link to this plan on MoonClerk
- `plan.payer_manage_url` - The link for the payer to manage their plan
- `plan.name` - Payer name
- `plan.email` - Payer email
- `plan.payment_source.type` - Method of payment used, i.e. Credit/Debit Card or Bank Account
- `plan.payment_source.last4` - Last four digits of the card or bank account
- `plan.payment_source.brand` - Card Type, i.e. Visa, MasterCard, etc.
- `plan.payment_source.bank_name` - Bank Name, i.e. Chase, Wells Fargo, etc.
- `plan.payment_source.expiration` - The month and year the active card expires
- `plan.discount.coupon` - A description of the currently applied coupon, i.e. "10off ($10.00 off once)". This will only exist if there is an active discount. Otherwise it will be empty.
- `plan.discount.expiration` - The date the active discount expires for this plan. This will only exist if there is an active discount. Otherwise it will be empty.
- `plan.status` - The status of the plan (Active, Not Started, Canceled, etc.)
- `plan.next_payment_attempt` -  The next scheduled payment attempt on this plan
- `plan.start` - The first scheduled payment attempt
- `plan.expires` - The expiration date of the plan, if applicable
- `plan.amount` - The recurring amount in cents
- `plan.amount_description` - The description of the matching form amount option, if applicable
- `plan.interval` - week, month or year
- `plan.interval_count` - 1 or more
- `plan.frequency` - interval phrase, i.e. "every 3 months”
- `plan.custom_fields` - A Hash of custom field responses based on the form. See [Custom Fields](#custom-fields) section.
- `plan.checkout.date` - The date of the checkout
- `plan.checkout.amount_due` - The total amount of the first payment. This is the big number the payer sees when checking out. Amount is in cents.
- `plan.checkout.subtotal` - The amount of the form or the chosen amount if deferred. Amount is in cents.
- `plan.checkout.fee` - The net fee amount based on a fee amount or percentage as configured in your form. Amount is in cents.
- `plan.checkout.upfront_amount` - The one-time upfront amount that is added to the first payment. (Fees are not applied to this amount.) Amount is in cents.
- `plan.checkout.trial_period_days` - The number of trial period days for the checkout, if applicable.

### Payment
- `payment.id` - the MoonClerk internal payment ID
- `payment.reference` - the Stripe charge ID
- `payment.url` - a link to this plan on MoonClerk
- `payment.date` - create date
- `payment.name` - Payer's name
- `payment.email` - Payer's email
- `payment.amount` - the payment amount in cents
- `payment.amount_description` - The description of the matching form amount option, if applicable
- `payment.fee` - the fee in cents
- `payment.amount_refunded` - the amount refunded in cents
- `payment.status` - payment status in the dashboard
- `payment.payment_source.type` - Method of payment used, i.e. Credit/Debit Card or Bank Account
- `payment.payment_source.last4` - Last four digits of the card or bank account
- `payment.payment_source.brand` - Card Type, i.e. Visa, MasterCard, etc.
- `payment.payment_source.bank_name` - Bank Name, i.e. Chase, Wells Fargo, etc.
- `payment.invoice` - the Stripe invoice ID
- `payment.custom_fields` - A Hash of custom field responses based on the form. See [Custom Fields](#custom-fields) section.

### Form
- `form.id`
- `form.url`
- `form.title`
- `form.description`


## Custom Fields

Fields contain the responses from the custom fields created on your
forms. They are accessed through either `payment.custom_fields` or `plan.custom_fields`.

The `custom_fields` variable is a Hash. [See how to loop over a Hash in Liquid](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#for-loops).

The key will be the key you assign during the creation of your custom field in the payment form builder. The value contain the following fields:

- `title` - the title of the field
- `response` - the text response or the address on a single line
- `address` - contains field with the address parts. If the field is not an address it will be empty (null).

If address is not empty, it will contain:

- `address.line1`
- `address.line2`
- `address.city`
- `address.state`
- `address.postal_code`
- `address.county`

Here is a sample of the structure of some custom field data:

```
  'shipping_address' => {
    'title' => 'Shipping Address',
    'address' => {
      'line1' => '123 Sycamore St.',
      'line2' => 'Suite 100',
      'city' => 'Greenville',
      'state' => 'SC',
      'postal_code' => '29601',
      'country' => 'United States'
    },
    'response' => '123 Sycamore St., Suite 100, Greenville, SC 29601, United States'
  },
  'tshirt_size' => {
    'title' => 'T-shirt size',
    'address' => nil,
    'response' => 'X-Large'
  }
```

[Browse examples of how to use custom fields within a template](https://github.com/moonclerk/developer/blob/master/liquid/examples.md#using-custom-fields).

### Deprecated fields
We are maintaining deprecated fields for backward compatibility but all new development should use the newer variables described below.

- `fields` is now a hash called `custom_fields` 
- `plan.card.last4` is now `plan.payment_source.last4`
- `plan.card.type` is now `plan.payment_source.brand`
- `plan.card.expiration` is now `plan.payment_source.expiration`
- `payment.card.last4` is now `payment.payment_source.last4`
- `payment.card.type` is now `payment.payment_source.brand`


## Variable Availability

Which Liquid variables can I use in which email notifications?

> Keep in mind that even though a variable may be sent to your template,
  it may be empty. Outputting an empty variable should not cause an error.


Template                          | payment.*          | plan.*             | account.*         | form.*             | invoice.*
--------------------------------- | :----------------: | :----------------: | :---------------: | :----------------: | :-------:
Successful Payment                | :white_check_mark: | :white_check_mark: | :white_check_mark:| :white_check_mark: |
Recurring Plan Created            |                    | :white_check_mark: | :white_check_mark:| :white_check_mark: |
Failed Payment on Recurring Plan  |                    | :white_check_mark: | :white_check_mark:| :white_check_mark: | :white_check_mark:
Recurring Plan Ended              |                    | :white_check_mark: | :white_check_mark:| :white_check_mark: |
Card Expiration Date Approaching  |                    | :white_check_mark: | :white_check_mark:| :white_check_mark: |
Upcoming Payment                  |                    | :white_check_mark: | :white_check_mark:| :white_check_mark: |


# Liquid Filters

## MoonClerk Filters
- `money` - formats money with a symbol
- `money_with_currency` - formats money with a symbol and currency
- `money_without_currency` - formats money as a decimal

## Official Liquid Docs

Visit the [Official Liquid Wiki](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)
to learn more about the standard filters and control flow constructs (if-then, etc.)
that you can use within templates.
