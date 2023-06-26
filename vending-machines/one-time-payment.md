<!-- START_METADATA
---
title: Dynamic QR directing to the app for payment
sidebar_label: Dynamic QR direct to the app for payment
sidebar_position: 10
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Dynamic QR directing to the app for payment

This flow uses a one-time payment QR (i.e., a dynamic QR) that is shown on a screen.
The QR directs the user to the Vipps or MobilePay app, where they authorize the payment.

**Please note:** This flow requires that you have a screen connected.

## Details

A [Vipps QR code](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api/#one-time-payment-qr-codes) is presented on the vending machine.

When the customer scans the QR code, they go directly to the Vipps payment screen on their phone, where they can approve the payment.

The QR code is a dynamic representation of the payment URL, and the user needs to scan the QR code within 5 minutes.  

**When to use:**

* The One-Time payment QR code is the preferred flow when it's possible to show a dynamic QR code on the vending machine.

**How to use:**

* Vipps [ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api) supports both the One-time payment QR and payment in the
[CreatePaymentRequest](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments).
Specify `"customerInteraction": "CUSTOMER_PRESENT"` and  `"userFlow": "QR"` to generate the QR code to be presented on the customer facing screen.

![One-time payment QR](images/0_one_time_payment_qr.jpg)
