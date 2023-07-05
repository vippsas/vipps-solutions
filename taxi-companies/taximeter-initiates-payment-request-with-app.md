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

# Customer orders through taxi app and taximeter initiates the payment request

Customer orders through taxi app and taximeter initiates the payment request.

The customer orders a taxi in the taxi company's app. After the journey, the taximeter automatically initiates a payment request to Vipps MobilePay with the customer's phone number, which is already known.
The customer's Vipps or MobilePay app open and the customer pays the amount due.

Use the [ePayment API](https://developer.vippsmobilepay.com/docs/APIs/epayment-api) with `userFlow:PUSH_MESSAGE` and `customerInteraction: CUSTOMER_PRESENT` while initiating the payment.

After final amount is confirmed do a [partial capture](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/capture#partial-capture)
and release the remaining amount from reservation with a [partial cancel](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/cancel#cancel-after-a-partial-capture) request.