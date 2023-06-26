<!-- START_METADATA
---
title: Vipps MobilePay subscriptions flow
sidebar_label: Subscriptions
sidebar_position: 70
description: Simplify subscriptions by using the Login API and Recurring API together.
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Subscriptions

The [Login API](https://developer.vippsmobilepay.com/docs/APIs/login-api) and the [Recurring API](https://developer.vippsmobilepay.com/docs/APIs/recurring-api)
can be used together making registration and payment of subscriptions simple for your customers.

## The process

![Login and recurring process](images/login-recurring-process-v2.svg)

## Step 1. Buy a subscription

A user wants to buy a subscription on a merchant’s website or app.

![Buy subscription](images/login-recurring-step1-v2.svg)

## Step 2. Log in

The user logs in with Vipps MobilePay on the merchant’s site.
If the user is remembered in browser, the login will be completed directly in the browser. If not, the user will be taken to the app to authenticate.

![Log in](images/login-recurring-step2-v2.svg)

## Step 3. Confirm login

If the user needs to authenticate in the app, the user will be taken to the Vipps or MobilePay to confirm the login.

![Confirm login](images/login-recurring-step3.svg)

## Step 4. Give consent to share information

If the user has not consented to sharing information with the merchant earlier the user needs to give this consent.
The user may click "See your information" to see the actual information that will be shared, but this is optional.

![Give consent to share information](images/login-recurring-step4.svg)

## Step 5. Logged in and ready to check out

This step is controlled and designed by the individual merchant. Typically, the user will now be logged in on the merchant’s page, and can proceed to set up the payment for the subscription. The information the user has shared with the merchant is automatically filled in. The merchant can also provide the user with the possibility to edit or add information if necessary.

![Checkout](images/login-recurring-step5-v3.svg)

## Step 6. Accept agreement

The user accepts the agreement in the Vipps or MobilePay app.

![Accept agreement](images/login-recurring-step6-v2.svg)

## 7. Subscription confirmed

The user is returned to the merchant's website or app, and the subscription is confirmed on the merchant's site.

![Confirmation page](images/login-recurring-step7.svg)
