---
title: How online payment works
sidebar_label: Online payment
sidebar_position: 10
---

# How online payment works

This is how payments in web shops or merchant websites work using the ePayment and order management API.


## 1. Pay with Vipps

The user chooses “Pay with Vipps”, on the product page of a merchant’s website or app.

Your system can send the payment request by using the
[create payment endpoint](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment).

Set `userFlow` to `WEB_REDIRECT` and users browser will either do an automatic appswitch or open the landing page to confirm the mobile number.

Here is an example of the HTTP POST you can use:

[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)

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


![Pay with Vipps](../images/vipps-ecom-step1-2.png)

## 2. The Vipps landing page (If customer started on desktop)

If the payment was started on a desktop device the user will be sent to the Vipps landing page.
The user confirms their number, and is prompted to log in to Vipps.

If the payment was started from a mobile device, the app will automatically switch over to Vipps.

![Vipps landing page](../images/vipps-ecom-step2.svg)

## 3. Confirm payment in Vipps

The user receives a push notification on their phone. They log in to Vipps, and confirm the payment.
The payment is reserved, and the user gets a receipt of the successful payment

![Confirm payment](../images/vipps-ecom-confirm2.png)

## 4. Order Confirmation

The user is redirected back to the merchant’s store, and the order is confirmed.

![Order confirmation](../images/vipps-ecom-step4.png)

## 5. Add order receipts

After payment, enrich the users Vipps app with a receipt from the payment.

This postReceipt endpoint [`POST:/order-management/v2/{paymentType}/receipts/{orderId}`](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2) is for sending receipt information. Receipt information is a combination of order lines and a bottom line with sum and VAT. An OrderLine is a description of each item present in the order. 

![Order receipt](../images/order-receipt.png)

## 6. Completing the order and shipping

The merchant completes the order, and ships the order to the customer.

![Shipping](../images/vipps-shipping.svg)

## 7. Money in the bank

The payment is transferred to the merchant’s account. This may take 2-3 days depending on your bank.

![Money](../images/vipps-money.svg)
