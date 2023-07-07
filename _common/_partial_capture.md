After final amount is confirmed, do a
[partial capture](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture#partial-capture).
Then, release the remaining amount from the reservation with a
[partial cancel](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/cancel#cancel-after-a-partial-capture).


Check the status of the captured payment.

<details>
<summary>Detailed example</summary>
<div>

First, the capture:

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

Then, cancel after partial capture:

[`POST:/epayment/v1/payments/{reference}/cancel`](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment)

With body:

```json
{
   "aggregate":{
      "authorizedAmount":{
         "currency":"NOK",
         "value":49900
      },
      "cancelledAmount":{
         "currency":"NOK",
         "value":0
      },
      "capturedAmount":{
         "currency":"NOK",
         "value":10000
      },
      "refundedAmount":{
         "currency":"NOK",
         "value":0
      }
   }
}
```

</div>
</details>
