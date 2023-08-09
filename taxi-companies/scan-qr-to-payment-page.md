<!-- START_METADATA
---
sidebar_position: 101
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'
import FULLCAPTURE from '../_common/_full_capture.md'
END_METADATA -->

# Payment through company website

The customer scans a Vipps QR code and is directed to the taxi company's landing page.
There, they follow instructions and pay with Vipps.

![Labeling in the taxi](images/labeling_in_the_taxi.png)

## Details

### Step 1: Generate a static QR code

Generate a static QR code with a
[merchant redirect QR](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes)
linking to your website or app.

### Step 2: The customer scans the static QR

When the customer scans the QR, your system will receive a notification that the QR has been scanned and will be able to get the customer's phone number.

### Step 3: Send the payment request

Use the customer's phone number to send them a request for the taxi fare.

<details>
<summary>Detailed example</summary>
<div>

Specify `"customerInteraction": "CUSTOMER_PRESENT"` and `"userFlow": "WEB_REDIRECT"` to redirect user to the app.

Here is an example HTTP POST:

[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)

```json
{
  "amount": {
    "value": 10000,
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

### Step 4: The customer authorizes the payment

<AUTHORIZEPAYMENT />

### Step 5: Capture the payment

<FULLCAPTURE />

## Sequence diagram

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant QR as QR API
    participant ePayment as ePayment API

    QR->>C: Scan for customer ID
    M->>M: Add product to sale
    M->>ePayment: Initiate payment request with receipt
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    ePayment->>C: Provide payment information
    M->>ePayment: Capture payment 
    M->>ePayment: Check the status of capture
```
