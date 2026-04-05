# Architecture: Dual-Channel Commerce Platform
### B2B + B2C Commerce on SAP Hybris / Commerce Cloud with AEM

**Author:** Satyendra Kanjilal
**Context:** Enterprise appliance manufacturer with simultaneous B2C (D2C) and B2B (trade/wholesale) commerce operations
**Note:** Sanitized reference architecture based on real enterprise experience.

---

## The Dual-Channel Challenge

Running B2B and B2C commerce on a shared platform creates a fundamental tension:

**B2C needs:** Fast, frictionless, mobile-first, personalized, emotionally engaging, conversion-optimized
**B2B needs:** Account management, contract pricing, approval workflows, bulk ordering, ERP integration, long-cycle sales support

Most organizations solve this by running separate platforms — which creates content silos, duplicated infrastructure, and fragmented customer data. The smarter approach is a **shared platform with separated experiences** — a single commerce backbone serving both channels with channel-specific storefronts and business logic.

This is the architecture that informed my D2C and B2B eCommerce leadership at Whirlpool.

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           EXPERIENCE LAYER                              │
├──────────────────────────────────┬──────────────────────────────────────┤
│     B2C STOREFRONT (D2C)         │       B2B STOREFRONT (Trade/Wholesale)│
│   AEM + Hybris Accelerator       │    Hybris B2B Accelerator + AEM      │
│   Consumer-facing · Personalized │    Account login · Contract pricing  │
│   Cart · Checkout · My Account   │    Approval workflows · Bulk order   │
└──────────────┬───────────────────┴────────────────┬────────────────────┘
               │                                    │
               └──────────────┬─────────────────────┘
                              │ Shared commerce services
┌─────────────────────────────▼───────────────────────────────────────────┐
│                     SAP COMMERCE CLOUD CORE                             │
│  Product Catalog  ·  Pricing Engine  ·  Promotions  ·  Order Management │
│  Customer Management  ·  Search (Solr/Elasticsearch)  ·  APIs           │
└────────┬──────────────────┬─────────────────────┬───────────────────────┘
         │                  │                     │
┌────────▼────────┐ ┌───────▼──────────┐ ┌───────▼───────────────────────┐
│   SAP ECC /     │ │   SAP CRM        │ │  Payment · Tax · Fraud        │
│   S4HANA        │ │                  │ │  Stripe/Braintree · Avalara   │
│  Inventory      │ │  Account mgmt    │ │  Signifyd                     │
│  Pricing        │ │  Case mgmt       │ │                               │
│  Financials     │ │  Service history │ │                               │
└─────────────────┘ └──────────────────┘ └───────────────────────────────┘
         │
┌────────▼──────────────────────────────────────────────────────────────┐
│               CONTENT & PRODUCT DATA LAYER                            │
│   Adobe Experience Manager (CMS)  ·  Salsify (PIM)  ·  DAM           │
└───────────────────────────────────────────────────────────────────────┘
```

---

## The Shared Core Principle

The key architectural decision: **what is shared, and what is separated?**

### Shared (Single Instance)
- **Product catalog**: One catalog, managed centrally, surfaced differently per channel
- **Pricing engine**: Base prices from ERP; B2B contract prices layered on top
- **Order management**: Single order model regardless of channel origin
- **Customer/account data**: Unified customer profile (with B2B account hierarchy on top)
- **Inventory**: Real-time ATP (Available-to-Promise) from ERP, same feed to both channels
- **Search index**: One index, channel-specific filtering applied at query time

### Separated (Channel-Specific)
- **Storefront UI**: B2C is AEM-driven, content-rich, mobile-first; B2B is functional, account-centric
- **Checkout flow**: B2C = streamlined, guest checkout enabled; B2B = PO number, approval routing, net terms
- **Pricing visibility**: B2C sees MSRP; B2B sees contract-specific pricing based on account
- **Promotions**: B2C promotions (discount codes, cart-level offers) don't bleed into B2B
- **Navigation and content**: B2C is product-discovery oriented; B2B is task-oriented

---

## Key Design Decisions

### Decision 1: AEM + Hybris Integration Model
Adobe Experience Manager manages the content presentation layer while SAP Hybris manages commerce logic. The integration is the complexity.

**Approach used:** Hybris serves as the commerce API backend; AEM renders the experience, calling Hybris APIs for product data, pricing, and cart operations. Content authors work in AEM; commerce configuration lives in Hybris.

**Trade-off:** More complex integration, but gives marketing teams AEM's authoring experience without requiring commerce knowledge.

### Decision 2: B2B Pricing Isolation
Contract pricing for B2B partners must be strictly isolated — a B2C customer must never see B2B pricing, and different B2B accounts must never see each other's contract pricing.

**Approach:** Hybris B2B price tables keyed to account group. Session-level price isolation enforced. Hybris's built-in B2B commerce module handles the account hierarchy and entitlement model.

**Critical test:** Penetration testing specifically targeting price isolation. This is a business-critical vulnerability if misconfigured.

### Decision 3: Order Routing Logic
B2C and B2B orders may need to route differently in the ERP — different GL accounts, different fulfillment priorities, different order types in SAP.

**Approach:** Order type flag set at checkout, passed through Hybris order model to ERP integration. ERP routes based on order type attribute, not storefront of origin.

### Decision 4: Search Strategy
Unified search index with channel-aware filtering. B2C search surfaces consumer-relevant attributes (style, color, finish, room type). B2B search surfaces professional attributes (BTU rating, installation specs, certification codes).

Same index. Different boost rules and facet configurations applied per channel at query time.

---

## Integration Architecture Detail

### ERP Integration (SAP ECC / S4HANA)
```
Hybris → ERP:
  - Order submission (real-time, synchronous)
  - Order status polling (scheduled, every N minutes)

