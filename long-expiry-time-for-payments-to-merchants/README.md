<!-- START_METADATA
---
title: Extended expiration payments
sidebar_position: 110
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Extended expiration for payments to merchants

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

💥 Work in progress 💥


The typical expiration time for payments is
payment is
[10 minutes](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/timeouts).
But, in some cases, this is not enough time for a customer to complete the payment.

For instance:

* Paying at a doctor's office, where the payment request may arrive as the patient
  is leaving the office and doesn't notice it.
* Paying at a toll road where the driver cannot stop to complete the payment.

You can extend this expiration time for payments through the
[ePayment API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api).

[How to create a payment request with extended expiration time](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api/features/long-living-payments)

![Payment flow in the app](images/Long-expiry-time-payment-request.png)

The customer can soft dismiss this payment by clicking `Cancel` -> `I'll pay later`.
This option will only be shown to payments that have `expiresAt` property set,
as shown below:

![Soft dismiss a payment](images/Soft-dismiss.png)

Soft-dismissed payments will be available in the Vipps app until expiry.
Vipps will remind the user when the payment is about to expire. For example:

![Soft dismiss a payment](images/Soft-dismissed-payment-in-home-screen.png)
