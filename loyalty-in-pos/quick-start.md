<!-- START_METADATA
---
title: Quick Start
sidebar_position: 1
---
END_METADATA -->

# Vipps Loyalty in POS step by step guide

This is the step-by-step guide for how to implement _Loyalty in POS_.

## Step 1 - Scan the QR code
When scanning the QR code, you will get a url like this:
```
https://qr.vipps.no/28/2/01/031/4791504800?v=1
```
The phonenumber of the user in this case is `4791234567`. This will have to be saved by the POS, as it will be used a lot more.


## Step 2 - POS check membership
Once you got the phonenumber from step 1, you need to check your internal systems if this user is a member or not. After that check is completed, trigger a check in using the vipps [check-in-api](https://github.com/vippsas/vipps-check-in-api)

[`POST:https://api.vipps.no/point-of-sale/v1/loyalty-check-in`](https://vippsas.github.io/vipps-check-in-api/redoc.html#tag/point-of-sale/operation/initiateLoyaltyCheckIn)
```
{
    "phoneNumber": "4791234567",
    "loyaltyProgramName": "Acme loyalty club",
    "isMember": false
}
```
Lets continue the scenario where the user isn't a member already. You will then set isMember to false and use Vipps Login to enroll the user.

## Step 3 Initiate a login
To send a login request to a user, you will need to use the [CIBA flow](https://vippsas.github.io/vipps-developer-docs/docs/APIs/login-api/vipps-login-api#activating-vipps-login-from-phone-number)
from the login api. The steps needed to get a consent from the user is explained in detail there. The CIBA flow will send a push to the user, and once the user has finished the flow it should be reflected in the POS.

## Step 4 Initiate a ecom payment
Once membership status is confirmed, all wares are scanned and all discounts are added, it is time to send a payment request to the user. This is done by sending a payment-push to the user using the skiplandingpage flag on the [ecomApi](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#skip-landing-page)

[`POST:https://api.vipps.no/ecomm/v2/payments`](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#skip-landing-page)

```
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

The skipLandingPage flag will send a push directly to the customer (the one that scanned the QR code), and after the payment is completed by the user you should be all set!

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
