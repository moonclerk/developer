# Working with Google Analytics (v4)

Below is an overview of the URLs of checkout pages and what they mean. Payers paying on your payment forms will be directed to unique URLs once they enter their initial information.

## Checkout Page Reference

### Visiting a checkout page

When a potential payer visits a payment form the URL will look something like this:

```
https://app.moonclerk.com/pay/5/3x993hrglt
```

#### Notes

- When this page is visited we will fire a `checkout_start` event.
- The `3x993hrglt` value in the example above will vary between payment forms and represents the form token.
- The `/5` portion of the URL may be removed in the future.

### Moving to the payment page

After a potential payer has entered their info on the first page of the checkout, they will be directed to a URL similar to:

```
https://app.moonclerk.com/pay/5/3x993hrglt/i/efv7Rb2Hgnb5us2GxmPN4tYW
```

#### Notes

- When this page is visited we will fire a `checkout_add_payment` event.
- The `efv7Rb2Hgnb5us2GxmPN4tYW` value above will vary between payers' checkout sessions.

### Editing the checkout

If a potential payer goes back to edit info on the first page, the URL will be similar to:

```
https://app.moonclerk.com/pay/5/3x993hrglt/i/efv7Rb2Hgnb5us2GxmPN4tYW/edit
```

- No event is fired for this page.

**4. Once a payer successfully enters a payment method, they will be directed here:**

### Completing a checkout

Once a payer successfully enters a payment method and completes a checkout with either a successful (or pending in the case of bank payments), the URL will be similar to:

```
https://app.moonclerk.com/pay/5/3x993hrglt/i/efv7Rb2Hgnb5us2GxmPN4tYW/complete
```

#### Notes

- When this page is visited we will fire a `checkout_complete` event.

## Using MoonClerk Events in GA4

### Tracking events on a form basis

All events will contain the `form_token` parameter which is the form token in the URL. This can be useful to set up a [custom dimension](https://support.google.com/analytics/answer/10075209?hl=en) in the GA4 admin which will let you see conversions on a per form basis.

### Conversions

If you want to see conversions in your GA4 dashboard, you configure `checkout_complete` as a [conversion event](https://support.google.com/analytics/answer/12844695?hl=en).
