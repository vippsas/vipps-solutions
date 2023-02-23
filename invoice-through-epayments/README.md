<!-- START_METADATA
---
title: Invoicing through ePayment
---
END_METADATA -->

# Invoicing through ePayment

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->


💥 Work in progress 💥

You can use Vipps to send invoices to your customers! This is possible by combining requests for the
[ePayment](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api)
and
[Order Management](https://vippsas.github.io/vipps-developer-docs/docs/APIs/order-management-api) APIs.

## First-time payment of an invoice

You can set up invoicing through the Vipps APIs by following these steps.

1. In your website or mobile app, provide your customers with an option for opting-in to receive
   invoices in the Vipps app.
1. Present them also with the *Pay with Vipps* option where they view their invoice in your website or app.
1. When they select *Pay with Vipps*, send the
   [create payment](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments)
   request.

   Request access to the user' phone number by including the `scope` parameter with a value of `phoneNumber`.


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
      "returnUrl":"https://example.com/redirect?orderId=1512202",
      "userFlow":"WEB_REDIRECT"
   }
   ```

1. If the customer is on a desktop computer, the
   [Vipps Landing page](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/vipps-landing-page)
   opens. If on a mobile device, the Vipps app opens.
1. The customer consents to share their phone number.
1. The customer approves the payment in the Vipps app.

   These steps can be visualized as:

   ![First time payment of an invoice](images/first-time-invoice-payment.png)

1. To retrieve the user's phone number, start by making a request to
   [Get Payment](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/QueryPayments/operation/getPayment) to get the `sub` value.
1. Then, make a request to
   [getUserinfo](https://vippsas.github.io/vipps-developer-docs/api/userinfo#operation/getUserinfo).
   For details and more examples, see the
   [UserInfo API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/userinfo-api).

1. Finally, use the customer's phone number to send them direct payment requests for subsequent invoices.



## Subsequent invoice payments

Once you have the customer's phone number and their approval to send invoices through Vipps, you can send
them invoices directly. Your invoices should include an image or a link to a web view where they can get more details about the charges.

1. Start by adding the invoice data, such as images or links to a web view, to a payment through the
   [Order Management](https://vippsas.github.io/vipps-developer-docs/docs/APIs/order-management-api) API.

   Send the
  [Add category to an order](https://vippsas.github.io/vipps-developer-docs/api/order-management#operation/putCategoryV2)
  request.
  Note that the request path allows for different `{paymentType}` options. In this case, it must be set  to `ecom`.
  For example: `https://api.vipps.no/v2/ecom/categories/{orderId}`.

   Here is an example of a valid request body:

   ```json
   {
   "category": "GENERAL",
   "orderDetailsUrl": "https://example.com/orderdetails/acme-shop-123-order123abc"
   }
   ```

1. Make a
   [create payment](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments)
  request where the following are set:
    - `reference` - The `orderId` used in Step 1.
    - `expiresAt` - The expiration date for the payment.
    - `userFlow`  - Must be `"PUSH_MESSAGE"`.
    - `customer.phoneNumber` - The customer's phone number.


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
      "returnUrl":"https://example.com/redirect?orderId=1512202",
      "userFlow":"PUSH_MESSAGE",
      "expiresAt":"2023-09-15T00:00:00Z"
   }
   ```

1. The customer will receive a push notification in their Vipps app.
1. When the customer selects `See details` in the payment confirmation screen, they are presented with the order information provided by the merchant.
   If they are provided with a link, they can tap this to view the invoice data in a web view without leaving the Vipps app.
1. The customer approves the payment.

For example, the sequence might look like this:
![Subsequent payment of an invoice](images/subsequent-invoice-payment.png)

The invoice payments must have extended expiration dates, as specified in the
[create payment](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments) request.

Users can soft dismiss this payment
by clicking `Cancel` -> `I'll pay later` and come back into the Vipps app to pay at a later time.

For more information about extended expiration dates, see [Extended expiration for payments to merchants](long-expiry-time-for-payments-to-merchants).
