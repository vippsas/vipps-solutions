<details>
<summary>General create request example</summary>
<div>

Here is an example HTTP POST:

[`POST:/epayment/v1/payments`](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)

With body:

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

</div>
</details>
