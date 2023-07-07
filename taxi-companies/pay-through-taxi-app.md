<!-- START_METADATA
---
sidebar_position: 102
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'
import ATTACHRECEIPT from '../_common/_attach_receipt.md'
import PARTIALCAPTURE from '../_common/_partial_capture.md'
END_METADATA -->

# Payment through company app

The customer pays the taxi company from their app when ordering the taxi and selects to pay with Vipps.
The amount is reserved until the final amount is known, at which time the payment is captured.

![Taxi route](images/taxi_route.png)

This flow combines multiple products to illustrate the recommended online payment flow.

## Details

### Step 1. Get the customer's the payment method

Display an option to pay with Vipps on your app.

### Step 2. Initiate a payment request

When the customer is ready to pay, initiate a
[payment request](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments).

<details>
<summary>Details</summary>
<div>

The payment request amount should be large enough to cover the cost of the journey.

If the payment is approved, this amount will be reserved on customer's account.
The amount that is unused will be released when the journey is finished.

Set `userFlow` to `WEB_REDIRECT`, so the customer's browser will either do an automatic app-switch or open the landing page to confirm the mobile number.

</div>
</details>

### Step 3. The customer authorizes the payment

<AUTHORIZEPAYMENT />

### Step 4. Confirm the order

Upon authorization, the Vipps app should automatically redirect the customer to your app.
Confirm that the order has been successful in your app.

### Step 5. Attach a receipt

After the drive is complete, calculate how much the customer owes and provide a receipt.

<ATTACHRECEIPT />


### Step 6. Capture the amount due

<PARTIALCAPTURE />

## Related information


* [ePayment how it works guide](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/how-it-works/vipps-epayment-api-how-it-works-online)
* [Recommended flow for online payments](../online/README.md)

## Sequence diagram

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant ePayment as ePayment API
    participant ordermanagement as Order Management API

    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    M->>C: Display order confirmation
    M->>C: Drive
    M->>M: Determine the amount owed
    M->>C: Send a push notification with actual amount paid
    M->>ePayment: Initiate capture request for amount due
    M->>ePayment: Release <amount reserved - amount due>
    ePayment->>C: Capture amount due
    ePayment->>C: Release amount remaining
    M->>ordermanagement: Attach receipt showing amount due
    M->>ePayment: Check the status of capture
```
