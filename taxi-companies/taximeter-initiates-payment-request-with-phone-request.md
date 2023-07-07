<!-- START_METADATA
---
sidebar_position: 103
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'
import ATTACHRECEIPT from '../_common/_attach_receipt.md'
import FULLCAPTURE from '../_common/_full_capture.md'
END_METADATA -->

# Payment with manual phone number entry

The driver enters the customer's mobile number into the taximeter
which then initiates a payment request to Vipps.
The Vipps app opens on the customer's phone and the customer pays the amount due.

## Details

### Step 1. Ask for the customer's phone number

Get the customer's phone number and enter it into your taximeter system.

### Step 2. Initiate a payment request

To create this payment, you first send a
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request, where `customer.phoneNumber` is set.

Use `userFlow:PUSH_MESSAGE` and `"customerInteraction": "CUSTOMER_PRESENT"` while initiating the payment.

### Step 3. The customer approves the payment

<AUTHORIZEPAYMENT />

### Step 4. Attach a receipt

<ATTACHRECEIPT />

### Step 5. Capture the payment

<FULLCAPTURE />

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
