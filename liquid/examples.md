# Liquid Examples for MoonClerk

## Using custom fields

There are a variety of ways to use custom field depending on your needs. Here are a few ideas.

### Looping over all custom fields

When looping through fields, the actual variable name will be determined
by how you name the for loop in the template. In this example, the variable
will be called `field`.

```Liquid
{% for field in payment.custom_fields %}
{{ field[1].title }}: {{ field[1].response }}
{% endfor %}
```

**Liquid uses the `[1]` to access the value of the field hash.**


### Accessing a specific field for display

Another way to deal with custom field is to access a specific field. It is important to remember that the notification template is across all forms. If you write code to accesses a certain custom field you will want it to degrade nicely if the field doesn't exist on the current payment or plan.

Let's say you have a `shipping_address` key on many of you forms and want to output it on the payment successful notification to the account holder.

```Liquid
{% if payment.custom_fields.shipping_address %}
{% assign address = payment.custom_fields.shipping_address.address %}
{{ address.line1 }}{% if address.line2 %}
{{ address.line2 }}{% endif %}
{{ address.city }}, {{ address.state }} {{ address.postal_code }}{% endif %}
```


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
Payment Method Last 4: {{ payment.payment_source.last4 }}{% if plan %}

Recurring Plan Details:

Amount: {{ plan.amount | money_with_currency }}
Frequency: {{ plan.frequency | capitalize }}
Start Date: {{ plan.start | date: '%b %d, %Y' }}{% if plan.expires %}
Expiration: {{ plan.expires | date: '%b %d, %Y' }}{% endif %}{% if plan.next_payment_attempt %}
Next Payment Attempt: {{ plan.next_payment_attempt | date: '%b %d, %Y'  }}{% endif %}

To update your card on file, please visit:
{{ plan.payer_manage_url }}{% endif %}
```
