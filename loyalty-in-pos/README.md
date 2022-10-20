<!-- START_METADATA
---
title: Introduction
sidebar_position: 1
---
END_METADATA -->

# Vipps Loyalty in Point-Of-Sale

## How it works
Vipps Loyalty in point of sale (POS) is a solution that combines multiple Vipps products and makes a great market fit for retail stores that want to combine loyalty with payments. Its a great way to improve the payment experience for users and remove friction for adding customers to the merchant loyalty club. The solution is a mix of the personal QRs in the Vipps app, Vipps Login, Vipps Check-in and Vipps eCommerce. 
The step-by-step api guide is documented [here](step-by-step.md) 

![Loyalty Flow](images/Lojalitet%20i%20Kassa.png)
### Step 1 - Scan the QR
The flow begins with the user presenting their QR to the merchant. This can happen two ways:
 - User-facing scanner. The store will have a permanent user-facing scanner, and the customers can show their QR to the scanner at any time.
 - The QR is scanned by the cashier, using a wired scanner. This could happen while the cashier is scanning wares or right before the payment. 

//insert image of cashier scanning QR maybe?
### Step 2 - POS check membership
The scanning of the Vipps QR will give the POS the phonenumber of the user. The POS should now proceed to check the membership status of this user.
If the user has a membership already, skip to step 4. If not - you can enroll the user in your membership program using Vipps Login (step 3). 

At this stage, you should trigger a Vipps Check-In to inform the user if they are members or not of your loyalty club. This will help the users trough the payment process. This should happen automatically by the POS once the QR is scanned.

// image something something cashier POS system
// insert image of check in
### Step 3 Initiate a login (optional)
Okay - the status here is that the POS knows the phonenumber of a user - but the user isnt a member of the loyalty program. There should be a button in the POS that the cashier can press that will trigger a Vipps Login flow to gather consent from the user. When this login flow is completed the user will be enrolled in the customer club. 

The login flow is explained in detail [here](https://vippsas.github.io/vipps-developer-docs/docs/APIs/login-api/vipps-login-api#vipps-login-from-phone-number). After the user has accepted the terms and has been enrolled in the loaylty club, continue to step 4.

// insert image of CIBA flow
### Step 4 Initiate a ecom payment
Last step - payment. At this stage, the customer has shown his QR, and membership status has been decided. When that is done and all wares are scanned, it is time to send a payment request to the user. Since the POS already have the number for the user, it should be done by just pressing a button on the POS and a push will appear on the users Vipps app. Once that is completed, the POS will be updated with the status of the payment. Done!

// image of payment opening in app?

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
