<!-- START_METADATA
---
title: Taxi companies
sidebar_label: Taxi companies
sidebar_position: 70
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Taxi companies

<!-- START_COMMENT -->

ℹ️ Please use the website:
[Vipps MobilePay Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

💥 Improvements coming soon. 💥


Use Vipps to simplify the payment flow in your taxi solutions.

## Scenario 1 - Customer scans QR code which leads to payment page

The customer scans a QR and follows instructions to pay from the taxi company's landing page.

![labeling_in_the_taxi](images/labeling_in_the_taxi.png)

## Scenario 2 - Customer orders and pays through taxi app

The customer pays the taxi company from their app when ordering the taxi. The amount is reserved until the final amount is known, at which time the payment is captured.

![taxi_route](images/taxi_route.png)

## Scenario 3 - Customer orders through taxi app and taximeter initiates Vipps push

The customer orders a taxi in the taxi company's app. After the journey, the taximeter initiates a push to Vipps. It is not necessary to enter the phone number manually, because it is already known.
The Vipps app opens on the customer's phone and the customer pays the amount due.

## Scenario 4 - Taximeter initiates Vipps push with manually entered phone number

The driver enters the customer's mobile number into the taximeter, which initiates a push.
The Vipps app opens on the customer's phone and the customer pays the amount due.
