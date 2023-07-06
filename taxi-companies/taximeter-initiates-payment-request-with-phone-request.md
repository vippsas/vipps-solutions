<!-- START_METADATA
---
title: Customer orders through taxi app and taximeter initiates the payment request
sidebar_label: Customer orders through taxi app and taximeter initiates the payment request
sidebar_position: 103
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Taximeter initiates the payment request with manually entered phone number

The driver enters the customer's mobile number into the taximeter, which initiates a push.
The Vipps app opens on the customer's phone and the customer pays the amount due.

## Details

### Step 1. Ask for the customer's phone number

Get the customer's phone number and enter it into your taximeter system.

### Step 2. Initiate a payment request

To create this payment, you first send a
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request, where `customer.phoneNumber` is set.

Use `userFlow:PUSH_MESSAGE` and `"customerInteraction": "CUSTOMER_PRESENT"` while initiating the payment.

### Step 3. The customer approves the payment

The payment request will appear in the customer's Vipps app where they can authorize the payment.

To get confirmation that payment was approved, monitor
[webhooks](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api) and
[query the payment](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment).

### Step 4. Attach a receipt to the order

The
[`postReceipt` endpoint](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2)
allows you to send receipt information to the customer's app.

The customer will get the receipt in their Vipps MobilePay app.

See
[Adding a receipt](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api/#adding-a-receipt)
for more details.

### Step 5. Capture the amount

When reservation is complete, perform a
[capture](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture#capture-via-the-api).

The
[`capturePayment` endpoint](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment)
allows you to capture a payment.

Be sure to check the status of the captured payment.

## Sequence diagram


``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant ePayment as ePayment API

    M->>C: Request phone number verbally
    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    M->>ePayment: Check the status of authorization
    M->>ePayment: Initiate payment capture
    ePayment->>C: Capture payment
    M->>ePayment: Check the status of capture
```
