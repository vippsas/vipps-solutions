<!-- START_METADATA
---
title: Vending machines
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Vending machines

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->


## QR direct to payment in Vipps app (recommended!)


Post a [Vipps QR code](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes) on the vending machine.

When the customer scans the QR code, they go directly to the Vipps payment screen on their phone, where they can approve the payment.

![qr_direct_to_payment](images/1_qr_direct_to_payment.png)

This is the most recommended flow, because it is simplest for the user.

## QR to a merchant site for payment options

Post a merchant-generated QR code on the vending machine.

When the customer scans the QR code,
they are taken to the merchant's landing page which is waiting for the product to be selected on the vending machine.
The total price is calculated and the user pays for the product(s) in their Vipps app.

![2_qr_to_landing_page_waiting_for_selection](images/2_qr_to_landing_page_waiting_for_selection.png)



## QR to a merchant site where products are selected

Post a merchant-generated QR code on the vending machine.

When the customer scans the QR code,
they are taken to the merchant's landing page where products can selected.
The total price is calculated and the user pays for the product(s) in their Vipps app.

![3_qr_to_landing_page_providing_selection](images/3_qr_to_landing_page_providing_selection.png)
