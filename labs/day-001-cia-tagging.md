# Day 001 Lab — CIA-Tag the ShopFast Architecture

**Date:** 2026-05-16
**Concept:** CIA Triad (Confidentiality, Integrity, Availability)
**Time budget:** 30-45 minutes

---

## Objective

Tag every component of the ShopFast e-commerce platform with its Confidentiality, Integrity, and Availability sensitivity (H / M / L). Identify the "crown jewels" — components where all three are HIGH.

This is the foundational exercise of CyberForge 365. ShopFast will be our testbed for the next 364 days.

---

## ShopFast Architecture (v1 — Day 1 baseline)

### Services
- `auth-service` — registration, login, JWT issuance, password reset
- `catalog-service` — product listing, search, categories
- `cart-service` — user cart state, Redis-backed
- `order-service` — checkout orchestration, order lifecycle
- `payment-service` — Stripe integration wrapper
- `notification-service` — transactional email + SMS (SendGrid + Twilio)

### Datastores
- PostgreSQL — `users`, `orders`, `audit_log`
- MongoDB — `catalog` (products, categories, reviews)
- Redis — `cart`, `session`, rate-limit counters

### Edge / Infra
- React SPA frontend
- Cloudflare CDN + WAF
- API Gateway (Kong / NGINX)

---

## The CIA Tagging Table

Fill in H / M / L for each row. Add a one-line justification.

| Component | C | I | A | Justification |
|-----------|---|---|---|---------------|
| auth-service | H | H | H | Password hashes, JWT signing key, MFA secrets — full compromise = total system takeover |
| catalog-service |   |   |   |   |
| cart-service |   |   |   |   |
| order-service |   |   |   |   |
| payment-service |   |   |   |   |
| notification-service |   |   |   |   |
| PostgreSQL (users) |   |   |   |   |
| PostgreSQL (orders) |   |   |   |   |
| PostgreSQL (audit_log) |   |   |   |   |
| MongoDB (catalog) |   |   |   |   |
| Redis (cart) |   |   |   |   |
| Redis (session) |   |   |   |   |
| React frontend |   |   |   |   |
| Cloudflare CDN + WAF |   |   |   |   |
| API Gateway |   |   |   |   |

---

## "What if it fails?" — One-line business impact

For each component, complete this sentence:

- **auth-service fails →** _e.g. nobody can login; full platform outage; brand trust hit_
- **catalog-service fails →**
- **cart-service fails →**
- **order-service fails →**
- **payment-service fails →**
- **notification-service fails →**
- **PostgreSQL (users) compromised →**
- **PostgreSQL (orders) tampered →**
- **PostgreSQL (audit_log) tampered →**
- **MongoDB (catalog) tampered →**
- **Redis (cart) wiped →**
- **Redis (session) leaked →**
- **CDN + WAF down →**
- **API Gateway down →**

---

## Crown Jewels

List the top 3 components where **all three** CIA pillars are HIGH. These are your most valuable assets — every future security decision will be weighted against protecting them.

1.
2.
3.

---

## Bonus — Architecture Diagram

Draw the ShopFast architecture using [excalidraw.com](https://excalidraw.com) (no login). Export PNG and save as `labs/day-001-shopfast-arch.png`. Mark crown jewels in red.

---

## Success Criteria

You'll know it worked when:
- [ ] Every row in the table has C / I / A scores + justification
- [ ] Every component has a "what if it fails" line
- [ ] `payment-service` and `auth-service` made your crown-jewels list
- [ ] You can defend each rating in one sentence if challenged
