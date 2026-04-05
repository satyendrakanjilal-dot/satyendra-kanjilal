# Case Study: GenAI for Content Operations
### Reducing Manual Effort in Enterprise PXM Workflows with AI-Assisted Content

**Organization:** Whirlpool Corporation
**Role:** Senior IT Manager – Product Experience & Digital Commerce
**Status:** Active initiative (2023–present)
**Scope:** AI-assisted content generation and enrichment across 1,000+ SKU portfolio

---

## The Problem Worth Solving

Product content is expensive to produce and painful to maintain at scale.

For a multi-brand appliance manufacturer selling across Amazon, Lowe's, Home Depot, and D2C — every SKU needs channel-specific titles, bullet points, long descriptions, feature callouts, and compliance attributes. Multiply that across 1,000+ products, 5 categories, multiple brands, and regular product refreshes, and you have a content operation that is permanently behind.

The traditional solution is more headcount — more copywriters, more content coordinators, more review cycles. This scales linearly with SKU count and is never fast enough for business needs.

The question I set out to answer: **Can we use GenAI to shift content production from a bottleneck to a capability?**

---

## My Hypothesis

GenAI doesn't replace content teams. It changes where they spend their time — from generating first drafts to reviewing, refining, and approving AI-generated output. If the AI can produce a structured, channel-compliant first draft at 80% quality, the human effort required drops dramatically.

The key insight: **GenAI is only as good as the governance structure around it.** Without clear attribute standards, content templates, and quality gates, AI-generated content is just fast garbage.

This is why the [PXM governance work](./pxm-governance-at-scale.md) came first.

---

## Approach

### Step 1 — Define the Content Use Cases for AI
Not all content is equal. I segmented content tasks by AI suitability:

| Content Type | AI Suitability | Rationale |
|---|---|---|
| Structured attribute descriptions | ✅ High | Templated, factual, bounded |
| Channel-specific bullet points | ✅ High | Rule-based, optimizable by channel |
| Long-form marketing copy | 🟡 Medium | Requires brand voice calibration |
| Technical spec sheets | ✅ High | Data-in, structured text-out |
| Regulatory / compliance text | ❌ Low | High risk, requires legal validation |
| Lifestyle and brand storytelling | ❌ Low | Requires creative brand judgment |

### Step 2 — Build the Prompt Architecture
Generic prompts produce generic content. I worked with the content team to engineer structured prompts that embedded:

- Brand voice guidelines (per brand — Whirlpool, Maytag, KitchenAid have different tones)
- Channel-specific requirements (Amazon character limits, Home Depot attribute schemas)
- Product attribute data pulled directly from Salsify (title, category, key specs, differentiators)
- Output format templates (structured JSON that maps directly to PIM fields)

Example prompt structure (sanitized):
```
You are a product content specialist for [BRAND]. 
Write Amazon bullet points for the following product:
- Product: {product_name}
- Category: {category}
- Key specs: {spec_list}
- Primary differentiator: {differentiator}

Requirements:
- 5 bullets, each starting with a capital benefit keyword
- Bullet 1 must lead with the primary differentiator
- Max 200 characters per bullet
- Tone: {brand_tone_guideline}

Return as JSON: {"bullets": ["...", "...", "...", "...", "..."]}
```

### Step 3 — Integration into PXM Workflow
AI-generated content needed to live inside the existing workflow, not alongside it:

- Built an integration between the AI content service and **Salsify** — generated content populates a "draft" field, not the live field
- Configured a **human review workflow** — content stewards review, edit, and approve before any field goes live
- Tracked AI-assist rate (% of final content that started as AI-generated draft) as a productivity metric
- Established a **feedback loop** — rejected or heavily-edited AI content was logged to improve prompt quality

### Step 4 — Quality Guardrails
Without guardrails, AI content creates new problems while solving old ones:

- **Hallucination detection**: Automated check that AI-generated specs match source attribute data in Salsify
- **Compliance scan**: Flagged content containing restricted claims (energy claims, comparative claims, unsubstantiated superlatives)
- **Brand voice scoring**: Simple keyword-based check for off-brand terminology
- **Channel validation**: Output validated against retailer schema before entering review queue

---

## Outcomes & Current State

This initiative is ongoing, but early results are directionally strong:

| Metric | Baseline | With AI Assist |
|---|---|---|
| Time to first draft (per SKU) | 45–60 min (human) | 2–3 min (AI) + 15 min review |
| Content backlog clearance rate | Behind by weeks | Catching up — backlog reduced |
| Content consistency score | Variable | More consistent across SKUs |
| Reviewer sentiment | N/A | Positive — less "blank page" problem |

The initiative has also surfaced an important insight: **the bottleneck shifted from content creation to content review.** This is a better problem to have — it's easier to scale review capacity than production capacity — but it requires deliberate workflow design.

---

## What This Initiative Taught Me

**AI adoption in enterprise is a change management problem, not a technology problem.**

The technology worked faster than the organization adapted. Content teams had concerns about job displacement. Legal had concerns about AI-generated claims. Brand teams had concerns about voice consistency. Each was legitimate.

The most important work I did wasn't prompt engineering. It was:
1. Being transparent about what AI would and wouldn't do
2. Designing workflows that kept humans in control of what goes live
3. Measuring productivity gains at the team level, not the individual level
4. Treating content stewards as partners in improving the AI output, not as gatekeepers to bypass

The initiative is stronger because of that friction — not despite it.

---

## What's Next

- Expanding AI-assist to additional content types (FAQ generation from spec data, feature comparison tables)
- Evaluating multimodal AI for image-attribute consistency checking
- Building a content quality feedback loop that improves prompts automatically from review decisions
- Documenting this as a repeatable playbook for other teams in the organization

---

## Technologies Used

`Salsify` · `OpenAI API (GPT-4)` · `Azure OpenAI Service` · `Python` · `Syndigo` · `Adobe Experience Manager` · `Custom integration layer`

---

*← [Back to Portfolio](../README.md)*
