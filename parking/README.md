<!-- START_METADATA
---
title: Parking
sidebar_position: 30
---
END_METADATA -->

<!-- START_COMMENT -->
ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).
<!-- END_COMMENT -->

# Parking

💥 DRAFT! Unfinished work in progress. 💥

There are many ways to use Vipps for paying for parking.

One way is to use the
[Recurring API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/recurring-api)
and
[Recurring agreements with variable amount](https://vippsas.github.io/vipps-developer-docs/docs/APIs/recurring-api/vipps-recurring-api#recurring-agreements-with-variable-amount):

1. The user enters an Agreement as usual. See
   [Create an agreement](https://vippsas.github.io/vipps-developer-docs/docs/APIs/recurring-api/vipps-recurring-api#create-an-agreement).
2. The user parks one or more times.
3. The accumulated parking fees are used to create one charge with the
   accumulated amount. See:
   [Create a charge](https://vippsas.github.io/vipps-developer-docs/docs/APIs/recurring-api/vipps-recurring-api#create-a-charge).   

See also:
* [Vipps QR API](https://vippsas.github.io/vipps-developer-docs/docs/APIs/qr-api)

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-solutions/issues),
a [pull request](https://github.com/vippsas/vipps-solutions/pulls),
or [contact us](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/contact).

Sign up for our [Technical newsletter for developers](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/newsletters).
