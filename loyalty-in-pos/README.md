<!-- START_METADATA
---
title: Loyalty in POS
sidebar_position: 1
---
END_METADATA -->

# Vipps Loyalty in Point-Of-Sale

<!-- START_TOC -->

## Table of Contents

* [How it works](#how-it-works)
* [Step 1: Scan the QR code](#step-1-scan-the-qr-code)
* [Step 2: POS check membership](#step-2-pos-check-membership)
* [Step 3: Initiate a login (optional)](#step-3-initiate-a-login-optional)
* [Step 4: Initiate a eCom payment](#step-4-initiate-a-ecom-payment)
* [Questions?](#questions)

<!-- END_TOC -->

## How it works

Vipps Loyalty in point of sale (POS) is a solution that combines multiple Vipps products and makes a great market fit for retail stores that want to combine loyalty with payments. Its a great way to improve the payment experience for users and remove friction for adding customers to the merchant loyalty club.

The solution is a combination of the personal QR codes in the Vipps app,
the
[Vipps Login API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/login-api),
ther Vipps Check-In API
and the
[Vipps eComm API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/ecom-api).

 See the [Quick Start](quick-start.md) guide.

![Loyalty Flow](images/POS_flow.png)

## Step 1: Scan the QR code

The flow begins with the user presenting their QR code to the merchant. This can happen two ways:
 - User-facing scanner. The store will have a permanent user-facing scanner, and the customers can show their QR code to the scanner at any time.
 - The QR code is scanned by the cashier, using a wired scanner. This could happen while the cashier is scanning wares or right before the payment.

![Loyalty Flow](images/POS_step_1.png)

## Step 2: POS check membership

The scanning of the Vipps QR code will give the POS the phone number of the user. The POS should now proceed to check the membership status of this user.
If the user has a membership already, skip to step 4. If not - you can enroll the user in your membership program using Vipps Login (step 3).

At this stage, you should trigger a Vipps Check-In to inform the user if they are members or not of your loyalty club. This will help the users trough the payment process. This should happen automatically by the POS once the QR code is scanned.

![Loyalty Flow](images/POS_step_2.png)

## Step 3: Initiate a login (optional)

Okay - the status here is that the POS knows the phone number of a user - but the user isn't a member of the loyalty program. There should be a button in the POS that the cashier can press that will trigger a Vipps Login flow to gather consent from the user. When this login flow is completed the user will be enrolled in the customer club.

The login flow is explained in detail
[here](https://vippsas.github.io/vipps-developer-docs/docs/APIs/login-api/vipps-login-api#vipps-login-from-phone-number).
After the user has accepted the terms and has been enrolled in the loyalty club, continue to step 4.

![Loyalty Flow](images/POS_step_3.png)

## Step 4: Initiate a eCom payment

Last step - payment. At this stage, the customer has shown his QR code, and membership status has been decided. When that is done and all wares are scanned, it is time to send a payment request to the user. Since the POS already have the number for the user, it should be done by just pressing a button on the POS and a push will appear on the users Vipps app. Once that is completed, the POS will be updated with the status of the payment. Done!

![Loyalty Flow](images/POS_step_4.png)

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-solutions/issues),
a [pull request](https://github.com/vippsas/vipps-solutions/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
