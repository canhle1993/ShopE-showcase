# Multi-Vendor E-Commerce (ShopE) Showcase

![Java 21](https://img.shields.io/badge/Java%2021-ED8B00?style=flat-square&logo=openjdk&logoColor=white) ![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white) ![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) ![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white) ![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square) ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

Public showcase repository for **ShopE**, a multi-vendor marketplace for buyers, sellers and admins. The storefront runs under the brand name SwiftCart.

![ShopE: buyer web, buyer app and rider app](assets/cover.webp)

<sub>Real screenshots from a local run with demo accounts. Personal details are blurred.</sub>

## At A Glance

| | |
|---|---|
| **Project type** | Aptech semester 4 team project, scored **99/100**, now extended as a personal project |
| **My role** | Team Lead and main developer across the backend, frontend and CI |
| **Scale** | ~380 REST endpoints · 142 Flyway migrations · ~680 test classes · CI split into 10 parallel shards |
| **Main stack** | Java 21, Spring Boot, Spring Security, Spring Data JPA, React, MySQL, Redis, MongoDB, AWS |
| **Source code** | Private. I am happy to walk through it in an interview |

## Quick Links

- [Overview](#overview)
- [My Contributions](#my-contributions)
- [Screenshots](#screenshots)
- [Engineering Highlights](#engineering-highlights)
- [Architecture](#architecture)
- [Order And Money Flow](#order-and-money-flow)
- [Testing And CI](#testing-and-ci)
- [Tech Stack](#tech-stack)
- [Project Status](#project-status)
- [Author](#author)

## Overview

ShopE lets many independent shops sell in one marketplace, the way large Southeast Asian e-commerce platforms work:

- **Buyers** browse and search products, check out from several shops in one order, pay by VNPay, in-app wallet or cash on delivery, track shipments, request cancellations or returns, and review purchases.
- **Sellers** register a shop, pass identity verification (KYC), manage products and stock, and process orders and fulfilment.
- **Admins** review shops and products, handle disputes, moderate listings and reconcile payments and shipping.
- **Shipping** goes through the GHN carrier API. A self-built GHN Simulator stands in for the carrier during development, covering delivery, proof of delivery, COD collection and remittance.

The hardest part of the project is money: every payment, refund, payout and withdrawal goes through a double-entry style ledger so balances can always be proven.

## My Contributions

- Led the team, planned the work in phases and reviewed and merged the code
- Designed the backend architecture and the database schema, managed through Flyway migrations
- Built most of the REST API: checkout, vouchers, loyalty coins, returns, disputes and seller payouts
- Designed and implemented the money ledger and wallet custody model
- Integrated VNPay, GHN, AWS S3 / SES / SQS / Parameter Store and real-time chat
- Built the GHN Simulator so the full shipping and COD cycle can be tested without a real carrier account
- Set up the test strategy on Testcontainers and the sharded GitHub Actions pipeline

## Screenshots

### Buyer web

![Storefront home with vouchers, categories and recommendations](docs/screenshots/buyer-01-home.webp)

| Product detail | Search with filters |
|---|---|
| ![Product detail with variants and shop vouchers](docs/screenshots/buyer-02-product.webp) | ![Search results with location, price and brand filters](docs/screenshots/buyer-03-search.webp) |

### Multi-shop cart, checkout and tracking

| Cart split by shop | Checkout per shop |
|---|---|
| ![Cart with items from two shops](docs/screenshots/buyer-04-cart.webp) | ![Checkout with one package per shop](docs/screenshots/buyer-13-checkout.webp) |

| Payment: SwiftPay wallet, VNPay or COD | Order tracking |
|---|---|
| ![Payment method choice with wallet balance check](docs/screenshots/buyer-14-checkout-pay.webp) | ![Package timeline from order placed to completed](docs/screenshots/buyer-10-track.webp) |

### Seller center

![Seller dashboard](docs/screenshots/seller-01-seller-dashboard.webp)

| Product catalog (540 products, variants) | Order handling |
|---|---|
| ![Seller product list with variants and content quality](docs/screenshots/seller-02-seller-products.webp) | ![Order card with packaging evidence and Arrange shipment](docs/screenshots/seller-03-seller-orders.webp) |

| Shop vouchers | Add product with content quality score |
|---|---|
| ![Shop voucher management](docs/screenshots/seller-07-seller-vouchers.webp) | ![Add product form with quality checklist](docs/screenshots/seller-12-seller-products-new.webp) |

### Admin: money, shipping and moderation

| Platform ledger | Wallet protection readiness checks |
|---|---|
| ![Platform ledger entries with direction, amount and dispute key](docs/screenshots/admin-05-admin-platform-ledger.webp) | ![Checks that custody equals wallet balances before protection is switched on](docs/screenshots/admin-06-admin-settings-wallet-custody.webp) |

| Shipping reconciliation | COD remittance |
|---|---|
| ![Quoted vs actual carrier fee per package](docs/screenshots/admin-08-admin-shipping-reconciliation.webp) | ![COD evidence and remittance workflow](docs/screenshots/admin-09-admin-cod-remittance.webp) |

| Operations overview | Product moderation (rules + AI) |
|---|---|
| ![Admin operations overview](docs/screenshots/admin-01-admin-dashboard.webp) | ![Moderation queue with keyword rules, AI analysis and risk level](docs/screenshots/admin-03-admin-moderation.webp) |

### Off-platform transaction protection

A seller sent a phone number to move the deal off the platform. The buyer never sees it, and the message lands in the admin review queue with the matched rule and the conversation context.

| What the buyer sees | What the admin sees |
|---|---|
| ![Buyer chat where the off-platform message is hidden for safety](docs/screenshots/chat-01-buyer-chat-widget.webp) | ![Off-platform chat queue with 85% risk and matched pattern](docs/screenshots/chat-03-admin-offplatform-chat.webp) |

![Spam pattern list used by the first detection tier](docs/screenshots/admin-20-admin-spam-patterns.webp)

### Livestream selling

| Seller studio on air | Broadcast monitor and live chat |
|---|---|
| ![Live Commerce Broadcast Studio on air](docs/screenshots/live-03-studio-on-air.webp) | ![Camera monitor with live interaction panel](docs/screenshots/live-04-broadcast-monitor.webp) |

![Buyer watch page with live chat and products in the live](docs/screenshots/live-07-buyer-watch.webp)

### Mobile apps (Flutter, Android)

Buyer app:

<p>
  <img src="docs/screenshots/android-02-home.webp" alt="Buyer app home" width="200" />
  <img src="docs/screenshots/android-03-cart.webp" alt="Buyer app cart" width="200" />
  <img src="docs/screenshots/android-06-purchases.webp" alt="Buyer app purchases" width="200" />
  <img src="docs/screenshots/android-07-order-detail.webp" alt="Buyer app order detail" width="200" />
  <img src="docs/screenshots/android-12-chat-thread.webp" alt="Buyer app chat with hidden message" width="200" />
</p>

Rider app for the GHN Simulator:

<p>
  <img src="docs/screenshots/android-g01-home.webp" alt="Rider home with cash to hand in" width="200" />
  <img src="docs/screenshots/android-g02-deliveries.webp" alt="Rider assigned deliveries" width="200" />
  <img src="docs/screenshots/android-g03-cash.webp" alt="Rider cash handover" width="200" />
  <img src="docs/screenshots/android-g06-delivery-detail.webp" alt="Delivery detail with COD amount" width="200" />
</p>

### GHN Simulator (carrier sandbox)

| Shipment timeline | Cash collection from riders |
|---|---|
| ![Shipment detail with event history](docs/screenshots/ghn-02-admin-shipments-S5UP97.webp) | ![Cash flow from rider to GHN to SwiftCart](docs/screenshots/ghn-05-admin-cod-remittance.webp) |

![Shipper management](docs/screenshots/ghn-04-admin-shippers.webp)

## Engineering Highlights

- **Multi-shop checkout:** one cart, split into one order per shop, with vouchers built on the Strategy pattern and loyalty coins
- **VNPay payments:** HMAC-SHA512 signature checks and idempotent IPN handling, so a callback that arrives twice never pays twice
- **Money ledger:** every wallet balance is backed by a platform custody account. Transfers are idempotent, locks are always taken in a fixed order to avoid deadlocks, and MySQL triggers reject any wallet write that has no matching custody movement
- **Seller payouts held until the return window closes**, with automatic recovery of seller debt from later payouts
- **Overselling guard in two layers:** a Redis availability gate, then an atomic conditional SQL update with an optimistic version column
- **Security:** JWT refresh-token rotation with reuse detection, Redis-based rate limiting, AES-GCM encryption for seller ID numbers and a private S3 bucket for KYC documents
- **Moderation:** SQS queues feed rule-based checks and optional AI review of new products
- **Real-time chat** between buyers and sellers over WebSocket, stored in MongoDB
- **Livestream selling** split into its own microservice, so thousands of open connections cannot slow down ordering

## Architecture

```mermaid
flowchart LR
    People[Buyer / Seller / Admin] --> SPA[React SPA]
    Mobile[Flutter buyer app] --> Proxy
    SPA --> Proxy[Caddy reverse proxy]
    Proxy --> API[Spring Boot API<br/>Java 21]
    Proxy --> Live[Livestream service]
    API --> MySQL[(MySQL 8)]
    API --> Redis[(Redis 7)]
    API --> Mongo[(MongoDB<br/>chat)]
    API --> S3[AWS S3<br/>public + private]
    API --> SES[AWS SES<br/>email]
    API --> SQS[AWS SQS<br/>moderation]
    API --> VNPay[VNPay]
    API --> GHN[GHN carrier API]
    API <--> Sim[GHN Simulator]
```

- The main API is a modular Spring Boot monolith with layered packages for controllers, services, repositories, security, integrations and scheduled jobs
- The livestream service and the GHN Simulator each have their own database and run as separate services
- Secrets come from AWS Systems Manager Parameter Store in the cloud profile, never from the repository
- Everything runs in Docker Compose behind Caddy, with an optional Cloudflare edge

## Order And Money Flow

```mermaid
flowchart LR
    Buyer -->|VNPay / wallet| Prepaid[Prepaid clearing]
    Buyer -->|cash on delivery| Carrier[Carrier settlement]
    Carrier -->|COD remittance| COD[COD clearing]
    Prepaid -->|after return window| Custody[Wallet custody]
    COD -->|after return window| Custody
    Custody -->|seller payout| Wallet[Seller wallet]
    Wallet -->|withdrawal| Reserve[Withdrawal reserve]
    Reserve --> Bank[Seller bank account]
    COD -->|commission and fees| Treasury[Platform treasury]
    Prepaid -->|refund| BuyerWallet[Buyer wallet]
```

Each arrow is a named ledger posting with an idempotency key. A readiness check proves that custody equals the sum of all wallet balances, and an admin can then switch on database-enforced wallet protection, with a reason and an audit trail.

The full COD cycle (seller-fault and platform-fault returns, automatic payouts, debt recovery and an emergency switch-off and back on) passed an end-to-end QA run on a separate database in October 2026, with custody matching total wallet balances throughout.

## Testing And CI

- Around **680 test classes**, with integration tests running on **Testcontainers MySQL** against the real production Flyway migrations
- Migration tests start from earlier schema versions to prove each upgrade works, and Hibernate schema validation guards the final schema
- **GitHub Actions** runs a fast tier (backend checks without a database plus frontend tests and build) on every push
- The full backend suite is split into **10 parallel shards**, balanced from recorded test timings, plus a two-system end-to-end tier with the GHN Simulator

## Tech Stack

- **Backend:** Java 21, Spring Boot, Spring Security, Spring Data JPA / Hibernate, Flyway
- **Frontend:** React, Vite, React Router
- **Mobile:** Flutter buyer app for Android (in progress)
- **Data:** MySQL 8, Redis 7, MongoDB 7, optional Meilisearch for search
- **Cloud:** AWS S3, SES, SQS, Systems Manager Parameter Store
- **Integrations:** VNPay, GHN shipping API
- **Ops:** Docker Compose, Caddy, GitHub Actions, Testcontainers

## Project Status

ShopE is under active development. Buyer, seller, admin, payment, shipping and ledger flows are in place and tested locally. The VPS deployment is being prepared, so there is no public demo URL yet. I can show it running locally on a screen share.

## Private Source Code Note

The full source code is private. This repository explains the product, architecture and my part of the work. If you are a recruiter, interviewer or reviewer, I can provide:

- a guided walkthrough of the code
- a live demo over screen sharing
- more detail on any design decision above

## Author

**Lê Văn Cảnh** · Java Full-Stack Developer · Ho Chi Minh City, Vietnam

- Portfolio: [canhle.pages.dev](https://canhle.pages.dev/)
- Email: [aplevancanh1993@gmail.com](mailto:aplevancanh1993@gmail.com)
- GitHub: [@canhle1993](https://github.com/canhle1993)
