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

## Step 1. Pay with Vipps or MobilePay

The user chooses *Pay with Vipps* or *Pay with MobilePay*, on the product page of your website or app.

Your system can send the payment request by using the
[create payment endpoint](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/create/).

Set `userFlow` to `WEB_REDIRECT` and users browser will either do an automatic app-switch or open the landing page to confirm the mobile number.

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
</div>
</details>

![Pay with Vipps MobilePay](images/vipps-ecom-step1-2.png)

## Step 2. The landing page (If customer started on desktop)

If the payment was started on a desktop device, the user will be sent to the
[Vipps MobilePay landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page/).
The user confirms their number and is prompted to log in to Vipps MobilePay.

If the payment was started from a mobile device, the app automatically switches over to the app.

![Vipps MobilePay landing page](images/vipps-ecom-step2.svg)

## Step 3. Confirm payment

The user receives a push notification in their Vipps or MobilePay app. They log in and confirm the payment.
The payment is reserved, and the user gets a receipt of the successful payment.

![Confirm payment](images/vipps-ecom-confirm2.png)

## Step 4. Order Confirmation

The user is redirected back to your store, and the order is confirmed.

![Order confirmation](images/vipps-ecom-step4-2.png)

## Step 5. Add order receipts

After payment, add a payment receipt. This will appear in the Vipps or MobilePay app.

This `postReceipt` endpoint,
[`POST:/order-management/v2/ecom/receipts/{reference}`](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2),
is for sending receipt information.
This is a combination of *order lines* and a *bottom line* with sum and VAT.
An *order line* is a description of each item present in the order.

![Order receipt](images/order-receipt.png)

## Step 6. Completing the order and shipping

The merchant completes the order and ships the order to the customer.
Finally, capture the payment using the
[Capture endpoint](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture/).

<details>
  <summary>Detailed example</summary>
  <div>
    Here is an example HTTP POST:

    [`POST:/epayment/v1/payments/{reference}/capture`](/api/epayment/#tag/AdjustPayments/operation/capturePayment)

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

![Shipping](images/vipps-shipping.png)

## 7. Money in the bank

The payment is transferred to your account. This may take 2-3 days depending on your bank.

![Money](images/money_bag.png)
