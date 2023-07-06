The customer approves the payment in the Vipps MobilePay app.

To get confirmation that payment was approved, monitor
[webhooks](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api) and
[query the payment](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment).

If you have already attached order information to this purchase, the customer will be able to see this in the Vipps app.
When they select `See details` in the payment confirmation screen, they are presented with the order information without leaving the app.

Once the customer authorizes the payment, update your system with the status.

Note that, for long-living payments, customers also have the option of soft-dismissing the payment and postponing it for later.
