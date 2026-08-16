# CampusEats — Project Brief

## What the system does

CampusEats is a food-ordering platform built for students on a college campus. A student logs in, sets up a profile with delivery addresses, browses the restaurants and menus available nearby, adds dishes to a cart, and places an order. Payment happens online at checkout. Once the order is confirmed, a delivery rider is assigned to pick it up and drop it off, and the student can track the delivery in real time. Throughout the process — when the order is placed, when payment goes through, when it's out for delivery, and when it finally arrives — the student receives a notification.

## Who uses it

- **Students** — the primary users. They register, manage their profile/addresses, browse food options, order, pay, and track deliveries.
- **Restaurants** — provide the menus and pricing that students browse from (their listings live inside the system, even if they aren't directly "logging in" in this version).
- **Riders** — are assigned to orders and carry out the actual delivery.
- The system itself acts as the **coordinator**, connecting all of the above so an order flows smoothly from cart to doorstep.

## The nouns — the services and what they own

Breaking the system down by the data each part is responsible for gives six clear building blocks:

| Service | What it owns |
|---|---|
| **Accounts** | Student users, their login details, and saved addresses |
| **Catalogue** | Restaurants, their menus, and item prices |
| **Orders** | Shopping carts, placed orders, and order status |
| **Payments** | Transactions and refunds |
| **Delivery** | Riders and their assignments to orders |
| **Notifications** | The log of messages sent to students |

Each service owns its own slice of data exclusively — no two services share the same table. That separation is what keeps the system easy to change: for example, Catalogue's pricing logic can be rebuilt without Orders ever noticing.

## The verbs — the actions each service offers

Instead of letting other services reach into each other's data directly, each service exposes a small set of operations (a contract) that others can call:

- **Accounts:** register/login, update profile, add or edit an address
- **Catalogue:** `listRestaurants`, `getMenu`, `checkItem` (is it available, and at what price)
- **Orders:** `addToCart`, `placeOrder`, `getOrder`, `cancelOrder`
- **Payments:** `charge`, `refund`
- **Delivery:** `assignRider`, track delivery status
- **Notifications:** `send` (order placed, paid, on the way, delivered)

## How it comes together

When a student places an order, Orders acts as the coordinator: it calls Catalogue to confirm item availability and price, calls Payments to charge the student, calls Delivery to assign a rider, and calls Notifications to confirm the order — all without ever touching another service's internal data directly. Orders owns nothing beyond the order itself; everything else it needs, it asks for through a contract. That's the core idea behind splitting CampusEats into services: one clear job and one owned set of data per service, with contracts as the only way they talk to each other.
