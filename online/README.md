<!-- START_METADATA
---
title: Vipps MobilePay online payments flow
sidebar_label: Online payments
sidebar_position: 10
description: Using Vipps MobilePay in an online setting
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Online payments

This flow combines multiple products to illustrate the recommended online payment flow.

![ePayment online process](images/ePayment_online.png)

## Details

### Step 1. Get the customer's the payment method

Display an option to *Pay with Vipps* or *Pay with MobilePay*, on the product page of your website or app.

### Step 2. Send the payment request to Vipps

Add the products to the order and send the payment request to Vipps.

If the payment was started on a desktop device, the customer will be sent to the
[Vipps landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page/).
There, they confirm their number and are prompted to log in through the Vipps app on their phone.

![Landing page](images/vipps-ecom-step2.svg)

If the payment was started from a mobile device, the app automatically switches over to the Vipps app.

![Pay with Vipps MobilePay](images/vipps-ecom-step1-2.png)


<details>
<summary>Detailed example</summary>
<div>
Here is an example HTTP POST:

[`POST:/epayment/v1/payments`](/api/epayment#tag/CreatePayments/operation/createPayment)

With body:

```json
{
  "amount": {
    "value": 49900,
    "currency": "NOK"
  },
  "paymentMethod": {
    "type": "WALLET"
  },
  "customer": {
    "phoneNumber": 4796574209
  },
  "reference": 2486791679658155992,
  "userFlow": "WEB_REDIRECT",
  "returnUrl": "http://example.com/redirect?reference=2486791679658155992",
  "paymentDescription": "Purchase of socks"
}
```


Set `userFlow` to `WEB_REDIRECT`, so the customer's browser will either do an automatic app-switch or open the landing page to confirm the mobile number.
</div>
</details>

### Step 3. The customer authorizes the payment

The customer receives a push notification in their app. They log in and confirm the payment.
Poll or monitor callbacks to see that the payment is approved, then reserve it and
provide a receipt.

![Confirm payment](images/vipps-ecom-confirm2.png)

### Step 4. Confirm the order

The app redirects the customer back to your store, where you confirm that the order has been successful.

![Order confirmation](images/vipps-ecom-step4-2.png)

### Step 5. Add a receipt

Add a payment receipt to the order. This will appear in their app.

This `postReceipt` endpoint,
[`POST:/order-management/v2/ecom/receipts/{reference}`](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2),
is for sending receipt information.
This is a combination of *order lines* and a *bottom line* with sum and VAT.
An *order line* is a description of each item present in the order.

![Order receipt](images/order-receipt.png)

### Step 6. Ship the order

Complete and ship the order to the customer.
![Shipping](images/vipps-shipping.png)

### Step 7. Capture the payment

Capture the payment.

The payment is transferred to your account. This may take 2-3 days depending on your bank.

<details>
<summary>Detailed example</summary>
<div>
Here is an example HTTP POST:

[`POST:/epayment/v1/payments/{reference}/capture`](https://developer.vippsmobilepay.com/api/epayment/#tag/AdjustPayments/operation/capturePayment)

With body:

```json
{
  "modificationAmount": {
    "value": 49900,
    "currency": "NOK"
  }
}
```
</div>
</details>

![Money in the bank](./images/money_bag.png)

## Sequence diagram

Sequence diagram for the standard online payment flow.

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant ePayment as ePayment API
    participant ordermanagement as Order Management API
    M->>C: Get payment method
    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    ePayment->>ePayment: Reserve payment
    ePayment->>M: Callback with status
    M->>C: Display order confirmation
    M->> ordermanagement: Attach receipt
    ordermanagement->>C: Provide receipt
    M->>C: Ship the order
    M->>ePayment: Capture the payment
```
