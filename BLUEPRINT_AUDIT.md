# App Blueprint Audit — Swagger

**Date:** 2026-07-16
**Auditor:** Research Agent (blueprint-audit skill)
**Live deploy:** https://swagger-app.lovable.app
**Local repo:** Next.js 15 scaffold (`src/app/page.tsx` + `layout.tsx` only)
**Method:** Codebase inventory (local repo) + live deploy forensics (HTTP probes, JS bundle analysis, CSS analysis) + acceptance criteria comparison

**⚠️ CRITICAL FINDING — Codebase/Deploy Disconnect:** The local repository is a bare Next.js 15 scaffold (`npx create-next-app` default) with zero Swagger-specific code. The live deploy at swagger-app.lovable.app is a completely different application — a TanStack Router SPA built via **Lovable** (project `efe9473f-928a-4e63-931e-2b969885d9b3`). The two codebases are unrelated. All audit findings below are based on the **live deploy**, not the local repo.

---

## Summary

| Metric | Count |
|--------|-------|
| **SHIPPED** | 0 |
| **PARTIAL** | 2 |
| **MISSING** (planned, not built) | 6 |
| **DRIFTED** | 0 |
| **REMOVED** (matches blueprint) | 47 |
| **ORPHANS** | 1 (Lovable platform infrastructure) |
| **Forensic findings** | 3 |

---

## Nodes Audit

### ===== APP =====

| Node | Kind | Blueprint Status | AUDITED Status | Evidence |
|------|------|-----------------|----------------|----------|
| planned-app | app | planned | **PARTIAL** | Live deploy at https://swagger-app.lovable.app exists as a TanStack Router SPA with a styled landing page. The app shell is functional and visually polished. **But the product purpose is unfulfilled**: "turns a company domain into a live, on-brand merchandise storefront in minutes via AI." No storefront, no brand detection, no products, no payment processing. App scaffold exists; product functionality is entirely missing. |

### ===== ROUTES =====

| Node | Kind | Blueprint Status | AUDITED Status | Evidence |
|------|------|-----------------|----------------|----------|
| `/` — Landing & Domain Paste | route | failed | **PARTIAL** | Live deploy serves a polished landing page with: "Your brand, detected in minutes." headline, domain input field, "Generate My Store" button, 3-step explainer, badges. **Fails acceptance criteria:** <br>• ❌ Domain ownership verification (DNS TXT or email): Not implemented — form submission is client-side only (no API call occurs).<br>• ❌ Routing to /generating on success: Form submission sets local loading state only; no navigation or redirect happens.<br>• ❌ Error for invalid/claimed domain: Only client-side regex validation; no server-side check for already-claimed domains.<br>• ✅ User can paste domain and submit without friction — the sole passing AC. |
| `/launch-confirmation` | route | planned | **MISSING** | Returns HTTP 404. No route file exists on either the local codebase or the live deploy. |
| `/admin/dashboard` | route | removed | **REMOVED** | Returns HTTP 404. Matches recorded "removed" status. |
| `/admin/login` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/admin/onboarding` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/admin/orders` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/admin/products` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/admin/settings` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/cart` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/dashboard` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/dashboard/settings` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/pricing` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/setup-progress` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/store-preview` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/store/:storeId` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/storefront-live/:company_slug` | route | removed | **REMOVED** | Returns HTTP 404. |
| `/storefront/:store_id` | route | removed | **REMOVED** | Returns HTTP 404. |

### ===== COMPONENTS =====

| Node | Kind | Blueprint Status | AUDITED Status | Evidence |
|------|------|-----------------|----------------|----------|
| component:product-card | component | planned | **MISSING** | No `src/components/` directory in local repo. No product card component found in the live deploy's JS bundles. All product-related UI elements are absent. |
| component:checkout-form | component | planned | **MISSING** | No checkout form component exists in either codebase. Stripe Elements are not imported. |
| component:order-confirmation | component | planned | **MISSING** | No order confirmation component exists. No order confirmation routes. |
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
|------|------|-----------------|----------------|----------|
| integration:stripe-payments | integration | planned | **MISSING** | No Stripe dependencies found in either codebase. No `@stripe/stripe-js` package. No Payment Intent API routes. No Stripe Elements in the frontend bundles. Zero Stripe JS in the live deploy's assets. |
| integration:brand-detection-ai | integration | planned | **MISSING** | No API routes for brand detection. No AI/ML dependencies. The landing page's "Generate My Store" button sets local loading state but performs **zero** API calls — no brand detection is triggered. JS bundle analysis confirms: the form `onSubmit` handler only validates domain format client-side and sets a `loading` boolean state; no `fetch()`, no `XMLHttpRequest`, no Supabase, no external API call. |
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

---

## Orphan Analysis

