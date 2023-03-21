# Working with analytics

Below is an overview of the URLs of checkout pages and what they mean. Payers paying on your payment forms will be directed to unique URLs once they enter their initial information.

**1. When a potential payer visits a payment form the URL will look something like this:**

```
app.moonclerk.com/pay/5/3x993hrglt
```

**2. After a potential payer has entered their info on the first page of the checkout, they will be directed here:**

```
app.moonclerk.com/pay/5/3x993hrglt/i/efv7Rb2Hgnb5us2GxmPN4tYW
```

**3. If a potential payer goes back to edit info on the first page they will be directed here:**

```
app.moonclerk.com/pay/5/3x993hrglt/i/efv7Rb2Hgnb5us2GxmPN4tYW/edit
```

**4. Once a payer successfully enters a payment method, they will be directed here:**

```
app.moonclerk.com/pay/5/3x993hrglt/i/efv7Rb2Hgnb5us2GxmPN4tYW/complete
```

### Notes
1. If you're looking for a quick way to see the number of successfull checkouts, look for visits to a URL that ends in `/complete`.
2. The `3x993hrglt` value in the example above will vary between payment forms.
3. The `efv7Rb2Hgnb5us2GxmPN4tYW` value above will vary between payers' checkout sessions.
4. The `/5` portion of the URL will be removed in the future.
