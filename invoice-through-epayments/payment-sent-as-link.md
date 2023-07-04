<!-- START_METADATA
---
title: Vipps MobilePay Payment request as a link
sidebar_label: Payment request as a link
sidebar_position: 52
hide_table_of_contents: true
pagination_next: null
pagination_prev: null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

import EX1 from './_create_payment_example.md'
END_METADATA -->

# Payment request as a link

When a merchant does not know the phone number of the user and want to start a payment request, you could send them a link to your own landing page that in turn triggers a payment request through Vipps MobilePay API.

The flow for the customer will look like this:

<Tabs
defaultValue="vipps"
groupId="app-choice"
values={[
{label: 'Vipps', value: 'vipps'},
{label: 'MobilePay', value: 'mobilepay'},
]}>
<TabItem value="vipps">

![Vipps payment request landing page flow](images/payment-request-with-link-vipps.png)

</TabItem>
<TabItem value="mobilepay">

![MobilePay payment request landing page flow](images/payment-request-with-link-mobilepay.png)

</TabItem>
</Tabs>

1. In your website, mobile app, on paper document or email you send, provide your customers with an option for opting-in to receive payment request for payment requests in the Vipps or MobilePay app.
2. Present them with the *Pay with Vipps* or *Pay with MobilePay* option.
3. Send the [create payment](https://developer.vippsmobilepay.com/api/epayment#tag/CreatePayments) request.

  <EX1 />

4. If the customer is on a desktop computer, the
   [Vipps MobilePay Landing page](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/vipps-landing-page)
   opens. If on a mobile device, the Vipps MobilePay app opens automatically.
