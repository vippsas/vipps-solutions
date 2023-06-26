<!-- START_METADATA
---
title: Static QR directing to the merchant site for product selection
sidebar_label: Static QR direct to merchant site for product selection
sidebar_position: 40
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Static QR directing to the merchant site for product selection

This flow uses a static QR code that is posted on the vending machine.
The QR directs the user to the merchant's landing page which provides a user interface for selecting products and paying.

## Details

A merchant-generated QR code is posted on the vending machine.

When the customer scans the QR code,
they are taken to the merchant's landing page, where products can select.
The price is presented, and the user pays for the product in their Vipps app.

**When to use:**

* The merchant redirect QR code can be used when it is not possible to present the dynamic [one-time payment QR](one-time-payment.md).

**How to use:**

* Generate a static QR code with a [merchant redirect QR](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes)
linking to a webshop connected to the specific vending machine. The user selects the products and clicks the `pay` button that generates a
[Create payment request](https://developer.vippsmobilepay.com/api/epayment/#tag/CreatePayments/operation/createPayment) based on the selected products.

* Specify `"customerInteraction": "CUSTOMER_PRESENT"` and `"userFlow": "WEB_REDIRECT"` to redirect user to Vipps.

![3_qr_to_landing_page_providing_selection](images/3_qr_to_landing_page_providing_selection.png)
