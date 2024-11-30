# Integrating With Your Site

We offer two ways to integrate with your site:

1. Seeding the checkout with the data so your user doesn't have to enter it
2. Passing a custom ID in order to track an existing user through a checkout

## Seeding a payment form with data

You can pass parameters to the MoonClerk Payment Form that will make select fields pre-filled. This is a great option if you already have customer information and would like to link to a payment form from an app you control.

### Available Parameters

| Name           | Example value      |
| -------------- | ------------------ |
| `amount_cents` | `1000`             |
| `email`        | `leon@example.com` |
| `name`         | `Leon+Bridges`     |

Note: Payment Form must have an amount that is decided at checkout.

You can set the URL parameters of the form like so:

`https://app.moonclerk.com/pay/8h7frjfytj?amount_cents=1000&name=Leon+Bridges&email=leon@example.com`

Doing so will yield the following result:

![Pre-filled form](assets/pre-filled.png)

[See embedding documentation](/embedding.md) for info on how to accomplish this
with an embedded checkout.

## Tracking a user through a checkout with a custom ID

Sometimes you want to track an existing user on your system through a MoonClerk checkout. Here is the basic flow you will need to follow.

1. Create or retrieve your identifier for the customer
2. Pass the identifier as a custom ID (`cid`) param when the checkout is loaded
3. Set up and configure a webhook endpoint in the MoonClerk dashboard.
4. When a checkout is completed, you will receive a webhook with a payment (for one-off payments) or customer (for recurring plans) payload
5. Compare the `custom_id` field in the payload with the identifier that you passed into the checkout and store the MoonClerk payment ID or customer ID in your database.
6. Then you'll be able to look up a payment using the payment endpoint or a customer using the customer endpoint in the MoonClerk API.

### Passing the custom ID to a checkout

#### When using a linked checkout

If your user ID is 234, you can add the following parameter to your checkout URL:

`https://app.moonclerk.com/pay/8h7frjfytj`

becomes

`https://app.moonclerk.com/pay/8h7frjfytj?cid=234`

**A note about security. All traffic to and from MoonClerk is encrypted with SSL which includes the URL. If you are concerned about sending real IDs over the URL you can create a MD5 or SHA hash based on the id and timestamp, store it with the user in your database, and pass that parameter instead of the actual ID.**

#### When embedding a checkout

If you are using [an embed code for your form](/embedding.md), you'll need to dynamically add the `cid` to the `opts` object. Looking inside the embed code, you'll find an `opts` object similar to this:

`opts={"checkoutToken":"8h7frjfytj","width":"100%"};`

You'll need to add the `cid` as follows:

`opts={"checkoutToken":"8h7frjfytj","width":"100%","cid":"234"};`

Here you can see the entire embed snippet with the updated `opts` object:

```html
<div id="mc8h7frjfytj">
  <a href="https://app.moonclerk.com/pay/8h7frjfytj">My Payment Form</a>
</div>
<script type="text/javascript">
  var mc8h7frjfytj;
  (function (d, t) {
    var s = d.createElement(t),
      opts = { checkoutToken: "8h7frjfytj", width: "100%", cid: "234" };
    s.src = "https://d2l7e0y6ygya2s.cloudfront.net/assets/embed.js";
    s.onload = s.onreadystatechange = function () {
      var rs = this.readyState;
      if (rs) if (rs != "complete") if (rs != "loaded") return;
      try {
        mc8h7frjfytj = new MoonclerkEmbed(opts);
        mc8h7frjfytj.display();
      } catch (e) {}
    };
    var scr = d.getElementsByTagName(t)[0];
    scr.parentNode.insertBefore(s, scr);
  })(document, "script");
</script>
```

### Retrieving the MoonClerk ID associated with your custom ID

The [Customer](/webhooks.md#example-customer-payload) and
[Payment](/webhooks.md#example-payment-payload) webhook payloads should contain a `custom_id` field which should match the `cid` you provided.

You will want to query your resource using the `custom_id` and then store the associated `id` field so you can then use it to find the MoonClerk resource via our API.

---

⚠️ **Important!** Previous versions of this document referenced `payment_id` and `customer_id` parameters for the redirect URL. These parameters are no longer supported. As a replacement, we recommend to use webhooks.
