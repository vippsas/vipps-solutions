<!-- START_METADATA
---
title: In-store using static QR flow
sidebar_label: In-store using static QR
sidebar_position: 40
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# In-store using static QR

In this flow, a user pays by scanning a QR, such as a sticker with their Vipps or MobilePay app. The merchant gets their ID and sends a payment request to their app. They approve it, and the merchant sends them a receipt.

## Details

The flow is a combination of the
[QR API: Merchant Callback QRs](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-callback-qr-codes) and the
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api).

The following describes the process at a high level.

![User scans QR. Merchant gets ID and sends payment. User pays and gets receipt.](images/static_qr_at_pos.png)

### Step 1: The user scans the QR

The user scans the static merchant callback QR. The QR could for example be shown on a screen,
or be printed out and placed on a cash register, a portable POS, or a vending machine.

### Step 2: Merchant receives an ID

When the user scans the QR, the merchant will receive a notification that the QR has been scanned.
Meanwhile, the Vipps or MobilePay app will show a waiting screen to the user. Thus, the user understands that the scan was successful.

### Step 3: Merchant sends the payment request

When the merchant is ready to get paid, the merchant uses the ID received in previous step to send the payment request to the user through the ePayment API.

### Step 4: The user pays

If the user has the app open, the payment screen will open automatically.
Otherwise, the payment screen will appear to them upon opening and logging into the app.

The merchant sends a receipt to the user's app.
