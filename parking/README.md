<!-- START_METADATA
---
title: Parking
sidebar_position: 30
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Parking

<!-- START_COMMENT -->
ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).
<!-- END_COMMENT -->

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
