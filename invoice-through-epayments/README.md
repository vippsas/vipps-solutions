<!-- START_METADATA
---
title: Payment request for invoices
sidebar_label: Payment request for invoices
sidebar_position: 120
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Payment request for invoices

<!-- START_COMMENT -->

ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/).

<!-- END_COMMENT -->

💥 Work in progress 💥

Use Vipps to request payment from your customers for an invoice by combining requests for the
[ePayment](https://developer.vippsmobilepay.com/docs/APIs/epayment-api)
and
[Order Management](https://developer.vippsmobilepay.com/docs/APIs/order-management-api) APIs.

This section will explain how to implement for different scenarios.

## Customer's phone number is known

When the merchant has the customer's phone number and their approval to send a payment request through Vipps, they can send payment requests for invoices directly.

Please note:

* The invoices should a link to a web view where the customer can get more details about the charges.
* The invoices must be hosted by the merchant.

1. Start by adding a web view link to a payment by using the
   [Order Management](https://developer.vippsmobilepay.com/docs/APIs/order-management-api) API.

   Send the [Add category to an order](https://developer.vippsmobilepay.com/api/order-management#operation/putCategoryV2) request. Note that the request path allows for different
   `{paymentType}` options. In this case, `{paymentType}` must be set  to `ecom`.
   For example: `https://api.vipps.no/v2/ecom/categories/{orderId}`.

   Here is an example of a valid request body:

   ```json
   {
   "category": "GENERAL",
   "orderDetailsUrl": "https://example.com/orderdetails/acme-shop-123-order123abc"
   }
   ```

1. Make a [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

   Here is an example of a valid request body:

   ```json
   {
      "amount":{
         "currency":"NOK",
         "value":2000
      },
      "customer":{
         "phoneNumber":4791234567
      },
      "paymentMethod":{
         "type":"wallet"
      },
      "reference":"acme-shop-123-order123abc",
      "paymentDescription": "Invoice# 424243, due date: 01 Jan 2025",
      "returnUrl":"https://example.com/redirect?orderId=1512202",
      "userFlow":"PUSH_MESSAGE",
      "expiresAt":"2023-09-15T00:00:00Z"
   }
   ```

    For this solution, the following parameters must be set:

      * `reference` - The `orderId` used in Step 1.
      * `expiresAt` - The expiration date for the payment.
      * `userFlow`  - Must be `"PUSH_MESSAGE"`.
      * `customer.phoneNumber` - The customer's phone number.
      * `paymentDescription` - Short description with relevant information about the invoice.

1. The customer will receive a push notification in their Vipps app.
1. When the customer selects `See details` in the payment confirmation screen, they are presented with the order information provided by the merchant.
   In this scenario, they can tap this to view the invoice data hosted by the merchant in a web view without leaving the Vipps app.
1. The customer approves the payment.

   For example, the sequence might look like this:
   ![Subsequent payment of an invoice](images/subsequent-invoice-payment.png)

   Invoice payments must have extended expiration dates, as specified in the
   [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

   Users can soft dismiss this payment
   by clicking `Cancel` -> `I'll pay later` and come back into the Vipps app to pay at a later time.

   For more information about extended expiration dates, see [Extended expiration for payments to merchants](../long-expiry-time-for-payments-to-merchants/README.md).


## One time payment request

   When a merchant does not know the phone number of the user and want to send a one-time request for payment for an invoice, they can use the Vipps API by following these steps.

1. In your website, mobile app, on paper document or email you send, provide your customers with an option for opting-in to receive payment request for invoices in the Vipps app.
1. Present them with the *Pay with Vipps*  option.
1. When they select *Pay with Vipps*, start by adding the invoice link to a web view, to a payment through the
   [Order Management](https://developer.vippsmobilepay.com/docs/APIs/order-management-api) API.

   Send the [Add category to an order](https://developer.vippsmobilepay.com/api/order-management#operation/putCategoryV2) request. Note that the request path allows for different `{paymentType}` options. In this case, it must be set  to `ecom`. For example: `https://api.vipps.no/v2/ecom/categories/{orderId}`.

   Here is an example of a valid request body:

   ```json
   {
   "category": "GENERAL",
   "orderDetailsUrl": "https://example.com/orderdetails/acme-shop-123-order123abc"
   }
   ```

1. Send the [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

   Request access to the user' phone number by including the `scope` parameter with a value of `phoneNumber`. In the `paymentDescription` field provide relevant information about the invoice.

   Here is an example of a valid request body:

   ```json
   {
      "amount":{
         "currency":"NOK",
         "value":2000
      },
      "paymentMethod":{
         "type":"wallet"
      },
      "reference":"acme-shop-123-order123abc",
      "paymentDescription": "Invoice# 424242, due date: 31 Dec 2024",
      "returnUrl":"https://example.com/redirect?orderId=1512202",
      "userFlow":"WEB_REDIRECT"
   }
   ```
   For this solution, the following parameters must be set:

      * `reference` - The `orderId` used in Step 3.
      * `paymentDescription` - Short description with relevant information about the invoice.

1. If the customer is on a desktop computer, the
   [Vipps Landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page)
   opens. If on a mobile device, the Vipps app opens.
1. The customer approves the payment in the Vipps app.

   These steps can be visualized as:

   ![One-time request for payment](images/one-time-payment-of-invoice.png)



## Customer's phone number is not known

When a merchant does not know the phone number of the user, they can request payment through the Vipps APIs by following these steps.

1. In your website, mobile app, on paper document or email you send, provide your customers with an option for opting-in to receive payment request for invoices in the Vipps app.
1. They view their invoice in your website or app.
1. Present them also with the *Pay with Vipps* option.
1. When they select *Pay with Vipps*, send the [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

   Request access to the user' phone number by including the `scope` parameter with a value of `phoneNumber`. In the `paymentDescription` field provide relevant information about the invoice.

   Here is an example of a valid request body:

   ```json
   {
      "amount":{
         "currency":"NOK",
         "value":2000
      },
      "customer":{
         "phoneNumber":4791234567
      },
      "paymentMethod":{
         "type":"wallet"
      },
      "profile":{
         "scope":"phoneNumber"
      },
      "reference":"acme-shop-123-order123abc",
      "paymentDescription": "Invoice# 424242, due date: 31 Dec 2024",
      "returnUrl":"https://example.com/redirect?orderId=1512202",
      "userFlow":"WEB_REDIRECT"
   }
   ```

1. If the customer is on a desktop computer, the
   [Vipps Landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page)
   opens. If on a mobile device, the Vipps app opens.
1. The customer consents to share their phone number.
1. The customer approves the payment in the Vipps app.

   These steps can be visualized as:

   ![First time payment of an invoice](images/first-time-invoice-payment.png)

1. To retrieve the user's phone number, start by making a request to
   [Get Payment](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment) to get the `sub` value.
1. Then, make a request to
   [getUserinfo](https://developer.vippsmobilepay.com/api/userinfo#operation/getUserinfo).
   For details and more examples, see the
   [UserInfo API](https://developer.vippsmobilepay.com/docs/APIs/userinfo-api).

1. Finally, use the customer's phone number to send them direct payment requests in the future.
