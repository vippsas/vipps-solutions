<!-- START_METADATA
---
sidebar_position: 103
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'
import ATTACHRECEIPT from '../_common/_attach_receipt.md'
END_METADATA -->

# Payment through taximeter and app

The customer orders a taxi in the taxi company's app. After the journey, the taximeter automatically initiates a payment request to Vipps with the customer's phone number, which is already known.
The customer's Vipps app open and the customer pays the amount due.

## Details

### Step 1. Get the customer's phone number through taxi app

Your system should contain the customer's phone number, since they ordered the taxi through the app.

### Step 2. Initiate a payment request

Send
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request, where `customer.phoneNumber` is set.

Use `userFlow:PUSH_MESSAGE` and `"customerInteraction": "CUSTOMER_PRESENT"` while initiating the payment.

### Step 3. The customer approves the payment

<AUTHORIZEPAYMENT />

### Step 4. Attach a receipt

<ATTACHRECEIPT />

### Step 5. Capture the amount due

<PARTIALCAPTURE />

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
