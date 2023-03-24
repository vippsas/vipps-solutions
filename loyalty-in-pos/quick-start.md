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

```HTTP
https://qr.vipps.no/28/2/01/031/4791234567?v=1
```

In this example, the customer's phone number is +4791234567, where +47 is the country code, entered without the `+`.

Store the number in the POS, as it will be used in subsequent steps.

## Step 2 - POS check membership

Now that you have the customer's phone number, check your internal systems if this user is a member or not. After that check is completed, trigger a check in using the [Vipps Check-in API](https://github.com/vippsas/vipps-check-in-api) to let the customer know whether or not they are a member.

Here is an example of the HTTP POST you can use:

[`POST:/point-of-sale/v1/loyalty-check-in`](https://vippsas.github.io/vipps-developer-docs/api/check-in#tag/Loyalty-check-in/operation/initiateLoyaltyCheckIn)

With body:

```json
{
    "phoneNumber": "4791234567",
    "loyaltyProgramName": "Acme loyalty club",
    "isMember": false
}
```

Let's continue the scenario where the user isn't a member already. You will then set `isMember` to false and use the
[Vipps Login API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/login-api)
 to enroll the user.

## Step 3 Initiate a login

To send a login request to a user, you will need to use the
[CIBA flow](https://vippsas.github.io/vipps-developer-docs/docs/APIs/login-api/api-guide/flows/phone-number-ciba-flows)
from the Login API. The steps needed to get a consent from the user are explained in detail there. The CIBA flow will send a push to the user, and once the user has finished the flow, it should be reflected in the POS.

## Step 4 Initiate a payment

Once membership status is confirmed, all wares are scanned, and all discounts are added, it is time to send a payment request to the user.
This is done by sending a payment-push using the
[`skiplandingpage` parameter in the eCom API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#skip-landing-page).

Here is an example of the HTTP POST you can use:

[`POST:/ecomm/v2/payments`](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#skip-landing-page)

With body:

```json
{
  "customerInfo": {
    "mobileNumber": "4791234567"
  },
  "merchantInfo": {
    "merchantSerialNumber": "123456",
    "callbackPrefix": "https://example.com/vipps/callbacks-for-payment-update-from-vipps",
    "fallBack": "https://example.com/vipps/fallback-result-page-for-both-success-and-failure/acme-shop-123-order123abc"
  },
  "transaction": {
    "orderId": "acme-shop-123-order123abc",
    "amount": 20000,
    "transactionText": "One pair of Vipps socks",
    "skipLandingPage": true
  }
}
```

The `skipLandingPage` flag will send a push directly to the customer who scanned the QR code, and after the payment is completed, the POS will be updated with the status of the payment.
