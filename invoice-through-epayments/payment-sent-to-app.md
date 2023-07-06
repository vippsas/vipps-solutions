<!-- START_METADATA
---
title: Vipps MobilePay payment request sent directly to app
sidebar_label: Payment request sent directly to app
sidebar_position: 51
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

import EX1 from './_create_payment_example.md'
END_METADATA -->

# Payment request sent directly to app

If you have the customer's phone number and their consent to send payment requests through Vipps MobilePay,
you can send payment requests directly to the customer.

The flow for the customer will look like this:

<Tabs
defaultValue="vipps"
groupId="app-choice"
values={[
{label: 'Vipps', value: 'vipps'},
{label: 'MobilePay', value: 'mobilepay'},
]}>
<TabItem value="vipps">

![Vipps payment request push flow](images/payment-request-sent-directly-to-app-vipps.png)

</TabItem>
<TabItem value="mobilepay">

![MobilePay payment request push flow](images/payment-request-sent-directly-to-app-mobilepay.png)

</TabItem>
</Tabs>

## Details

### Step 1. Create a payment request

To create this payment, you first send a
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request, where `customer.phoneNumber` is set.

<EX1 />

The customer will receive a push notification in their Vipps MobilePay app.

### Step 2. Customer approves the payment

When the customer selects `See details` in the payment confirmation screen, they are presented with the order information provided by the merchant without leaving the app.

The customer approves the payment.

For long-living payments, customers also have the option of soft-dismissing the payment and postponing it for later.


### Step 3. Add a receipt

Add a receipt using the [Order Management API](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api/#adding-a-receipt).

The receipt with the payment details will be provided the customer's Vipps app.

### Step 4. Capture the payment

[Capture](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/capturePayment) the payment.

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
    ePayment->>M: Callback with status
    M->>C: Display order confirmation on product page
    M->> ordermanagement: Attach receipt
    ePayment->>C: Provide payment information
    M-->>C: Ship the order (if applicable)
    M->>ePayment: Initiate payment capture
    ePayment->>C: Capture payment
```
