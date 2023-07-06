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

# Customer orders through taxi app and taximeter initiates the payment request

Customer orders through taxi app and taximeter initiates the payment request.

The customer orders a taxi in the taxi company's app. After the journey, the taximeter automatically initiates a payment request to Vipps MobilePay with the customer's phone number, which is already known.
The customer's Vipps or MobilePay app open and the customer pays the amount due.

Use the [ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api) with `userFlow:PUSH_MESSAGE` and `customerInteraction: CUSTOMER_PRESENT` while initiating the payment.

After final amount is confirmed do a [partial capture](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture#partial-capture)
and release the remaining amount from reservation with a [partial cancel](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/cancel#cancel-after-a-partial-capture) request.

## Details

### Step 1. Get the customer's phone number through taxi app

Your system should contain the customer's phone number, since they ordered the taxi through the app.

### Step 2. Initiate a payment request

Send
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

    M->>C: Get phone number through taxi app
    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    M->>ePayment: Check the status of authorization
    M->>ePayment: Initiate payment capture
    ePayment->>C: Capture payment
    M->>ePayment: Check the status of capture
```
