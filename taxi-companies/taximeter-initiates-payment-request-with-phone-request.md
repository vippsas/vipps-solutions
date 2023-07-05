<!-- START_METADATA
---
title: Customer orders through taxi app and taximeter initiates the payment request
sidebar_label: Customer orders through taxi app and taximeter initiates the payment request
sidebar_position: 103
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Taximeter initiates the payment request with manually entered phone number

The driver enters the customer's mobile number into the taximeter, which initiates a push.
The Vipps or MobilePay app opens on the customer's phone and the customer pays the amount due.

Use the [ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api) with `userFlow:PUSH_MESSAGE` and `"customerInteraction": "CUSTOMER_PRESENT"` while initiating the payment.
Finally, when reservation is completed perform a [full capture](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture#capture-via-the-api).
