# Liquid Examples for MoonClerk

## Setting a different message for each form

You'll need to know the id of each form for which you want a custom message. You can find that by looking at the url when viewing a form. (These are example non-functional URLs used only for reference. They won't pull up an actual form.)

```
https://app.moonclerk.com/forms/32
https://app.moonclerk.com/forms/97
https://app.moonclerk.com/forms/104
```

The form IDs are 32, 97, and 104 in the above urls.

```
This email confirms that {{ account.name }} has successfully charged your card for {{ payment.amount | money_with_currency }}.{% if form.id == 32 %}

This is my custom message for Form 32{% elsif form.id == 97 %}

This is my custom message for Form 97{% elsif form.id == 104 %}

This is my custom message for Form 104{% endif %}

Payment Details:

Charge Date: {{ payment.date | date: '%b %d, %Y' }}
Bill To: {{ payment.name }}
Amount: {{ payment.amount | money_with_currency }}
Last 4 Card Digits: {{ payment.card.last4 }}{% if plan %}

Recurring Plan Details:

Amount: {{ plan.amount | money_with_currency }}
Frequency: {{ plan.frequency | capitalize }}
Start Date: {{ plan.start | date: '%b %d, %Y' }}{% if plan.expires %}
Expiration: {{ plan.expires | date: '%b %d, %Y' }}{% endif %}{% if plan.next_payment_attempt %}
Next Payment Attempt: {{ plan.next_payment_attempt | date: '%b %d, %Y'  }}{% endif %}

To update your card on file, please visit:
{{ plan.card.update_url }}{% endif %}
```