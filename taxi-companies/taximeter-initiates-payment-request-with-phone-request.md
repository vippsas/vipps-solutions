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

### Step 2. Initiate a payment request to Vipps

To create this payment, you first send a
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request, where `customer.phoneNumber` is set.

Use `userFlow:PUSH_MESSAGE` and `"customerInteraction": "CUSTOMER_PRESENT"` while initiating the payment.

### Step 3. The customer approves the payment

The customer will receive a push notification in their Vipps app and can approve the payment.


### Step 4. Capture the amount

When reservation is complete, perform a [full capture](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture#capture-via-the-api).

## Sequence diagram

Sequence diagram for the standard online payment flow, where payment request is sent directly to app.

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant ePayment as ePayment API

    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    ePayment->>M: Check the status
    M->>ePayment: Initiate payment capture
    ePayment->>C: Capture payment
```
