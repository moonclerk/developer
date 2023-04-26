# Embedding a Payment Form on Your Site

Embedding a form on your website allows payers to checkout right on your site. Before embedding a payment form on your website, please keep in mind the following information:

1. If you have uploaded an logo/image to the theme the form is using, we do not display that image in the embed. In addition, the page background color is set to transparent and the exterior borders of the form are removed.
2. When the form is embedded, it expands to 100% of the width of the containing element by default. The minimum width for the containing element should be no less than 320 pixels. To edit the width of the embedded form, find the word "width" in the embed code we provide and adjust the pixel count that is listed after the colon.
3. Some payment methods using [3D Secure](https://stripe.com/docs/payments/3d-secure) may require a redirect to a banking website to authorize the payment. In these cases the checkout cannot be fully completed in the embedded frame. But you can configure your payment form to [redirect back to your site](https://help.moonclerk.com/en/articles/1239873-how-do-i-redirect-payers-to-a-web-page-that-i-designate-after-they-finish-checking-out) to somewhat maintain the seamless experience.

[View this page](https://help.moonclerk.com/en/articles/1239752-how-do-i-embed-one-of-my-payment-forms-inside-my-website) to see where you can find the embed code for a form.

## Options

| key            | description                                                                   | example value        |
| -------------- | ----------------------------------------------------------------------------- | -------------------- |
| `width`        | width of the embeded checkout (the default is `"100%"`)                       | `"100%"`             |
| `amount_cents` | prefilled amount (only works when the form has an amount decided at checkout) | `1000`               |
| `name`         | pre-filled name                                                               | `"Leon Bridges"`     |
| `email`        | pre-filled email                                                              | `"leon@example.com"` |
| `cid`          | custom ID ([see here](/integration.md#passing-a-custom-id) for more info)     | `"2-4757546-456"`    |

Here is an example with all of the options above.

```html
<div id="mcREPLACE_WITH_TOKEN"><a href="http://moonclerk.dev/pay/REPLACE_WITH_TOKEN">Easy Time</a></div>
<script type="text/javascript">var mcREPLACE_WITH_TOKEN;(function(d,t) {var s=d.createElement(t),opts={
  "checkoutToken":"REPLACE_WITH_TOKEN",
  "width":"100%",
  "amount_cents":1000,
  "name":"Leon Bridges",
  "email":"leon@example.com",
  "cid":"2-4757546-456"
};
s.src='https://d2l7e0y6ygya2s.cloudfront.net/assets/embed.js';s.onload=s.onreadystatechange = function() {var rs=this.readyState;if(rs) if(rs!='complete') if(rs!='loaded') return;try {mcREPLACE_WITH_TOKEN=new MoonclerkEmbed(opts);mcREPLACE_WITH_TOKEN.display();} catch(e){}};var scr=d.getElementsByTagName(t)[0];scr.parentNode.insertBefore(s,scr);})(document,'script');</script>
```

⚠️ NOTE: All instances of `REPLACE_WITH_TOKEN` must have the correct token value from the form. This value will be auto-generated when you [copy the embed code](https://help.moonclerk.com/en/articles/1239752-how-do-i-embed-one-of-my-payment-forms-inside-my-website) from the MoonClerk dashboard, so always start there and change the `opts` as-needed.
