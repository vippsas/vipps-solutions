The
[capture](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment) request
allows you to capture a payment.

Be sure to check the status of the captured payment.
The payment is transferred to your account. This may take 2-3 days depending on your bank.

<details>
<summary>Detailed example</summary>
<div>

Here is an example HTTP POST:

[`POST:/epayment/v1/payments/{reference}/capture`](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment)

With body:

```json
{
  "modificationAmount": {
    "value": 10000,
    "currency": "NOK"
  }
}
```

</div>
</details>
