<!-- START_METADATA
---
title: Vipps MobilePay in-store with customer club flow
sidebar_label: In-store with customer club
description: Using Vipps MobilePay in a physical setting with customer club
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# In-store with customer club

This solution combines multiple Vipps products and makes a great product-market fit for
retail stores that want to combine loyalty with payments.
It's a great way to improve the payment experience for customers and simplify the process
of adding customers to the merchant loyalty club.

The solution is a combination of the personal QR codes in the Vipps app,
the
[Login API](https://developer.vippsmobilepay.com/docs/APIs/login-api),
the
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api),
and the
[Check-in API](https://developer.vippsmobilepay.com/docs/APIs/check-in-api).

The following describes the process at a high level.

![Loyalty Flow](images/POS_flow.png)

## Step 1: Scan the QR code

The flow begins with the customer presenting their QR code to the merchant. This can happen in two ways:

* Customer-facing scanner - The store will have a permanent customer-facing scanner and customers can scan their QR code at any time.
* Cashier scanner - The QR code is scanned by the cashier using a wired scanner. This could happen while the cashier is scanning wares or immediately before the payment.

![Loyalty Flow](images/POS_step_1.png)

The customer's personal QR code contains a URL like this:
`https://qr.vipps.no/28/2/01/031/4791234567?v=1`, where `4791234567` is their phone number in
[MSISDN](https://en.wikipedia.org/wiki/MSISDN) format.

When this QR code is scanned in the store, the POS will get their phone number.
This can be used for checking membership, in the next step.

## Step 2: Check membership

In your internal system, check the customer's membership status by using the phone number you received in the previous step.

Use the
[`POST:/v1/loyalty-check-in`](https://developer.vippsmobilepay.com/api/check-in#tag/Loyalty-check-in)
endpoint to trigger a "waiting screen" in the app. This will inform the customer whether
they are a member of your loyalty program and otherwise help them through the payment process.

Here is an example of the HTTP POST you can use:

[`POST:/point-of-sale/v1/loyalty-check-in`](https://developer.vippsmobilepay.com/api/check-in#tag/Loyalty-check-in/operation/initiateLoyaltyCheckIn)

With body:

```json
{
    "phoneNumber": "4791234567",
    "loyaltyProgramName": "Acme loyalty club",
    "isMember": false
}
```

If the customer is a not member, proceed to step 3 where you can enroll them by using the
[Login API](https://developer.vippsmobilepay.com/docs/APIs/login-api).

If they are already a member, skip to step 4.

![Loyalty Flow](images/POS_step_2.png)

## Step 3: Request membership (optional)

If the customer is not a member of the loyalty program, you can request to enroll them by using
the [Vipps Login API](https://developer.vippsmobilepay.com/docs/APIs/login-api).

You already have their phone number from step 1, so just provide a button in
your user interface to allow the cashier to initiate the login.

Pressing the button will trigger a
[Login flow](https://developer.vippsmobilepay.com/docs/APIs/login-api/api-guide/flows/phone-number-ciba-flows)
to gather consent from the customer. The steps needed to get consent from the user are explained in detail there.
The CIBA flow will send a push to the user, and once the user has finished the flow, it should be reflected in the POS.

When this login flow is completed, the customer will be enrolled in the loyalty program.

![Loyalty Flow](images/POS_step_3.png)

## Step 4: Send a payment request

After membership status has been determined and all wares have been scanned, send a payment request to the customer.

You already have the phone number from step 1, so you don't need to ask for it again.
Just provide a button in your user interface to allow the cashier to send the payment request.

Your system can send the payment request by using the
[create payment endpoint](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment).

Set `userFlow` to `PUSH_MESSAGE`. This will send a push directly to the customer who scanned the QR code, and after the payment is completed, the POS will be updated with the status of the payment.

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
  "userFlow": "PUSH_MESSAGE",
  "returnUrl": "http://example.com/redirect?reference=2486791679658155992",
  "paymentDescription": "Purchase of socks"
}
```

A notification will appear on the customer's Vipps app.

Once they authorize the payment, the POS will be updated with the status.

![Loyalty Flow](images/POS_step_4.png)
