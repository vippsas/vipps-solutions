<!-- START_METADATA
---
title: EV Charging
sidebar_position: 5
---
END_METADATA -->

# Electric vehicle charging

Vipps is an excellent choice for EV charging as practically all Norwegians have
the Vipps app in their pocket, simplifying charging with out downloading a
specific charging app.

Implemented correctly, EV charging with Vipps, is a simple and efficient
solution that let's your customers use your charging network with no hassle.

![EV charging with Vipps](images/ev-charging-process-icons.png)

Drop in charging with Vipps is best implemented using QR codes, scanned either
with the Vipps app or the phone's camera. Vipps provides a
[QR API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api)
that can be used in combination with the [Vipps eCom API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/) to set up and start a
Vipps payment.

## User experience

![EV charging with Vipps: Screenshots](images/ev-charging-process-screenshots.png)

1. User scans a QR code on the charging station. Both the camera and the QR scanner in Vipps may be used.
2. User is redirected to a website provided by the charging provider. The user selects Vipps as payment option for charging and clicks "Pay with Vipps"
3. Vipps is automatically opened, with the max /reservation amount visible. User confirms payment.
4. User is redirected back to the charging providers website website where a status of the charge session is presented. The user can stop charging from this screen or from the charging station's user interface.
5. When the charging is complete: User gets a push message stating the final sum that will be deducted from user's account.
6. A receipt with the payment details is available in Vipps.

## Charging operator recommendations
As the charging provider we recommend that your technical flow resembles the steps below:

1. The QR code on the charging station contains a link to a website provided by the charging provider. It is also recommended that the particular charging station / charging point is identified in the QR code. QR codes should be Vipps-branded. This is achieved by generating QR codes using the [QR API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api). 
2. The website the user lands on should contain payment options in addition to terms & conditions. If the QR code contained an identification of the charging point the user does not have to type in any identification code to start charging. It is also possible to let the user choose max amount / reserved amount.
3. When the user clicks "Pay with Vipps" you should [initiate](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#initiate) a payment with an amount that will cover most charging sessions. Usually between 350 NOK and 500 NOK is sufficient but with higher electricity cost this may change.  If payment is approved by the user, this amount will be reserved on users account.
4. To get confirmation that payment was approved, you should both listen to  [callbacks](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#callbacks) and poll the [details API-call](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#get-payment-details). Once you know that payment was approved you can start charging.
5. When the charging session is completed you will know the amount the user should pay and you can [capture](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#capture) the amount due. After you have captured the amount due, you should release the remaining reserved amount on the user's account. This is done by doing a [cancel](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api/vipps-ecom-api#cancelling-a-partially-captured-order) API-call.
6. If you are set up in Vipps' systems with the correct MCC (Merchant Category Code) for EV charging (5552) we will automatically send a push notification to the user with the captured amount.
7. We recommend sending a digital reciept and a hyperlink to the charging session after charging is done. To do this the [Order Management API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/order-management-api/) is used.  

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-solutions/issues),
a [pull request](https://github.com/vippsas/vipps-solutions/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
