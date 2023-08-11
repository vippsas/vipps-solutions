<!-- START_METADATA
---
sidebar_position: 103
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'

END_METADATA -->

# Payment with manual phone number entry

The driver enters the customer's mobile number into the taximeter
which then initiates a payment request to Vipps.
The Vipps app opens on the customer's phone and the customer pays the amount due.

## Details

### Step 1. Ask for the customer's phone number

Get the customer's phone number and enter it into your taximeter system.

### Step 2. Initiate a payment request

Use the customer's phone number to send them a request for the taxi fare.

<details>
<summary>Detailed example</summary>
<div>

To create this payment, you first send a
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request, where `customer.phoneNumber` is set.

Specify `"customerInteraction": "CUSTOMER_PRESENT"`.
Specify `"userFlow": "WEB_REDIRECT"` to redirect user to the app.
You need the customer's phone number to send them a request from the taximeter.

You may also attach the receipt at this time.

Here is an example HTTP POST:

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
  "userFlow": "WEB_REDIRECT",
  "returnUrl": "http://example.com/redirect?reference=2486791679658155992",
  "paymentDescription": "Travel from Oslo central station to Oslo airport"
}
```

</div>
</details>


### Step 3. The customer approves the payment

<AUTHORIZEPAYMENT />

### Step 4. Capture the payment

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
    participant Webhooks as Webhooks API

    M->>C: Request phone number verbally
    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    Webhooks->>M: Callback with status
    M->>ePayment: Capture payment
```
