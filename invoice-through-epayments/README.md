<!-- START_METADATA
---
title: Vipps MobilePay Payment Requests
sidebar_label: Payment Requests
sidebar_position: 50
description: Using Vipps MobilePay for sending payment requests.
hide_table_of_contents: false
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

END_METADATA -->

# Payment Requests

Use Vipps MobilePay to make *long-living payment requests* for your customers by using the `"expiresAt"` feature in
[ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api)
and receipt information from
[Order Management API](https://developer.vippsmobilepay.com/docs/APIs/order-management-api).
This will create payment requests that can be seen and postponed by the user, up to 28 days.

Note: The API is ready, but not testable before app updates that are planned summer 2023.

The following sections will explain how to implement this feature:

* [Payment request sent directly to app](payment-sent-to-app.md)
* [Payment request as a link](payment-sent-as-link.md)
* [Payment request with sharing of telephone number](payment-with-share-phone-number.md)
