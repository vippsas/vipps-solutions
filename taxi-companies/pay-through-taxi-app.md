<!-- START_METADATA
---
sidebar_position: 102
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Payment through company app

The customer pays the taxi company from their app when ordering the taxi and selects to pay with Vipps.
The amount is reserved until the final amount is known, at which time the payment is captured.

![Taxi route](images/taxi_route.png)

This flow combines multiple products to illustrate the recommended online payment flow.

## Details

### Step 1. Get the customer's the payment method

Display an option to pay with Vipps on your app.

### Step 2. Initiate a payment request to Vipps

Reserve an amount large enough to cover the payment Vipps.

The app automatically switches over to the Vipps app.

Set `userFlow` to `WEB_REDIRECT`, so the customer's browser will either do an automatic app-switch or open the landing page to confirm the mobile number.
</div>
</details>

### Step 3. The customer authorizes the payment

The customer's Vipps should open automatically, with the maximum reservation amount visible.
They can then confirm the payment.

To get confirmation that payment was approved, monitor
[webhooks](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api) and
[query the payment](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment).

### Step 4. Confirm the order

Upon authorization, the Vipps app should automatically redirect the customer to your app.
Confirm that the order has been successful in your app.

### Step 5. Add a receipt

After the drive is complete, calculate how much the customer owes and provide a receipt.
Add a payment receipt to the order by using the
[`postReceipt`](https://developer.vippsmobilepay.com/api/order-management/#operation/postReceiptV2)
endpoint.

This will appear in their app.

See
[Adding a receipt](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/vipps-order-management-api/#adding-a-receipt)
for more details.

### Step 7. Capture the amount due

[Capture](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/capturePayment)
the amount due before releasing the remaining reserved amount on the customer's account.

Release the remaining about by sending a
[cancel](https://developer.vippsmobilepay.com/api/epayment#tag/AdjustPayments/operation/cancelPayment) API request.

Check the status of the captured payment.

Read more about the flow:

* [ePayment how it works guide](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/how-it-works/vipps-epayment-api-how-it-works-online)
* [Recommended flow for online payments](../online/README.md)

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
    M->>ordermanagement: Attach receipt showing amount due
    M->>ePayment: Check the status of capture
```
