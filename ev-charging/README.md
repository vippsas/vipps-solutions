<!-- START_METADATA
---
title: EV Charging
sidebar_position: 5
---
END_METADATA -->

# Electric vehicle charging

💥 TODO. This is just a placeholder for now. 💥

Vipps is an excellent choice for EV charging as practically all Norwegians have
the Vipps app in their pocket, simplifying charging with out downloading a
specific charging app.

Implemented correctly, EV charging with Vipps, is a simple and efficient
solution that let's your customers use your charging network with no hassle.

![EV charging with Vipps](images/ev-charging-process-icons.png)

Drop in charging with Vipps is best implemented using QR codes, scanned either
with the Vipps app or the phone's camera. Vipps provides a
[QR API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api)
that can be used in combination with the Vipps eCom API to set up and start a
Vipps payment.

## Payment flow

![EV charging with Vipps: Screenshots](images/ev-charging-process-screenshots.png)

1. Scan the QR code with Vipps
2. Get sent to the EV charging company's website
3. Select Vipps as payment process and click "Pay with Vipps"
4. Vipps is automatically opened, with the payment ready for confirming the maximum amount
5. Get sent back to the EV charging company's website where the status of the charge is presented
6. When the charging is complete: Get a push message with the final sum to pay in Vipps
7. Get the receipt with the payment details in Vipps

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-solutions/issues),
a [pull request](https://github.com/vippsas/vipps-solutions/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
