Register a webhook to alert you when your payment requests are approved.

<details>
<summary>Details</summary>
<div>

When changes happen to your payment request, events are triggered (for example: *Authorized*, *Terminated*, *Aborted*, *Cancelled*, *Expired*, and
[many more](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api/events/)).

You can register to receive these events, which is useful for programming an appropriate and quick response.

The webhook will send a message to your web server at the URL you specify.

Here is an example HTTP POST:

[`POST:/webhooks/v1/webhooks`](https://developer.vippsmobilepay.com/api/webhooks/#tag/v1/paths/~1v1~1webhooks/post)

```json
{  
    "url": "https://example.com/mystore_website_backend", 
    "events": ["epayments.payment.authorized.v1"] 
}
```

Use the `id` and `secret` to authenticate the message with HMAC.

The [payload](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/webhooks/) from this webhook will be in this form:

```json
{
    "msn": "123456",
    "reference": "24ab7cd6ef658155992",
    "pspReference": "1234567891",
    "name": "AUTHORIZED",
    "amount":
    {
        "currency": "NOK",
        "value": 35000
    },
    "timestamp": "2023-08-14T12:48:46.260Z",
    "idempotencyKey": "49ca711a9487112e1def",
    "success": true
}
```

</div>
</details>
