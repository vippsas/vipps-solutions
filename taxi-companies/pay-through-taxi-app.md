<!-- START_METADATA
---
sidebar_position: 102
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'
END_METADATA -->

# Payment through company app

The customer pays the taxi company from their app when ordering the taxi and selects to pay with Vipps.
The amount is reserved until the final amount is known, at which time the payment is captured.

This flow combines multiple products to illustrate the recommended online payment flow.

## Details

### Step 1. Get the customer's the payment method

Display an option to pay with Vipps on your app.

### Step 2. Initiate a payment request

When the customer is ready to pay, initiate a payment request.

The payment request amount should be large enough to cover the cost of the journey.
Do not include the receipt yet, since a receipt is immutable and the true amount is not known yet.

This amount will be reserved on the customer's account, and the unused amount will be released when the journey is finished.

<details>
<summary>Detailed example</summary>
<div>

To create this payment, you first send a
[create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

Since the customer is selecting this from an app on their phone, you don't need their phone number.
This payment command can do an app-switch and open their Vipps app with the payment request.
Specify `"userFlow": "WEB_REDIRECT"` to redirect user to the app.

Specify `"customerInteraction": "CUSTOMER_PRESENT"`.


Here is an example:

[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)

```json
{
  "amount": {
    "value": 31400,
    "currency": "NOK"
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "customerInteraction": "CUSTOMER_PRESENT",
  "reference": 2486791679658155992,
  "userFlow": "WEB_REDIRECT",
  "returnUrl": "http://example.com/redirect?reference=2486791679658155992",
  "paymentDescription": "Travel from Oslo central station to Storo"
}

```

</div>
</details>

### Step 3. The customer authorizes the payment

<AUTHORIZEPAYMENT />

### Step 4. Confirm the order

Upon authorization, the Vipps app should automatically redirect the customer to your app.
Confirm that the order has been successful in your app.

### Step 5. Complete the journey

After the drive is complete, calculate how much the customer owes.

### Step 6. Capture the amount due

After final amount is confirmed, do a
[partial capture](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture#partial-capture).

Check the status of the captured payment.

<details>
<summary>Detailed example</summary>
<div>

[`POST:/epayment/v1/payments/{reference}/capture`](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment)

With body:

```json
{
  "modificationAmount": {
    "value": 29900,
    "currency": "NOK"
  }
}
```

</div>
</details>

### Step 7. Cancel remaining amount

Release the remaining amount from the reservation with a
[cancel](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/cancel#cancel-after-a-partial-capture).

[`POST:/epayment/v1/payments/{reference}/cancel`](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment)

### Step 8. Attach a receipt

Attach a receipt with the amount paid.

<details>
<summary>Detailed example</summary>
<div>

Here is an example:

[`POST:/order-management/v2/{paymentType}/receipts/{orderId}`](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2)

For `paymentType`, use `eCom` for eCom or ePayment payments.
For `orderId`, use the `chargeId` of the charge.

Body:

```json
{
  "orderLines": [
    {
        "name": "trip",
        "id": "line_item_1",
        "totalAmount": 29900,
        "totalAmountExcludingTax": 22425,
        "totalTaxAmount": 7475,
        "taxPercentage": 25,
        "productUrl": "https://www.example.com/taxiride",
      },
    },
  ],
  "bottomLine": {
    "currency": "NOK",
    "posId": "taxi_122",
  }
}
```

</div>
</details>

## Related information

* [ePayment Quick start guide](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/quick-start/)
* [Order Management Quick start guide](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api-quick-start/)

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
    M->>ordermanagement: Attach receipt showing amount paid
```
