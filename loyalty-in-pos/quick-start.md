<!-- START_METADATA
---
title: Quick start with Loyalty in POS
sidebar_label: Quick start
sidebar_position: 2
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Quick start

This is the step-by-step guide for how to implement _Loyalty at the POS_.

## Step 1 - Scan the QR code

When scanning the QR code, you will get a URL like this:

```text
https://qr.vipps.no/28/2/01/031/4791234567?v=1
```

In this example, the customer's phone number is +4791234567, where +47 is the country code, entered without the `+`.

Store the number in the POS, as it will be used in subsequent steps.

## Step 2 - POS check membership

Now that you have the customer's phone number, check your internal systems if this user is a member or not. After that check is completed, trigger a check in using the [Vipps Check-in API](https://developer.vippsmobilepay.com/docs/APIs/check-in-api) to let the customer know whether or not they are a member.

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

Let's continue the scenario where the user isn't a member already. You will then set `isMember` to false and use the
[Vipps Login API](https://developer.vippsmobilepay.com/docs/APIs/login-api)
 to enroll the user.

## Step 3 Initiate a login

To send a login request to a user, you will need to use the
[CIBA flow](https://developer.vippsmobilepay.com/docs/APIs/login-api/api-guide/flows/phone-number-ciba-flows)
from the Login API. The steps needed to get a consent from the user are explained in detail there. The CIBA flow will send a push to the user, and once the user has finished the flow, it should be reflected in the POS.

## Step 4 Initiate a payment

Once membership status is confirmed, all wares are scanned, and all discounts are added, it is time to send a payment request to the user.
This is done by sending a payment-push using the
[create payment endpoint](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments/operation/createPayment)
in the [ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api).

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
