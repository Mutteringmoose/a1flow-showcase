# A1 Flow

**Institutional-grade market analytics for active retail options traders.**

A1 Flow lives in the gap between consumer brokerage apps and professional terminals. It aims for the analytical depth of a Bloomberg Terminal with the speed and clarity of a modern mobile app, built around one principle: signal, zero noise.

> Solo-built end to end: product, frontend, backend, data pipeline, and cloud infrastructure.

---

## What it does

A1 Flow ingests market, options, and fundamentals data, runs it through a set of proprietary quantitative screeners, and surfaces actionable signals through a fast, focused interface.

**Core screeners & analytics**
- **Valuation / re-rating detection.** Surfaces names undergoing multiple expansion against their own historical valuation bands.
- **Earnings inflection.** Identifies companies turning the corner on earnings trajectory, anchored to the start of a genuine up-run rather than noise.
- **Options & sector flow.** Distills unusual activity and sector rotation into a readable signal instead of a firehose.
- **Per-ticker research drill-down.** Fundamentals, valuation, and forward estimates in one view.

All screeners run on automated daily schedules and persist results for instant, cached delivery, so one server-side computation serves every user.

---

## Tech stack

**Frontend**
- React (TypeScript)
- Deployed on Vercel with CI/CD auto-deploy on push to `main`

**Backend / Infrastructure (AWS)**
- AWS Lambda (Node.js) for screener engines, data proxies, and API handlers
- API Gateway (HTTP API v2) as the routing layer
- DynamoDB for the results store and universe management
- EventBridge for scheduled daily screener runs
- S3 + CloudFront for static asset delivery
- Secrets Manager for credential management

**Data integration**
- Integrates **six external APIs** spanning market fundamentals, options data, macroeconomic indicators, and unusual-activity feeds.
- All providers are normalized behind a **custom proxy layer** that handles authentication, caching, provider rate limits, regional IP restrictions, and inconsistent response shapes, so the app sees one clean, stable interface regardless of upstream quirks.
- Credentials for every provider are centralized in AWS Secrets Manager, never hardcoded and never client-side.

**Payments: Stripe (live)**
- Live Stripe integration powering three subscription tiers (Free, Gold, Diamond).
- **Webhook-driven entitlements.** Stripe events are received by a dedicated Lambda handler, validated, and used to update a user's subscription state in DynamoDB. Stripe is the source of truth for tier.
- Tier state then flows through to feature access in the app.

**Authentication & access control (AWS)**
- **OAuth-based sign-in**, with identity and session handling backed by AWS.
- Requests hit **API Gateway**, which routes through **Lambda** handlers. Secrets and tokens are resolved at runtime from **Secrets Manager**, never embedded in the client.
- Subscription tier governs feature access, so a single auth and entitlement layer gates the premium analytics consistently across web and mobile.

---

## Architecture highlights

- **Cache-first design.** Expensive data fetches and screener computations run once, server-side, on a schedule, then serve all users from DynamoDB. No per-user API spend.
- **Resilient data layer.** A dedicated proxy layer handles provider rate limits, regional IP restrictions, and inconsistent response shapes so the app stays fast and stable.
- **Disciplined deploys.** Syntax-checked, dry-run-first, drift-guarded deployment scripts prevent regressions on live infrastructure.

---

## Product positioning

| | A1 Flow |
|---|---|
| **vs. consumer brokerages** | Adds real analytical depth instead of gamified basics |
| **vs. professional terminals** | Accessible, mobile-first, and priced for individuals |
| **Subscription tiers** | Free, Gold, Diamond |

---


## About

Built and maintained by myself, covering product strategy, full-stack engineering, cloud architecture, and the quantitative methodology behind the screeners.

