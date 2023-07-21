<!-- START_METADATA
---
title: Vipps in Auth0
sidebar_label: Auth0
sidebar_position: 190
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Auth0

The following will describe how Vipps ePayment and Login can be integrated with Auth0 to achieve a better flow for user registration and payment. The login and payment flow can be implemented both separately and together. Implementing the flows together allow users to be registered during payment and later log in as a registered user.

Auth0 is a customer identity management solution. Learn more about it on the [Auth0 pages](https://auth0.com/docs/get-started/auth0-overview), as Vipps support will not help you to configure the service.

These guides describe how to use the Login and ePayment APIs with Auth0. The login guide will help you to configure Vipps as an identity provider in Auth0 and retrieve contact info from the user that logs in. 

- [Vipps login in Auth0 with Social Connections](SocialConnectionLogin.md)
- [Vipps payment flow using Auth0](PaymentFlowAuth0.md)