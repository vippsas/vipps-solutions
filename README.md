<!-- START_METADATA
---
title: Vipps MobilePay Recommended flows
sidebar_label: Overview
sidebar_position: 1
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Recommended flows

<!-- START_COMMENT -->
ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://developer.vippsmobilepay.com/docs/vipps-solutions/).
<!-- END_COMMENT -->

Vipps MobilePay offers several APIs that together form the [Vipps MobilePay API platform](https://developer.vippsmobilepay.com/docs/APIs/).
All APIs use the same API keys, authentication methods, terminology, etc. and they can be combined in many ways to offer the best user experience in various scenarios.
We want everyone to get the most out of our API platform, and below are recommended ways to implement API platform for most common scenarios.

**Recommended flows for physical and online settings:**

* [Online payments](./online/README.md)
* [In-store payments](./in-store/README.md)
* [In-store payments with customer club](./loyalty-in-pos/README.md)

**Alternative flows:**

Examples of how to combine APIs for specific use cases:

* [In-store payments using static QR](./static-qr-at-pos/README.md)
* [Payment requests](./invoice-through-epayments/README.md)
* [Vending machines](./vending-machines/README.md)
* [Subscriptions](./recurring-and-login/README.md)
* [Electric vehicle charging](./ev-charging/README.md)
* [Parking and "Pay-as-you-go"](./parking/README.md)
* [Taxi companies](./taxi-companies/README.md)

**Flows using Azure AD B2C:**

* [Vipps login with custom policy](./azure-b2c/CustomPolicyLogin.md)
* [Vipps payment flow using Azure AD B2C](./azure-b2c/PaymentFlowB2C.md)
