<!-- START_METADATA
---
title: Longer expiry time
sidebar_position: 48
---
END_METADATA -->

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

# Longer expiry time

💥 WIP 💥

The expiration time for an ecommerce payment is 10 minutes. The payment requests will timeout after that. Read about timeouts here [Online payment timeout](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/common-topics/timeouts)

Scenarios like paying toll or paying at the doctor's office require payments to be active for more than 10 minutes to 24 hours (in some cases even longer).

Using [ePayments API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/epayment-api), merchants can specify the expiration time of a payment as part of [create payment API](https://vippsas.github.io/vipps-developer-docs/api/epayment#tag/CreatePayments)

For a merchant to be able to set the `expiresAt` property in the API request, the Merchant Serial Number needs to be allowed. If this is not set, the following error is returned. Please contact your KAM and we will fix it for you.

```
{
    "type": "",
    "title": "Long living Ecom not allowed",
    "status": 400,
    "detail": "Merchant serial number xxxxxx is not allowed to perform long living Ecom transactions",
    "instance": "/v1/payments/",
    "traceId": "00-631d10d7ee147e46941a0725ed2dcd6a-54bda84425e20140-01"
}
```

Currently, merchant can set `expiresAt` value between anything greater than 10 minutes to less than 28 days (40320 minutes) in the future.

```
{
    "type": "",
    "title": "Expiration date invalid",
    "status": 400,
    "detail": "'ExpiresAt' must be greater than 10 minutes and less than 40320 minutes into the future",
    "instance": "/v1/payments/",
    "traceId": "00-ac8df7e1a88947428e58ac5428b0bcac-7f5f22d3814edcf5-01"
}
```

A valid Create payment request with `expiresAt` property like below will send a push message to the specified Vipps user. 

```
{
    "amount": {
        "currency": "NOK",
        "value": 2000
    },
    "customer": {
        "phoneNumber": 4791234567
    },
    "paymentMethod": {
        "type": "wallet"
    },
    "reference": "{{_reference}}",
    "returnUrl": "https://example.io/redirect?orderId=1512202",
    "userFlow": "PUSH_MESSAGE",
    "expiresAt": "2023-02-15T00:00:00Z"
}

```

If the merchant is not aware of the phone number of the user, they could use the `userFlow: "QR"`. This will return the QR code for a payment with the expiration time set by the merchant. Users could scan and pay. 

```
{
    "amount": {
        "currency": "NOK",
        "value": 2000
    },
    "customer": {
        "phoneNumber": 4791234567
    },
    "paymentMethod": {
        "type": "wallet"
    },
    "reference": "{{_reference}}",
    "returnUrl": "https://example.io/redirect?orderId=1512202",
    "userFlow": "PUSH_MESSAGE",
    "expiresAt": "2023-02-15T00:00:00Z"
}
```


The user clicks on the notification or scans the QR code to complete the payment flow in the app.

![Payment flow in the app](images/Long-expiry-time-payment-request.png)



The user can soft dismiss this payment by clicking `Cancel` -> `I'll pay later`. This option will only be shown to payments that have expiresAt property set by the merchant.

![Soft dismiss a payment](images/Soft-dismiss.png)



Soft dismissed payment will be available in the App until expiry for the user to pay. Vipps will send reminder to users who have a soft dismissed a payment and that payment is about to expire.

![Soft dismiss a payment](images/Soft-dismissed-payment-in-home-screen.png)