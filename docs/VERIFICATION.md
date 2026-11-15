# Local verification record

Date: 2026-11-15

## Builds and runtime

- Packaged `mall-admin` and `mall-portal` with JDK 17 / Maven 3.9.16.
- `mall-admin-web`: `npm ci` and `npm run build` completed.
- `mall-app-web`: `npm ci` and `npm run build:h5` completed.
- Admin UI (`127.0.0.1:5174`) and storefront H5 (`127.0.0.1:5175`) returned HTTP 200.
- Admin API health check (`localhost:18080/actuator/health`) returned HTTP 200.
- Mall portal API health check (`localhost:18085/actuator/health`) reported `UP` with MySQL, Redis, MongoDB, and RabbitMQ connected.

## Shopping flow

Used the seeded test account in the H5 UI to sign in, open Huawei HUAWEI P20, add the gold 16G SKU to the cart, review checkout, and create an order. Checkout showed an item amount of ¥3,788, a promotion discount of ¥89, and a payable amount of ¥3,699.

The order list displayed the new order as **awaiting payment**. The isolated MySQL database contained a matching order row with `status=0`, `pay_amount=3699.00`, and one matching row in `oms_order_item`. No payment endpoint or external payment provider was called.

## Limits

- Maven packaging used `-DskipTests`; the upstream automated test suite was not run.
- The run covered a single local purchase flow. It did not include load, concurrency, production, or real-payment testing.
- Runtime seed data was isolated in local Docker containers. Do not treat this database as production data.

