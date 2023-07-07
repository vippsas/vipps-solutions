The
[`postReceipt`](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2) endpoint
allows you to send a
[receipt](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api/#receipts)
to the customer's Vipps MobilePay app.

Receipt information is a combination of *order lines* and a *bottom line*, with sum and VAT.
An order line is a description of each item present in the order.

<details>
<summary>Detailed example</summary>
<div>

Here is an example HTTP POST:

[`POST:/order-management/v2/{paymentType}/receipts/{orderId}`](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2)

Body:

```json
{
  "orderLines": [
    {
      "name": "socks",
      "id": "line_item_1",
      "totalAmount": 10000,
      "totalAmountExcludingTax": 8000,
      "totalTaxAmount": 2000,
      "taxPercentage": 25,
      "unitInfo": {
        "unitPrice": 4000,
        "quantity": "2",
        "quantityUnit": "PCS"
      },
    },
  ],
  "bottomLine": {
    "currency": "NOK",
    "posId": "vipps_pos_122"
  }
}
```

</div>
</details>
