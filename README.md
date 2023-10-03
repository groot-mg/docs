# Documentation

<p align="center"
    <a href="https://github.com/groot-mg">
        <img height=200 src="./images/groot-image.jpeg">
    </a>
    <br>This repo is used to keep diagrams, designs and documentation about the project. 
</p>

---

## Draw.io
[draw.io](http://draw.io/) is used for the designs.

Files are available under the folder [/draw.io](./draw.io/).

## PlantText

[PlantText](https://www.planttext.com/) is used for the sequence diagrams.

Files are available under the folder [/plant-text](https://www.planttext.com/).

# Application architecture/design

The application uses a gateway as entry point and Oauth2 integration. Keycload is the Identity Service responsible to keep users data and provvide the access token to be used on the resource servers.

List of frameworks and tools used on the applications:

- [Keycloak](https://www.keycloak.org/)
- [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway)
- [Spring Cloud Netflix Eureka](https://spring.io/projects/spring-cloud-netflix)
- [Spring Security Oauth2](https://spring.io/projects/spring-security-oauth)
- [Prometheus](https://prometheus.io/)
- [Alert Manager](https://prometheus.io/docs/alerting/latest/alertmanager/#alertmanager)
- [Slack](https://slack.com/intl/en-gb)
- [Micrometer](https://micrometer.io/)
- [Grafana Tempo](https://grafana.com/oss/tempo/)
- [Grafana Loki](https://grafana.com/oss/loki/)
- [Grafana](https://grafana.com/)

And below the Design how all the frameworks and tools integrate each other:

<img src="./images/groot-mg.drawio.png" />

# Respositories 

What each repository does:

* `docs (this repository)`: contains documentation
* `docker-local-setup`: contains docker config to run all the services together on a local environment
    * Repository: [docker-local-setup](https://github.com/groot-mg/docker-local-setup)
* `insomnia-requests`: contains a backup for the insomnia requests
    * Repository: [insomnia-requests](https://github.com/groot-mg/insomnia-requests)
* `observability-tools`: contains config for prometheus, alert-manager, grafana, tempo and fluent-bit.
    * Repository: [observability-tools](https://github.com/groot-mg/observability-tools)
* `local-database-config`: contains centralised config for database
    * Repository: [local-database-config](https://github.com/groot-mg/local-database-config)
* `service-discovery`: service discovery app (Netflix eureka)
    * Repository: [service-discovery](https://github.com/groot-mg/service-discovery)
* `gateway`: gateway app, the application entrypoint. 
    * Repository: [gateway](https://github.com/groot-mg/gateway)
* `identity-service`: contains keycloak config (and an application that is not used in the project, but can be used to login and create new users)
    * Repository: [identity-service](https://github.com/groot-mg/identity-service)
    * Wiki: [here](https://github.com/groot-mg/identity-service/wiki)
* `sales-catalog`: manage products and stock
    * Repository: [sales-catalog](https://github.com/groot-mg/sales-catalog)
* `basket-service`: manage the client basket
    * Repository: [basket-service](https://github.com/groot-mg/basket-service)
* `notification-service`: kafka consumer for notifications, but this has no implementation

# Requirements / Features

## High Level

The project is an simple e-commerce that manages products and allow customers to buy them. It provides a whole monitoring architecture with Prometheus, Grafana dashboards and logging tracing, also including an alerting system with alert-manager integrated with Slack. 

In deep details are provided on the next topics, but a summary on the main features are:
* Sale and customer users are allowed, some features on the application are limited to specific users
* Sale users will register, update, delete, active or deactivate products
* Customers are able to retrieve all the active products and buy them
* Validations are included during product registration, update, delete, on user interation with basket
* A reservation system is implemented to avoid 2 different customers to buy the last product in stock
* On checkout, Kafka messages are created to notify the seller, the customer, and to create a tracking record to delivery the order, but there is no implementation on those features besides reading and logging the message

### What is not included on this project
This is not intended to be a complex or a full e-commerce implementation, so the features below are not included:
* Sending real e-mail/SMS or other type of notifications
    * `notification` repository contains a kafka consumer to consume notifications (kafka message) but it just logs on the console and does not have much implementation
* Tracking system (Delivery)
    * `notification` repository contains a kafka consumer to consume messages when the user checks out a basket, but there is not tracking system implementation for the delivery (shipping, dispatched, delivered, etc), it just logs on the console
* Discount cupom or promotions, or delivery calculation
    * Discounts, promotions or delivery calculation are not included on this project during check out
* Multiple currencies (Dolar, Euro, Real, etc)
    * There is no support for multiple currencies
* Multi language
    * product names or desriptions could have support for more than one language, but it is not set on this project
* Payments
    * This project does not implement any payment flow

## Users
Users will have 2 different roles: 

- `Sale`: user that is able to register products and see its sales
- `Client`: user that is able to purchase products 

`p.s`: The users are created directly on keycloak.

### Login with Keycloak

For Authentication and Authorization, an Spring OAuth2 is used with Keycloak. The sequence diagram below shows what happen when an unauthenticated user tries to access a specific resource.

1. First the user sends a request to the gateway
1.2. Gateway checks a session does not exists and returns a 302 to /oauth2/authorization/keycloak
2. Again the gateway returns a 302 but now redirects the request to keycloak login
3. Users login successfully and Keycloak redirects the request to the gateway
3.1 Gateway saves the session and user session details on keycloak, including the JWT Access Token (this token is used by the gateway to redirect requests to the final downstreams)
3.2 Returns 302 with a redirect to the initial request 
4. Request the Gateway again, and now logged in, access successfully the resource

<img src="./images/authentication.png" />

---
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://github.com/groot-mg/docs/blob/main/LICENSE)
