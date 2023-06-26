<!-- START_METADATA
---
title: Static QR directing to the merchant site for payment
sidebar_label: Static QR direct to merchant site for payment
sidebar_position: 30
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Static QR directing to the merchant site for payment

This flow uses a static QR code that is posted on the vending machine.
The QR directs the user to the merchant's landing page where payment is initiated.

## Details

A merchant-generated QR code is posted on the vending machine.

When the customer scans the QR code, they are taken to the merchant's landing page, which is waiting for the product to be selected on the vending machine.
The price is presented, and the user pays for the product in their Vipps or MobilePay app.

**How to use:**

* Generate a static QR code with a
  [merchant redirect QR](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes)
  linking to a web page containing the selected product from the vending machine. The user clicks the pay button that generates a
  [Create Payment request](https://developer.vippsmobilepay.com/api/epayment/#tag/CreatePayments/operation/createPayment) with the selected amount.

* Specify `"customerInteraction": "CUSTOMER_PRESENT"` and `"userFlow": "WEB_REDIRECT"` to redirect the user to Vipps.

![QR to landing page while waiting for selection](images/2_qr_to_landing_page_waiting_for_selection.png)
