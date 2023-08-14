<!-- START_METADATA
---
title: Vipps MobilePay Payment request as a link
sidebar_label: Payment request as a link
sidebar_position: 52
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'

END_METADATA -->

# Payment request as a link

If you don't know your customer's phone number, you can start by sending them a link to your own landing page. There, you can trigger a payment request through Vipps, which will request their phone number automatically.

The flow for the customer will look like this:

<Tabs
defaultValue="vipps"
groupId="app-choice"
values={[
{label: 'Vipps', value: 'vipps'},
{label: 'MobilePay', value: 'mobilepay'},
]}>
<TabItem value="vipps">

![Vipps payment request landing page flow](images/payment-request-with-link-vipps.png)

</TabItem>
<TabItem value="mobilepay">

![MobilePay payment request landing page flow](images/payment-request-with-link-mobilepay.png)

</TabItem>
</Tabs>

## Details

### Step 1. Give the customer a QR or link to your page

Provide a QR code or link to your payment page where you present your customer with the option to pay with Vipps MobilePay.

<details>
<summary>Detailed example</summary>
<div>

The QR code contains a `Id` that connects it to the taxi where it is located.

Here is an example HTTP POST:

[`POST:/qr/v1/merchant-redirect`](https://developer.vippsmobilepay.com/api/qr/#operation/CreateMerchantRedirectQr)

```json
{
  "id": "user34",
  "redirectUrl": "https://example.com/mybill/1234678"
}
```

</div>
</details>


### Step 2. Create a payment request

When they select to pay with Vipps MobilePay, send the
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

<details>
<summary>Detailed example</summary>
<div>

Your system can send the payment request by using the
[`createPayment`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)
endpoint.

Set `userFlow` to `PUSH_MESSAGE`. This will send a push directly to the customer.
Attach the receipt simultaneously.

Here is an example HTTP POST:

[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)

With body:

```json
{
  "amount": {
    "value": 300000,
    "currency": "NOK"
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "receipt":{
    "orderLines": [
      {
        "name": "Accident insurance",
        "id": "12345",
        "totalAmount": 150000,
        "totalAmountExcludingTax": 112500,
        "totalTaxAmount": 37500,
        "taxPercentage": 25,
      },
      {
        "name": "Travel insurance",
        "id": "12345",
        "totalAmount": 150000,
        "totalAmountExcludingTax": 112500,
        "totalTaxAmount": 37500,
        "taxPercentage": 25,
      },
    ],
    "bottomLine": {
      "currency": "NOK",
    },
   "receiptNumber": "0527013501"
  },
  "reference": 2486791679658155992,
  "userFlow": "PUSH_MESSAGE",
  "returnUrl": "http://example.com/redirect?reference=2486791679658155992",
  "paymentDescription": "Spendings"
}
```

</div>
</details>

### Step 3. Customer approves the payment

<AUTHORIZEPAYMENT />

Note that, for long-living payments, customers also have the option of soft-dismissing the payment and postponing it for later.

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
    "value": 300000,
    "currency": "NOK"
  }
}
```

</div>
</details>

## Sequence diagram

Sequence diagram for the standard online payment flow, where payment request is sent as a link.

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant ePayment as ePayment API
    participant Webhooks as Webhooks API
    
    C->>M: Customer uses link to get to payment page
    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment and attach receipt
    C->>C: Customer clicks pay
    Webhooks-->>M: Callback with status of payment authorization
    M->>C: Display order confirmation on product page
    M-->>C: Ship the order (if applicable)
    M->>ePayment: Capture payment
    ePayment-->>M: Status of capture
    M->>M: Verify status of POS matches capture
```
