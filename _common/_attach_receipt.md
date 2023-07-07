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

Body:

```json
{
  "orderLines": [
    {
      "name": "socks",
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
      "isRefund": false,
      "isShipping": false
    },
    {
      "name": "flip-flops",
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
      "isRefund": false,
      "isShipping": false
    },
        {
      "name": "Home delivery",
      "id": "shipping_1",
      "totalAmount": 1000,
      "totalAmountExcludingTax": 1000,
      "totalTaxAmount": 0,
      "taxPercentage": 0,
      "discount": 0,
      "isRefund": false,
      "isShipping": true
    }
  ],
  "bottomLine": {
    "currency": "NOK",
    "tipAmount": 0,
    "giftCardAmount": 0,
    "terminalId": "vipps_pos_122"
  }
}
```

</div>
</details>
