# App Blueprint Audit — Swagger

**Date:** 2026-07-16
**Auditor:** Research Agent (blueprint-audit skill)
**Method:** Codebase inventory + build verification + acceptance criteria comparison

---

## Summary

| Metric | Count |
|---|---|
| SHIPPED | 0 |
| PARTIAL | 2 |
| MISSING (planned, not built) | 5 |
| REMOVED (matches blueprint) | 47 |
| DRIFTED | 1 |
| ORPHANS | 0 |

**Forensic findings:** 0 (no database — DATABASE_URL not available)

---

## Nodes audit

### ===== ROUTES =====

| Node | Kind | Blueprint Status | AUDITED Status | Evidence |
|---|---|---|---|---|
| `/` — Landing & Domain Paste | route | failed | **DRIFTED** | `src/app/page.tsx` exists and compiles (build succeeded). But it renders a generic "Swagger — Welcome to your application" starter template. No domain input field, no brand detection flow, no domain verification modal, no 3-step explainer. Fails ALL acceptance criteria: no domain paste/submit, no DNS/email verification, no routing to /generating. Page is a static Next.js boilerplate, not the landing page. |
| `/launch-confirmation` | route | planned | **MISSING** | No route file exists (`find src/app -name 'page.tsx'` returns only `src/app/page.tsx`). No `<launch-confirmation>/page.tsx` directory. |
| `/admin/dashboard` | route | removed | **REMOVED** | — (matches recorded status) |
| `/admin/login` | route | removed | **REMOVED** | — |
| `/admin/onboarding` | route | removed | **REMOVED** | — |
| `/admin/orders` | route | removed | **REMOVED** | — |
| `/admin/products` | route | removed | **REMOVED** | — |
| `/admin/settings` | route | removed | **REMOVED** | — |
| `/cart` | route | removed | **REMOVED** | — |
| `/dashboard` | route | removed | **REMOVED** | — |
| `/dashboard/settings` | route | removed | **REMOVED** | — |
| `/pricing` | route | removed | **REMOVED** | — |
| `/setup-progress` | route | removed | **REMOVED** | — |
| `/store-preview` | route | removed | **REMOVED** | — |
| `/store/:storeId` | route | removed | **REMOVED** | — |
| `/storefront-live/:company_slug` | route | removed | **REMOVED** | — |
| `/storefront/:store_id` | route | removed | **REMOVED** | — |

### ===== COMPONENTS =====

| Node | Kind | Blueprint Status | AUDITED Status | Evidence |
|---|---|---|---|---|
| component:product-card | component | planned | **MISSING** | No `src/components/` directory exists in the repo. |
| component:checkout-form | component | planned | **MISSING** | No components directory or checkout form file. |
| component:order-confirmation | component | planned | **MISSING** | No components directory or order confirmation file. |
| component:BrandKitEditor | component | removed | **REMOVED** | — |
| component:CartSummary | component | removed | **REMOVED** | — |
| component:ProductCard | component | removed | **REMOVED** | — |
| component:brand-kit-editor | component | removed | **REMOVED** | — |
| component:brand-preview | component | removed | **REMOVED** | — |
| component:cart | component | removed | **REMOVED** | — |
| component:cart-summary | component | removed | **REMOVED** | — |
| component:color-picker | component | removed | **REMOVED** | — |
| component:dashboard-cards | component | removed | **REMOVED** | — |
| component:domain-input | component | removed | **REMOVED** | — |
| component:faq | component | removed | **REMOVED** | — |
| component:kpi-card | component | removed | **REMOVED** | — |
| component:launch-modal | component | removed | **REMOVED** | — |
| component:login-magic-link | component | removed | **REMOVED** | — |
| component:metrics-card | component | removed | **REMOVED** | — |
| component:order-table | component | removed | **REMOVED** | — |
| component:orders-table | component | removed | **REMOVED** | — |
| component:pricing-table | component | removed | **REMOVED** | — |
| component:product-detail-modal | component | removed | **REMOVED** | — |
| component:product-editor-modal | component | removed | **REMOVED** | — |
| component:product-grid | component | removed | **REMOVED** | — |
| component:progress-indicator | component | removed | **REMOVED** | — |

