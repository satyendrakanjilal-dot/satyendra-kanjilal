# Case Study: PXM Governance at Scale
### Building a Scalable Product Content Governance Model for a Multi-Brand Enterprise

**Organization:** Whirlpool Corporation (Fortune 500, Global Appliance Manufacturer)
**Role:** Senior IT Manager – Product Experience & Digital Commerce
**Scope:** 1,000+ SKUs · 5 product categories · Multi-brand · Omnichannel retail

---

## The Problem

Whirlpool sells across a complex, multi-brand portfolio — through Amazon, Lowe's, Home Depot, D2C, and wholesale partners simultaneously. Each channel has different content requirements, attribute schemas, image specs, and compliance rules.

Without a unified governance model, the result was predictable:
- Product content was duplicated, inconsistent, and manually maintained
- Time-to-market for new SKUs was slow and unpredictable
- Retailer compliance failures caused suppressions on digital shelves
- No single source of truth for product attributes across brands and regions
- IT and business teams operated in silos, with unclear ownership of data quality

The cost wasn't just operational. Poor digital shelf presence directly suppressed conversion — a problem measured in revenue, not just efficiency.

---

## My Approach

Rather than solving for symptoms (fixing bad content), I designed a governance operating model that addressed root causes: ownership, standards, tooling, and feedback loops.

### Phase 1 — Audit & Baseline (Weeks 1–4)
- Conducted a full content audit across all SKUs in the PIM (Salsify) to establish a completeness baseline
- Mapped content requirements by channel (Amazon A+ content, Lowe's syndication schema, Home Depot attributes, D2C AEM templates)
- Identified the top 20% of attribute gaps responsible for 80% of compliance failures
- Documented the as-is data flow from product creation → enrichment → syndication → retailer ingestion

### Phase 2 — Governance Model Design (Weeks 5–10)
Designed a three-layer governance model:

```
┌─────────────────────────────────────────────────────┐
│  LAYER 3: Channel Readiness                         │
│  Retailer compliance scoring · Syndication QA       │
├─────────────────────────────────────────────────────┤
│  LAYER 2: Content Standards                         │
│  Attribute taxonomy · Image specs · Copy guidelines │
├─────────────────────────────────────────────────────┤
│  LAYER 1: Data Ownership                            │
│  RACI by attribute type · Workflow triggers         │
└─────────────────────────────────────────────────────┘
```

Key governance decisions:
- Established **data stewardship roles** across marketing, product management, and IT
- Built a **RACI matrix** for each attribute class (technical specs, marketing copy, media assets, compliance attributes)
- Created a **content scorecard** — a lightweight metric for each SKU's readiness across channels
- Defined SLAs for content enrichment at each stage of the product lifecycle

### Phase 3 — Tooling & Automation (Weeks 10–20)
- Configured **Salsify** workflows to enforce mandatory attributes before syndication could trigger
- Built validation rules that flagged non-compliant content before it reached retailer ingestion (shift-left quality)
- Integrated **Syndigo** for automated digital shelf monitoring — alerting on suppressed content within 24 hours
- Worked with Informatica MDM to establish upstream data quality gates at the master data level

### Phase 4 — Operationalization & Metrics (Ongoing)
- Established a monthly **Content Health Review** cadence with business and IT stakeholders
- Created a retailer compliance dashboard surfacing real-time suppression rates and readiness scores
- Embedded governance checkpoints into the new product introduction (NPI) process

---

## Outcome

| Metric | Before | After |
|---|---|---|
| Content completeness (avg. SKU) | ~60% | 85%+ |
| Retailer compliance failures | Frequent / reactive | Proactive detection, <24hr resolution |
| Time-to-syndication (new SKU) | Weeks | Days |
| Manual content effort | High — no workflow | Automated gates reduced manual review by ~40% |
| Stakeholder alignment | Siloed | Unified RACI, shared scorecard |

The governance model also became the foundation for onboarding GenAI-assisted content generation — because you can't automate what isn't governed.

---

## What I Learned

**Governance without tooling is just documentation. Tooling without governance is just noise.**

The winning combination was:
1. Clear ownership (who is accountable for what data)
2. Standards that are specific enough to be enforced in software
3. Automation that makes compliance the path of least resistance
4. Metrics that tie content quality to business outcomes — not just IT hygiene

This model is now reusable. See the [Digital Shelf Readiness Framework](../frameworks/digital-shelf-readiness.md) for the distilled version.

---

## Technologies Used

`Salsify` · `Syndigo` · `Informatica MDM` · `Adobe Experience Manager` · `SAP Commerce Cloud` · `Amazon Vendor Central` · `Home Depot ProXtra API` · `Lowe's Supplier Portal`

---

*← [Back to Portfolio](../README.md)*
