<!-- START_METADATA
---
sidebar_position: 103
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'

END_METADATA -->

# Payment through taximeter and app

The customer orders a taxi in the taxi company's app. After the journey, the taximeter automatically initiates a payment request to Vipps with the customer's phone number, which is already known.
The customer's Vipps app open and the customer pays the amount due.

## Details

### Step 1. Get the customer's phone number through taxi app

Your system should contain the customer's phone number, since they ordered the taxi through the app.

### Step 2. Initiate a payment request

Use the customer's phone number to send them a request for the taxi fare.

<details>
<summary>Detailed example</summary>
<div>

To create this payment, you first send a
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request, where `customer.phoneNumber` is set.

Use `userFlow:PUSH_MESSAGE` and `"customerInteraction": "CUSTOMER_PRESENT"` while initiating the payment.
You need the customer's phone number to send them a request from the taximeter.

You may also attach the receipt at this time.

Here is an example:

[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)

```json
{
  "amount": {
    "value": 100000,
    "currency": "NOK"
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "customer": {
    "phoneNumber": 4791234567
  },
  "customerInteraction": "CUSTOMER_PRESENT",
  "receipt":{
    "orderLines": [
      {
        "name": "trip",
        "id": "line_item_1",
        "totalAmount": 100000,
        "totalAmountExcludingTax": 80000,
        "totalTaxAmount": 20000,
        "taxPercentage": 25,
      },
    ],
    "bottomLine": {
      "currency": "NOK",
      "posId": "taxi_122",
      "tipAmount": 10000
    },
   "receiptNumber": "0527013501"
  },
  "reference": 2486791679658155992,
  "userFlow": "PUSH_MESSAGE",
  "returnUrl": "http://example.com/redirect?reference=2486791679658155992",
  "paymentDescription": "Travel from Oslo central station to Oslo airport"
}

```

</div>
</details>

### Step 3. The customer approves the payment

<AUTHORIZEPAYMENT />

### Step 4. Capture the amount due

Capture the payment and confirm that it was successful.

<details>
<summary>Detailed example</summary>
<div>

[`POST:/epayment/v1/payments/{reference}/capture`](/api/epayment/#tag/AdjustPayments/operation/capturePayment)

With body:

```json
{
  "modificationAmount": {
    "value": 100000,
    "currency": "NOK"
  }
}
```

</div>
</details>

## Sequence diagram

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant ePayment as ePayment API

    M->>C: Get phone number through taxi app
    M->>ePayment: Initiate payment request and attach receipt
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    M->>ePayment: Check the status of authorization
    M->>ePayment: Capture payment
```