### ===== INTEGRATIONS =====

| Node | Kind | Blueprint Status | AUDITED Status | Evidence |
|---|---|---|---|---|
| integration:stripe-payments | integration | planned | **MISSING** | No Stripe dependency in `package.json` (no `@stripe/stripe-js`, `stripe` npm package), no webhook routes, no payment intent API routes. |
| integration:brand-detection-ai | integration | planned | **MISSING** | No API route for brand detection (`src/app/api/` doesn't exist). No AI/ML dependencies. |
| integration:StripePayments | integration | removed | **REMOVED** | — |
| integration:address-validation | integration | removed | **REMOVED** | — |
| integration:Analytics | integration | removed | **REMOVED** | — |
| integration:BrandDetectionAI | integration | removed | **REMOVED** | — |
| integration:DomainVerification | integration | removed | **REMOVED** | — |
| integration:EmailService | integration | removed | **REMOVED** | — |
| integration:order-tracking | integration | removed | **REMOVED** | — |
| integration:printful-fulfillment | integration | removed | **REMOVED** | — |
| integration:product-generation | integration | removed | **REMOVED** | — |
| integration:stripe-payment | integration | removed | **REMOVED** | — |
| integration:tax-calculation | integration | removed | **REMOVED** | — |

### ===== APP =====

| Node | Kind | Blueprint Status | AUDITED Status | Evidence |
|---|---|---|---|---|
| planned-app | app | planned | **PARTIAL** | `src/app/layout.tsx` and `src/app/page.tsx` exist, compile, and build produces a working Next.js app. The app scaffold exists. But it does NOT fulfill its purpose statement: "turns a company domain into a live, on-brand merchandise storefront in minutes via AI." No storefront, no brand detection, no products, no payment processing. The app shell is SHIPPED; the product functionality is MISSING. |

## Classification Notes

- **PARTIAL (2):** `planned-app` (app scaffold exists but zero functionality) and technically the root `/` is also partial/drifted.
- **DRIFTED (1):** `/` — the route exists but is a generic starter page, not the domain-paste landing. It directly contradicts the blueprint's spec.
- **MISSING (5):** All "planned" nodes that have zero code: stripe-payments, brand-detection-ai, product-card, checkout-form, order-confirmation, launch-confirmation route.
- **REMOVED (47):** These match their recorded "removed" status — no code expected, none found.
- **ORPHANS (0):** Every file in the repo maps to a blueprint node (the generic `/` maps to the "failed" landing page node).

## Orphan Analysis

No orphans found. The only non-blueprint file is the auto-generated `_not-found` route (Next.js default). `documents/.gitkeep` is empty scaffolding.

## Top Gaps (highest leverage to build next)

1. **Landing Page (`/`):** The most visible gap — the root route is the first thing every user sees. It's recorded as "failed" and currently shows a generic welcome template. Building the domain-paste landing with brand detection trigger is the critical path for the entire funnel.

2. **Stripe Payment Processing (`integration:stripe-payments`):** The core business model (12% GMV capture) depends on this. No Stripe dependency or payment route exists. This is a hard prerequisite for any storefront to function.

3. **Brand Detection AI (`integration:brand-detection-ai`):** The differentiating feature — automatic brand detection from a domain. Without it, the core value proposition ("in minutes, no design work") is unrealizable. This is the engine for the entire product.

## Notes

- No live deploy URL was provided; `npm run build` verified compilation.
- No DATABASE_URL in environment; database forensics skipped.
- Design tokens from page_briefs.md (navy background `#0d1f33`, violet accent) are not applied anywhere in the current codebase — the pages use raw inline styles with no palette.
- The blueprint records 47 of 55 nodes as "removed" — this is an early-stage project that has gone through active descoping/iteration.
