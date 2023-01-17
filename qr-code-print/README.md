<!-- START_METADATA
---
title: QR codes for print
sidebar_position: 8
---
END_METADATA -->

# QR codes for print

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

It is possible to use QR codes with Vipps in two ways:

1. QR code that contains a URL to the merchant's website. We call these
   [Merchant Redirect QR codes](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes).
2. QR code that leads directly to the payment confirmation screen in Vipps.
   We call these
   [One-Time Payment QR codes](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api/vipps-qr-api#one-time-payment-qr-codes).

Merchant redirect QR codes may be printed, but one-time payment QR codes may not.
The reason for this is that a one-time payment QR code requires that the payment
has already been initiated, and that Vipps payments time out after 5 minutes.
See [Timeouts](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/timeouts).

Another important thing: The user must be presented with the terms and conditions
of the purchase before confirming the payment in Vipps. There is no functionality
for showing terms and conditions on the payment screen in Vipps. This is why
the answer to
[Can I send a Vipps payment link in an SMS, QR or email?](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs/reserve-and-capture-faq#can-i-send-a-vipps-payment-link-in-an-sms-qr-or-email)
is "No", but with an important exception:

> It may be acceptable to automatically trigger the Vipps payment when the user
> enters your website. This requires that the payment process is user initiated,
> and that there are no relevant terms and conditions or that the user has
> accepted any terms and conditions at an earlier stage.

This means that for printed QR codes there are two alternatives:

* Use a QR code that contains a URL to the merchant's website, and
  use
  [Vipps Hurtigkasse (express checkout)](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#express-checkout-payments)
  or a regular Vipps payment there.
* Present the terms and conditions of the purchase together with the
  QR code. Use a QR code that contains a URL to the website, but simply
  create a payment instantly and then redirect the user directly to the Vipps
  payment instead of showing and content on the website.
