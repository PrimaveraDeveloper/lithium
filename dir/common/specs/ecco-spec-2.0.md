# eCommerce Connect Service (ECCO) Specification v2.0

The eCommerce Connect Service provides a middleware to integration products (Jasmin, V10, etc.) with eCommerce platforms.

eCommerce Connect platforms include marketplaces like Dott and online stores like Shopify.

## Core Concepts

This service provides a Web application that allows the user to manage the catalog, the orders and the refunds from multiple channels, integrating them with his product.

A channel corresponds to an eCommerce platform, being it a marketplace (like Amazon) or a third-party solution to create online stores (like Shopify).

Each user - each subscription - can integrate his PRIMAVERA product (V10, Jasmin or ROSE) with any number of the channels available and manage the online business (sales) generated on those channels in a centralized application.

The ECCO service acts as a middleware, in the sense that allows the implementation of these integrations (between products and channels) in a standardized form, that results in an unified user experience.

## Products

The following products are supported in the current version of the service:

- Jasmin
- V10

## Channels

The following channels are supported in the current version of the service:

- Dott
- Shopify

## App Store

Each pair product/channel is called an integration. For each integration there is an app in the App Store that the user can subscribe to activate the corresponding integration. The following apps are available for the current version of the service:

- Dott for Jasmin
- Dott for V10
- Shopify for Jasmin
- Shopify for V10

## Main Features

The service's Web application allows the user, among other things, to:

- Manage how his catalog - the sales items - are published on each channel.
- Manage the orders for each channel and produce invoices when they are fullfilled.
- Manage the returns for each channel.