<!-- START_METADATA
---
sidebar_position: 101
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Vipps payment through company website

The customer scans a Vipps QR code and is directed to the taxi company's landing page.
There, they follow instructions and pay with Vipps.

![Labeling in the taxi](images/labeling_in_the_taxi.png)

## Details

The flow is a variation of the [In-store using static QR](../static-qr-at-pos/README.md) flow.

### Step 1: Generate a static QR code

Generate a static QR code with a
[merchant redirect QR](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes)
linking to your website or app.

### Step 2: The customer scans the static QR

When the customer scans the QR, your system will receive a notification that the QR has been scanned and will be able to get the customer's phone number.

### Step 3: Send the payment request

Use the customer's phone number to send them a [Create Payment request](https://developer.vippsmobilepay.com/api/epayment/#tag/CreatePayments/operation/createPayment) for the taxi fare through Vipps.

Specify `"customerInteraction": "CUSTOMER_PRESENT"` and `"userFlow": "WEB_REDIRECT"` to redirect user to the app.

### Step 4: The customer authorizes the payment

The payment request will appear in the customer's Vipps app where they can authorize the payment.

To get confirmation that payment was approved, monitor
[webhooks](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api) and
[query the payment](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment).

### Step 5: Attach a receipt to the order

[`postReceipt` endpoint](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2)
allows you to send receipt information to the customer's app.

The customer will get the receipt in their Vipps MobilePay app.

See
[Adding a receipt](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api/#adding-a-receipt)
for more details.

### Step 6: Capture the payment

Once the customer authorizes the payment, update the POS system with the status.

The
[`capturePayment` endpoint](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment)
allows you to capture a payment.

Be sure to check the status of the captured payment.

## Sequence diagram

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant QR as QR API
    participant ePayment as ePayment API
    participant ordermanagement as Order Management API

    QR->>C: Scan for customer ID
    M->>M: Add product to sale
    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    M->> ordermanagement: Attach receipt
    ePayment->>C: Provide payment information
    M->>ePayment: Initiate payment capture
    ePayment->>C: Capture payment
    M->>ePayment: Check the status of capture
```
