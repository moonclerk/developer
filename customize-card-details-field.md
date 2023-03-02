# Styling the "Card Details" field on your payment form

## What has changed?

MoonClerk has always supported applying custom CSS (Cascading Style Sheets) to your payment forms to create a pleasing and brand-matched experience for your payers. Due to changes in PCI Security Standards, Stripe introduced Stripe Elements to comply with these changes. Effectively, it moves the credit card input fields from MoonClerk's server to Stripe's servers. The net effect is that as of March 8, 2018, styling of the new Stripe Elements card field cannot be accomplished using CSS alone. If you need to provide additional styling to your payment form's card details field, you'll need to use a combination of CSS and custom JSON styling.

This is an advanced feature and there is a possibility that implementing it incorrectly could cause issues with your checkout. We suggest reading over the documentation carefully and reviewing the entire checkout process after you have customized the form to ensure it functions properly.

## OK, I want to customize my Card Details field...

There are a few things you need to know.

### Styling around the card input

If you want to style _around_ the input field, you'll still want to use custom CSS. The Card Element is contained in the `.StripeElement` class. Applying styles to it will effect the border, padding, margin, etc. **Do not attempt to style anything with CSS inside of the `.StripeElement` class. It will fail and could break your checkout.** Here is an example of applying custom CSS to the `.StripeElement` class:

```css
.StripeElement {
  background-color: white;
  height: 40px;
  padding: 10px 12px;
  border-radius: 4px;
  border: 1px solid transparent;
  box-shadow: 0 1px 3px 0 #e6ebf1;
  -webkit-transition: box-shadow 150ms ease;
  transition: box-shadow 150ms ease;
}

.StripeElement--focus {
  box-shadow: 0 1px 3px 0 #cfd7df;
}

.StripeElement--invalid {
  border-color: #fa755a;
}

.StripeElement--webkit-autofill {
  background-color: #fefde5 !important;
}
```

### Styling inside the card input

If you want to make any changes inside of the card input, you'll need to add properly formatted JSON to the "JSON Styling for Card Details Field" text area.

Stripe provides an [options reference](https://stripe.com/docs/stripe-js/reference#element-options) to help familiarize yourself with the options available.

**While the Stripe documentation shows options in JavaScript object format, MoonClerk requires that all option configuration be in JSON format. While very similar, there are key differences.**

#### BAD!

```javascript
{
  base: {
    color: '#32325d',
    lineHeight: '18px',
    fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
    fontSmoothing: 'antialiased',
    fontSize: '16px',
    '::placeholder': {
      color: '#aab7c4'
    }
  },
  invalid: {
    color: '#fa755a',
    iconColor: '#fa755a'
  }
}
```

#### GOOD

```json
{
  "base": {
    "color": "#32325d",
    "lineHeight": "18px",
    "fontFamily": "'Helvetica Neue', Helvetica, sans-serif",
    "fontSmoothing": "antialiased",
    "fontSize": "16px",
    "::placeholder": {
      "color": "#aab7c4"
    }
  },
  "invalid": {
    "color": "#fa755a",
    "iconColor": "#fa755a"
  }
}
```

There are four variants for your Card Details form field. Each variant should be a nested object within your main object:

`base` - all other variants inherit from this style
`complete` - applied only for valid input
`empty` - applied when there is no customer input
`invalid` - applied only for invalid input

For each of the variants above, the properties below can be customized with CSS properties:

```
color
fontFamily
fontSize
fontSmoothing
fontStyle
fontVariant
iconColor
lineHeight
letterSpacing
textAlign
textDecoration
textShadow
textTransform
```

Further, the following pseudo-classes can be styled with the above properties, as a nested object inside each variants:

```
:hover
:focus
::placeholder
::selection
:-webkit-autofill
::-ms-clear
```

All Card Details form field styling should exist (as valid JSON) within a single object, with all keys and values being wrapped in double quotes. We recommend composing your valid JSON in an external text editor or using a linter to assure you are providing valid JSON.
