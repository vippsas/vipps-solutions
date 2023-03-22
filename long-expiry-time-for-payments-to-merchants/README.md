<!-- START_METADATA
---
title: Extend payment time-outs
sidebar_label: Extend payment time-outs
sidebar_position: 110
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Extend payment time-outs

<!-- START_COMMENT -->

ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

💥 Work in progress 💥

You can extend the expiration time for payments beyond the default,
[10 minutes](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/timeouts).
In some cases, this is not enough time for a customer to complete the payment.
For instance:

* Paying at a doctor's office, where the payment request may arrive as the patient
  is leaving the office and doesn't notice it.
* Paying at a toll road where the driver cannot stop to complete the payment.

You can extend this expiration time for payments through the
[ePayment API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api).

Extending the payment expiration time is done by setting the `expiresAt` parameter in the
[`POST:/epayment/v1/payments`](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments)
request.
The `expiresAt` must be between 10 minutes and 28 days (40320 minutes) in the future.

This will send a push message to the customer's Vipps app on their mobile phone.
If the customer's phone number is unknown, the system can request with `userFlow` set to `QR`.
This will return the QR code for a payment with the expiration time that you have specified.

The customer either clicks on the notification or scans the QR code to complete the payment flow in the Vipps app.


![Payment flow in the app](images/Long-expiry-time-payment-request.png)

The customer can soft dismiss this payment by clicking `Cancel` -> `I'll pay later`.
This option will only be shown to payments that have `expiresAt` property set,
as shown below:

![Soft dismiss a payment](images/Soft-dismiss.png)

Soft-dismissed payments will be available in the Vipps app until expiry.
Vipps will remind the user when the payment is about to expire. For example:

![Soft dismiss a payment](images/Soft-dismissed-payment-in-home-screen.png)


See [How to create a payment request with extended expiration time](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/long-living-payments) for all the technical details.