ERP → Hybris:
  - Inventory / ATP updates (near real-time, event-driven)
  - Price updates (scheduled daily or on-change event)
  - Product availability changes (event-driven)
```

**Integration pattern:** SAP Integration Suite (formerly CPI) or Mulesoft as middleware. Never direct database connections between Hybris and ERP — always through an integration layer.

### CRM Integration (SAP CRM)
```
CRM → Hybris:
  - Account data and account hierarchy for B2B
  - Contract terms and entitlements

Hybris → CRM:
  - Order history (for service case context)
  - Customer registration events
  - Support ticket creation from storefront
```

### AEM ↔ Hybris
```
AEM calls Hybris APIs for:
  - Product data (catalog, attributes, images)
  - Pricing (real-time for authenticated users)
  - Cart and checkout operations
  - Account/order history

Hybris calls AEM for:
  - Rendering templates (in some integration patterns)
  - Content fragments (enriched product descriptions, marketing copy)
```

---

## Operational Considerations

### Performance
- Product catalog pages: Cache at CDN layer (Akamai/Cloudfront). TTL tuned to inventory update frequency.
- Pricing: Never cached for authenticated B2B users (contract-specific). Cache with short TTL for B2C anonymous users.
- Search: Solr/Elasticsearch cluster sized for peak traffic (Black Friday, promotional events). Autoscaling configured.

### Resilience
- Commerce platform on CCv2: SAP-managed Kubernetes with defined SLAs
- Graceful degradation: If ERP inventory check fails, default to "available" with oversell buffer rather than blocking checkout
- Circuit breakers on all third-party integrations (payment, tax, fraud)

### Security
- PCI-DSS compliance: Payment data never touches Hybris or AEM — handled by tokenized payment provider
- B2B account isolation: Session management and RBAC enforced at Hybris level
- API security: OAuth 2.0 for all API-to-API calls; rate limiting on all public-facing endpoints

---

## What I Would Do Differently

With the benefit of experience and hindsight:

1. **Invest earlier in headless commerce.** The AEM + Hybris tight coupling creates deployment friction. A headless approach (Hybris APIs + React/Next.js frontend) would give marketing more flexibility and reduce the blast radius of frontend changes.

2. **Build the integration layer first, not last.** ERP and CRM integrations are always the long pole — scoping them late causes schedule risk. Start integration design in Sprint 1, even if implementation comes later.

3. **Automate B2B account provisioning sooner.** Manual B2B account setup doesn't scale. Self-service account registration with approval workflow should be in scope from day one.

---

*← [Back to Portfolio](../README.md)*
*→ Related: [Enterprise PXM Ecosystem Architecture](./enterprise-pxm-ecosystem.md)*
