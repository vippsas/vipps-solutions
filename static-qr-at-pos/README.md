<!-- START_METADATA
---
title: Vipps MobilePay in-store using static QR flow
sidebar_label: In-store using QR
description: Using Vipps in a physical setting with a static QR code
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# In-store using static QR

In this solution, a user pays by scanning a QR (could be on a sticker), followed by the merchant sending a payment request to that user.

The solution is a combination of the
[Merchant Callback QRs](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-callback-qr-codes) and the
[Vipps ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api).

The following describes the process at a high level.

![Loyalty Flow](images/static_qr_at_pos.png)

## Step 1: The user scan the QR

The user scans the static merchant callback QR. The QR could for example be shown on a screen, or be printed out and placed on a cash register, a portable POS, or a vending machine.

## Step 2: Merchant receives an ID

When the user scans the QR, the merchant will receive a notification that the QR has been scanned. Meanwhile, the Vipps/MobilePay app will show a waiting screen to the user. Thus, the user understands that the scan was successful.

## Step 3: Merchant sends payment

When the merchant is ready to get paid, the merchant uses the ID received in previous step to send the payment to the user through the ePayment API.

## Step 4: The user pays and get a receipt in the app

Once the payment is sent by the merchant, the user will be shown the payment screen. If the user still has their app open, the payment will open automatically.
