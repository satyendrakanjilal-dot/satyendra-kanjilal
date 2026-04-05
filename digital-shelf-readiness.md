# Framework: Digital Shelf Readiness Model
### A Practical Model for Measuring and Improving Product Content Quality Across Retail Channels

**Author:** Satyendra Kanjilal
**Version:** 1.2
**Based on:** Enterprise PXM leadership at Whirlpool Corporation

---

## What This Is

The **Digital Shelf Readiness Model** is a scoring and governance framework for evaluating whether a product's content is ready to perform — not just be published — on a given retail channel.

"Ready to publish" and "ready to perform" are not the same thing. A product can be live on Amazon and still be invisible due to missing attributes, poor search indexing, or suppressed buy-box eligibility.

This model was developed through experience managing 1,000+ SKUs across Amazon, Lowe's, Home Depot, and D2C channels. It's designed to be **tool-agnostic** — implementable in Salsify, Akeneo, Informatica, or any PIM — and **channel-extensible** as your retail footprint grows.

---

## The Framework: Four Readiness Dimensions

```
┌─────────────────────────────────────────────────────────────────────┐
│                    DIGITAL SHELF READINESS SCORE                    │
├──────────────┬──────────────┬───────────────────┬───────────────────┤
│  COMPLETENESS│  COMPLIANCE  │     QUALITY       │    FRESHNESS      │
│    (30%)     │    (30%)     │     (25%)         │     (15%)         │
└──────────────┴──────────────┴───────────────────┴───────────────────┘
```

### Dimension 1: Completeness (30%)
*Does the product have all required attributes populated?*

Scored against a **mandatory attribute matrix** — a channel-specific list of fields that must be populated for a SKU to be considered complete.

| Attribute Tier | Description | Weight |
|---|---|---|
| Tier 1 — Critical | Product title, primary image, category, brand, UPC/GTIN | Block publication if missing |
| Tier 2 — Required | Key specs (dimensions, weight, power), short description, 3+ images | Required for full score |
| Tier 3 — Enhanced | Long description, lifestyle images, A+ content, video, FAQs | Boosts score above baseline |

**Completeness Score = (Populated Tier 2 attributes / Total Tier 2 attributes) × 100**

### Dimension 2: Compliance (30%)
*Does the content meet channel-specific technical requirements?*

Each retailer has schema requirements: character limits, image dimensions, prohibited terms, required keywords, attribute formats. Non-compliant content is suppressed or rejected — even if it's complete.

Compliance is checked per channel:

| Check | Example |
|---|---|
| Character limits | Amazon title ≤ 200 chars, bullet ≤ 200 chars |
| Image spec | Minimum 1000px on longest side, white background for primary |
| Prohibited terms | No "best," "cheapest," unsubstantiated superlatives without proof |
| Required attributes | Home Depot requires specific attributes not required on Amazon |
| Regulatory | Energy Star claims, UL certifications — must be verified, not asserted |

**Compliance Score = (Passed compliance checks / Total compliance checks) × 100**

### Dimension 3: Quality (25%)
*Is the content accurate, clear, and optimized for conversion?*

Quality is the hardest dimension to automate — it requires human judgment. But it can be **structured**:

| Quality Signal | Measurement Approach |
|---|---|
| Title optimization | Does title lead with primary benefit + key spec? |
| Bullet effectiveness | Do bullets lead with benefit keywords? Are they scannable? |
| Description accuracy | Does copy match spec sheet? (AI hallucination risk) |
| Image sequence | Does image order follow best practice (hero → lifestyle → detail → dimensions)? |
| Keyword presence | Are primary search keywords present in title and first bullet? |

Quality is scored in two ways:
- **Automated signals** (keyword presence, structural checks) — ~60% of quality score
- **Human review score** (content steward rating 1–5 on clarity and accuracy) — ~40% of quality score

### Dimension 4: Freshness (15%)
*Is the content current?*

Stale content costs money. Product updates, specification changes, and regulatory updates need to flow to the digital shelf — and often don't.

| Freshness Check | Flag |
|---|---|
| Content age > 12 months | Review trigger |
| Spec sheet updated but PIM not updated | Critical flag |
| Retailer schema updated since last syndication | Compliance risk |
| New product imagery available but not published | Opportunity flag |

**Freshness Score = f(days since last review, change events since last update)**

---

## Composite Readiness Score

```
Readiness Score = 
  (Completeness × 0.30) + 
  (Compliance × 0.30) + 
  (Quality × 0.25) + 
  (Freshness × 0.15)
```

**Thresholds:**
| Score | Status | Action |
|---|---|---|
| 90–100 | ✅ Shelf-Ready | Publish / maintain |
| 75–89 | 🟡 Near-Ready | Minor enrichment before next sync |
| 50–74 | 🟠 Incomplete | Enrichment required — do not prioritize new channels |
| < 50 | 🔴 Not Ready | Block from syndication until resolved |

---

## Implementation Guide

### Phase 1: Define Your Mandatory Attribute Matrix (Week 1–2)
- List all channels you publish to
- For each channel, define Tier 1, 2, and 3 attributes
- This becomes your scoring baseline

### Phase 2: Configure Scoring in Your PIM (Week 2–4)
- Most modern PIMs (Salsify, Akeneo) support calculated fields or workflow rules
- Build completeness and basic compliance checks as automated workflow conditions
- Quality and freshness scores may require custom fields or lightweight scripts

### Phase 3: Build the Review Workflow (Week 4–6)
- Route SKUs below threshold to a content review queue automatically
- Assign ownership: who is responsible for each tier of attributes?
- Set SLAs: how long can a SKU sit below threshold before escalation?

### Phase 4: Report and Iterate (Ongoing)
- Weekly: Content health dashboard (by category, brand, channel)
- Monthly: Trend review — are scores improving?
- Quarterly: Revisit thresholds and attribute matrix as retailer requirements evolve

---

## Common Failure Modes

| Failure | Root Cause | Fix |
|---|---|---|
| Scores are high but performance is low | Quality dimension not calibrated — scores don't reflect real content effectiveness | Recalibrate quality rubric with conversion data |
| Compliance score always 100% | Compliance rules not updated when retailer changes schema | Establish quarterly retailer schema review |
| Nobody acts on the score | Score exists in PIM but not in any workflow | Embed score in syndication trigger — content below threshold cannot syndicate |
| Freshness ignored | No process for monitoring spec changes upstream | Connect product lifecycle events to content review triggers |

---

## Tools That Support This Framework

| Tool | Role in Framework |
|---|---|
| **Salsify** | PIM, workflow automation, completeness tracking |
| **Syndigo** | Compliance monitoring, digital shelf analytics |
| **Informatica MDM** | Upstream data quality, master data accuracy |
| **Akeneo** | Alternative PIM with similar workflow capabilities |
| **Custom dashboards** | Composite scoring, executive reporting |

---

## Closing Note

This framework is not a one-time project. Digital shelves are dynamic — retailers change requirements, product lines evolve, brand standards update. Build the model to be **maintained**, not just deployed.

The organizations that win on the digital shelf are the ones that treat content as a living asset — not a publish-and-forget activity.

---

*← [Back to Portfolio](../README.md)*
*→ Related: [PXM Governance Case Study](../case-studies/pxm-governance-at-scale.md)*