**1 orphan found (Lovable platform infrastructure):**
- `/assets/styles-C10IOtH1.css` — Tailwind v4 CSS with full light/dark theme (shadcn/ui palette in oklch). Not referenced in any blueprint node.
- `/assets/index-DUv8j9BH.js` — TanStack Router + React runtime bundle. Not in blueprint.
- `/assets/routes-CWtYSANK.js` — The landing page component (only route). Not in blueprint.
- `/~flock.js`, `/__l5e/events.js` — Lovable analytics/telemetry scripts.
- The Lovable badge (Edit with Lovable) and its styling.

These are entirely from the Lovable build platform and are not Swagger product code. No orphan *product* code exists.

---

## Forensic Findings

### Finding 1: Form submission is a no-op (Critical)
**Severity: CRITICAL** — User-facing functionality that doesn't work

The sole interactive element on the live deploy — the domain input form — performs **no backend operation whatsoever**. When a valid domain is submitted:
1. Client-side regex validates the domain format (`/^(?!:\/\/)([a-zA-Z0-9-]+\.)+[a-zA-Z]{2,}$/`)
2. Button changes to "Analyzing your brand…" with a spinner icon
3. **No API call, no navigation, no state change beyond the loading toggle**

The button stays in loading state indefinitely until page refresh. This is a pure frontend demo with zero backend integration.

**Evidence:** JS bundle analysis of the routes component shows the entire `onSubmit` handler is:
```js
onSubmit: t => {
  t.preventDefault();
  let n = y(e);  // sanitize domain input
  if (/\s/.test(e) || !v.test(n)) {
    r('Enter a valid domain (e.g. yourcompany.com)');
    return;
  }
  r(null), a(!0);  // set loading state to true — no fetch, no API call
}
```

No `fetch()`, `axios`, Supabase, or any HTTP client call exists in the landing page component.

### Finding 2: Zero routes beyond landing page
**Severity: HIGH** — The entire product funnel is missing

Every route tested beyond `/` returns HTTP 404:
- `/admin`, `/admin/login`, `/admin/orders` → 404
- `/store/1`, `/storefront/test-company` → 404
- `/cart`, `/pricing`, `/dashboard` → 404
- `/launch-confirmation`, `/brand-preview`, `/generating` → 404
- `/api`, `/api/domains/check` → 404

The TanStack Router manifest confirms only two routes exist: `__root__` (layout) and `/` (landing page). No other route definitions exist.

### Finding 3: No backend infrastructure
**Severity: HIGH** — The product has no backend

The live deploy has:
- No Stripe integration (no `@stripe/stripe-js`, no Payment Intents, no checkout sessions)
- No database (no Supabase, no PostgreSQL connection)
- No authentication (no Supabase Auth, no magic links, no login)
- No API endpoints (no `/api/*` routes)
- No email service (no Resend, SendGrid, or Mailgun)
- No Printful or fulfillment integration
- No AI/ML brand detection service
- No analytics integration beyond Lovable's built-in event tracking

The live deploy is a static single-page application with no backend dependencies.

---

## Top Gaps (highest leverage to build next)

1. **Backend API & Domain Submission Flow (Critical Path)**
   The most foundational gap. The landing page currently submits to nowhere. Building an API endpoint that receives the domain, initiates verification (DNS/email), triggers brand detection, and returns status to the frontend is the *critical path* for the entire product funnel. Without this, the landing page is a static brochure.

2. **Stripe Payment Integration (Revenue Enabler)**
   The business model depends on 12% GMV capture via Stripe. No Stripe dependency, API routes, or frontend integration exists. This requires: adding `@stripe/stripe-js` and `stripe` npm packages, building `/api/create-payment-intent` and `/api/webhook` routes, and building the checkout modal with Stripe Elements.

3. **Brand Detection AI (Core Differentiator)**
   The value proposition — "AI detects your brand" — is completely unrealized. No brand detection algorithm, no API integration with an image/vision AI service, no color extraction logic exists. This is the technical core of the product and has zero implementation.

---

## Design Token Alignment

The page briefs specify these design tokens:
- `color.bg` = `#0d1f33` (deep navy)
- `color.surface` = `#102542`
- `color.border` = `#1a3a5c`
- `color.text` = `#ecebf3`
- `color.text_muted` = `#8fa3b8`
- `color.accent` = violet

The live deploy uses a Tailwind v4 theme with oklch color values (not the specified hex codes). However, the general aesthetic is consistent: dark navy backgrounds (oklch 12.9%–23.5% lightness ≈ #0d1f33–#1a3348 range), violet accent (295° hue on primary/accent, matching "violet" requirement), and light text. The specific hex values are not used but the intent is roughly matched.

---

## Notes

- **Codebase vs Deploy Mismatch:** The local repo is a Next.js 15 starter scaffold with zero product code. The live deploy is a TanStack Router SPA built via Lovable. These are completely separate codebases. Development should happen in the Lovable project (ID: `efe9473f-928a-4e63-931e-2b969885d9b3`), not the local repo.
- The blueprint records 47 of 55 nodes as "removed" — this is a very early-stage project that has gone through aggressive descoping.
- No database forensics were possible (no DATABASE_URL in environment).
- The landing page's visual design is well-executed with dark theme, violet accent, and responsive layout — the frontend foundation is solid.
