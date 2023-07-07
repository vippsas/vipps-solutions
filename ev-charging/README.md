<!-- START_METADATA
---
title: Electric vehicle charging flow
sidebar_label: Electric vehicle charging
sidebar_position: 80
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import PARTIALCAPTURE from '../_common/_partial_capture.md'
import AUTHORIZEPAYMENT from '../_common/_customer_authorizes_epayment.md'
import ATTACHRECEIPT from '../_common/_attach_receipt.md'
END_METADATA -->

# Electric vehicle charging

Vipps MobilePay is an excellent choice for Electric Vehicle (EV) charging, as practically all Nordic people have
the app on their phone. This removes the need to download a specific charging app.

This is a simple and efficient
solution that enables your customers to use your charging network with no hassle.

![EV charging](images/ev-charging-process-icons.png)

The best drop-in charging flow provides QR codes that can be scanned by
the customer with their phone's camera or the Vipps MobilePay app.

## Details

Combine the [QR API](https://developer.vippsmobilepay.com/docs/APIs/qr-api)
and
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api)
to build this flow.

![EV charging with Vipps: Screenshots](images/ev-charging-process-screenshots.png)

### Step 1. Customer scans the QR code

Generate a Vipps MobilePay QR code that contains the link to your website and the ID of the charging station and place it on the charging station.

The customer scans the QR code and is redirected to your website.

See [Merchant Redirect QR codes](https://developer.vippsmobilepay.com/docs/APIs/qr-api/vipps-qr-api#merchant-redirect-qr-codes)
in the QR API guide for more details about generating the QR code.

### Step 2. Initiate payment request

The website that the customer lands on should contain payment options, in addition to terms and conditions. If the QR code contained an identification of the charging point, the customer doesn't have to type in any identification code to start charging.

It is also possible to let the customer choose maximum amount or reserved amount.

When the customer is ready to pay, initiate a
[payment request](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments).

<details>
<summary>Details</summary>
<div>

The payment request amount should be large enough to cover the cost of a charging session. It is usually sufficient to reserve an amount between 350 NOK and 500 NOK, but with higher electricity costs, this may change.

If the payment is approved, this amount will be reserved on customer's account. The amount that is unused will be released when they are finished charging.
</div>
</details>

### Step 3. Customer approves the payment

The customer's Vipps app should open automatically, with the maximum reservation amount visible.
They can then confirm the payment.

Afterwards, they are redirected back to the charging provider's website, where the status of the charge session is presented.

Once you know that payment was approved you, can start charging.
To get confirmation, monitor
[webhooks](https://developer.vippsmobilepay.com/docs/APIs/webhooks-api) and
[query the payment](https://developer.vippsmobilepay.com/api/epayment#tag/QueryPayments/operation/getPayment).

### Step 4. Start charging

Once the customer has approved the payment, you can start charging.
The customer can stop the charging at any time from your website screen or from the charging station's user interface.

Stop charging when charging is complete or when the customer selects to stop.


### Step 5. Add a receipt

Send a digital receipt and a hyperlink to the charging session after charging is done.

<ATTACHRECEIPT />

### Step 6. Capture the payment

<PARTIALCAPTURE />

If you are set up in Vipps' systems with the correct MCC (Merchant Category Code) for EV charging (5552), we will automatically send a push notification to the customer with the captured amount.


## Sequence diagram

Sequence diagram for the electric vehicle charging payment flow.

``` mermaid
sequenceDiagram
    actor C as Customer
    participant M as Merchant
    participant QR as QR API
    participant ePayment as ePayment API
    participant ordermanagement as Order Management API

    C->>M: Customer scans QR to get to payment page
    M->>ePayment: Initiate payment request
    ePayment->>C: Request payment
    C->>ePayment: Authorize payment
    M->>M: Determine amount due after charging
    M->>ordermanagement: Attach receipt showing amount due
    M->>C: Send a push notification with actual amount paid
    M->>ePayment: Initiate capture request for amount due
    M->>ePayment: Release <amount reserved - amount due>
    ePayment->>C: Capture amount due
    ePayment->>C: Cancel amount remaining
    M->>ePayment: Check the status of capture
    M->>ePayment: Check the status of cancel
```
