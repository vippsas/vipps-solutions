<!-- START_METADATA
---
title: Vipps MobilePay Payment Requests
sidebar_label: Payment Requests
sidebar_position: 50
description: Using Vipps MobilePay for sending payment requests.
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Payment Requests

Use Vipps MobilePay to make *long-living payment requests* for your customers by using the `"expiresAt"` feature in
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api)
and receipt information from
[Order Management API](https://developer.vippsmobilepay.com/docs/APIs/order-management-api).
This will create payment requests that can be seen and postponed by the user, up to 28 days.

Note: The API is ready, but not testable before app updates that are planned summer 2023.

The following section will explain how to implement this feature for a couple scenarios:

## 1. Payment request sent directly to app

If you have the customer's phone number and their consent to send payment requests through Vipps MobilePay,
you can send payment requests directly to the customer.

The flow for the customer will look like this: ![Payment Request Push flow](images/Payment-request-sent-directly-to-app.png)

1. To create this payment, you first need to make a [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request where `customer.phoneNumber` is set.
2. The customer will receive a push notification in their Vipps or MobilePay app.
3. When the customer selects `See details` in the payment confirmation screen, they are presented with the order information provided by the merchant without leaving the app.
4. The customer approves the payment.

   Users also have the option of soft-dismissing the payment and postponing it for later.

## 2. Payment request as a link

When a merchant does not know the phone number of the user and want to start a payment request, you could send them a link to your own landing page that in turn triggers a payment request through Vipps MobilePay API.

The flow for the customer will look like this: ![Payment Request landing page flow](images/Payment-request-with-link.png)

1. In your website, mobile app, on paper document or email you send, provide your customers with an option for opting-in to receive payment request for payment requests in the Vipps or MobilePay app.
2. Present them with the *Pay with Vipps* or *Pay with MobilePay* option.
3. Send the [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

4. If the customer is on a desktop computer, the
   [Vipps MobilePay Landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page)
   opens. If on a mobile device, the Vipps MobilePay app opens automatically.

## 3. Payment request with sharing of telephone number

The flow for the customer will look like this: ![Payment Request landing page flow with userinfo](images/Payment-request-with-sharing-phone-number.png)

This is very similar as scenario 2, explained above. The difference is that you will also ask the user to share their telephone number. This is done by setting the `scope` parameter with a value of `phoneNumber` in the [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

After the user have finished the payment, you will get the phone number of the customer. This means you can proceed with scenario 1 in the future and send the payment request directly to the customer. There is more info about fetching user data in the [profile sharing](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/features/profile-sharing/) section of epayments.

## General create request example

Example body:

   ```json
   {
      "amount":{
         "currency":"NOK",
         "value":6000
      },
      "customer":{
         "phoneNumber":4791234567
      },
      "paymentMethod":{
         "type":"WALLET"
      },
      "receiptInfo":{
         "orderLines": [
            {
               "name": "MobilePay socks",
               "id": "line_item_1",
               "totalAmount": 1000,
               "totalAmountExcludingTax": 800,
               "totalTaxAmount": 200,
               "taxPercentage": 25,
               "unitInfo": {
               "unitPrice": 400,
               "quantity": "2.5",
               "quantityUnit": "KG"
               },
               "discount": 0,
               "productUrl": "https://example.com/store/socks",
               "isReturn": false,
               "isShipping": false
            },
            {
               "name": "Vipps flip-flops",
               "id": "line_item_2",
               "totalAmount": 5000,
               "totalAmountExcludingTax": 4000,
               "totalTaxAmount": 1000,
               "taxPercentage": 25,
               "unitInfo": {
               "unitPrice": 2500,
               "quantity": "3",
               "quantityUnit": "PCS"
               },
               "discount": 2500,
               "productUrl": "https://example.com/store/flipflops",
               "isReturn": false,
               "isShipping": false
            }
         ],
         "bottomLine": {
            "currency": "NOK",
            "tipAmount": 0,
            "posId": "vipps_pos_122",
            "paymentSources": {
               "giftCard": 0,
               "card": 0,
               "voucher": 0,
               "cash": 0
            },
            "barcode": {
               "format": "CODE 39",
               "data": "SC0527013501 "
            },
            "receiptNumber": "0527013501"
         }
      },
      "reference":"acme-shop-123-order123abc",
      "paymentDescription": "Invoice# 424243, due date: 01 Jan 2025",
      "returnUrl":"https://example.com/redirect?orderId=1512202",
      "userFlow":"PUSH_MESSAGE",
      "expiresAt":"2023-09-15T00:00:00Z"
   }
   ```

To create a *payment request*, the following parameters can/must be used, depending on the scenario:

* `reference` - The `orderId` of the payment request.
* `expiresAt` - The expiration date for the payment. This is what separates the long living payment request from a regular payment.
* `userFlow`  - Must be `"PUSH_MESSAGE"` or `"QR"`.
* `paymentDescription` - Short description with relevant information about the payment request.
* `receiptInfo` (might be renamed)- Order Lines for the payment. The orderlines are the same as referenced in the [Order Management](https://developer.vippsmobilepay.com/docs/APIs/order-management-api) API. This **must** be present.
* `customer.phoneNumber` - The customer's phone number. This is optional, and will be used if the users phone number is known in advance.
* `scope` - This can be used to request the user to share their telephone number.
