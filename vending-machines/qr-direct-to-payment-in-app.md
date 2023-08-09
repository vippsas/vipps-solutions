<!-- START_METADATA
---
title: Static QR directing to the app for payment
sidebar_label: Static QR direct to the app for payment
sidebar_position: 20
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'
END_METADATA -->

# Static QR directing to the app for payment

This flow uses a static QR code that is posted on the vending machine. The QR directs the user to the payment screen in their Vipps MobilePay app.

![QR direct to payment](images/1_qr_direct_to_payment.png)

## When to use

This QR code can be used when you don't have a screen, and it's not possible to present the dynamic [one-time payment QR](one-time-payment.md).

## Details

This is a variation of the [In-store using static QR](../static-qr-at-pos/README.md) flow.

A Vipps MobilePay [QR code](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes) is posted on the vending machine.

When the customer scans the QR code, they go directly to the Vipps or MobilePay payment screen on their phone, where they can approve the payment.

The payment amount should be the max amount of the vending machine products. After reservation, the amount of the selected product must be captured, and the remaining amount must be released.

### Step 1: Generate a static QR code

Generate a static QR code using our [QR API](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api/#merchant-callback-qr-codes)
linking the QR to your vending-machine.

### Step 2: Generate a payment request

When the user scans the QR code, send a [Create Payment request](https://developer.vippsmobilepay.com/api/epayment/#tag/CreatePayments/operation/createPayment).

Specify the amount of the most expensive item in your vending machine so that any selection they make can be successful.

Specify `"customerInteraction": "CUSTOMER_PRESENT"` and `"userFlow": "WEB_REDIRECT"` to redirect user to the app.

Include a receipt in the ePayment request.

### Step 3: Customer authorizes the payment

<AUTHORIZEPAYMENT />

### Step 4: Capture the amount

<details>
<summary>Detailed example</summary>
<div>

[`POST:/epayment/v1/payments/{reference}/capture`](/api/epayment/#tag/AdjustPayments/operation/capturePayment)

With body:

```json
{
  "modificationAmount": {
    "value": 3000,
    "currency": "NOK"
  }
}
```

</div>
</details>

## Sequence diagram

Sequence diagram for the vending machine flow with static QR directing to the app for payment.

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant QR as QR API
    participant ePayment as ePayment API
    
    QR->>C: Scan for customer ID
    M->>M: Add product to sale
    M->>ePayment: Initiate payment request with receipt
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    ePayment->>C: Provide payment information
    M->>ePayment: Initiate payment capture
    M->>ePayment: Initiate capture request for amount due
    M->>ePayment: Release <amount reserved - amount due>
    ePayment->>C: Capture amount due
    ePayment->>C: Release amount remaining
    M->>ePayment: Check the status of capture
```
