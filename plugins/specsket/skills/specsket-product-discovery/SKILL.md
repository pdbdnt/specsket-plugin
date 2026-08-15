---
name: specsket-product-discovery
description: Research, compare, and recommend products using category-aware specification profiles, explicit evidence gaps, supplier availability, and deterministic Specsket completeness before ingestion. Use when a user asks to find, source, research, shortlist, recommend, or compare building products, materials, fixtures, equipment, or suppliers for a project.
---

# Specsket product discovery

Use this workflow before ingestion so products are judged on whether they are specifiable, not only whether they look suitable.

Read [the discovery contract](references/discovery-contract.md), [the workflow contract](../../references/workflow-contract.md), and [the evidence contract](../../references/evidence-contract.md).

## Connect the live contract

Call `specsket_get_capabilities` first. For authoritative category profiles and completeness, require `product_discovery.read`, `product_discovery.analyze`, and `product_discovery.assess`.

If the Specsket MCP is unavailable, or if either `product_discovery.analyze` or `product_discovery.assess` is false, product web research may continue only as **Provisional research coverage (not Specsket-assessed)**. Repeat that exact label beside every provisional comparison or coverage summary. Do not calculate or display a percentage completeness score, critical-completeness score, risk tier, profile name, or weighted ranking from a client-created checklist. Use verified/missing property lists without an official percentage. Never shorten this label to "Spec completeness" or imply that product@2 schema availability means server assessment is enabled.

Do not invent a Specsket profile, score, schema, identity, or permission. State which live capability is unavailable and include the relevant capability warning. Explain that authoritative completeness requires **Specsket MCP Tools Beta**, OAuth, all three product-discovery capabilities, and a new chat after connection or server activation.

## Research before recommending

1. Translate the brief into design intent, product constraints, project stage, geography, application, and procurement needs.
2. Identify the likely product type and resolve functional and CSI classifications when possible.
3. Research a broad set, then select only serious candidates for deeper analysis. Prefer manufacturer evidence and authoritative regional suppliers.
4. For every serious candidate, collect evidence and an ordered search trace. Call `specsket_start_product_specification_analysis` with a stable idempotency key, then poll `specsket_get_product_specification_analysis` until it succeeds or fails.
5. Follow the returned profile and flexible source-escalation plan. Search for the expected technical properties, documents, installation information, maintenance, warranty, standards, certifications, and regional availability. The plan depends on category, product type, classification, application, and project stage; never apply tile properties to unrelated products.
6. Mark each expected property as `verified`, `not_found`, `insufficient_evidence`, `conflicting_evidence`, `not_applicable`, or `requires_vendor_confirmation`. “Not found” means not found in the recorded checked sources, not that the information does not exist. Use `insufficient_evidence` for manufacturer-platform, material-class, or otherwise inadequate evidence that cannot verify the exact product. Use `conflicting_evidence` when authoritative sources disagree, preserving at least two exact references.
7. Call `specsket_assess_specification_completeness` with the server-issued snapshot ID and digest, the complete evidence-backed observation set, and the ordered search trace. Preserve the returned assessment digest and `product_v2_fields`. Never submit client-created weights or replace the server profile.
8. Present the comparison before creating any Specsket record. Separate design fit, specification completeness, critical completeness, documentation quality, supplier/region availability, and classification confidence.

Never infer slip resistance, water absorption, fire performance, ingress protection, flow rate, structural capacity, or another performance value from material norms or a related product. Series-level evidence must be labeled as series scope. Manufacturer-wide claims must use `manufacturer_platform` scope and remain `insufficient_evidence` unless exact product evidence supports verification. Conflicting evidence must remain `conflicting_evidence` and preserve all conflicting references.

## Present useful results

For each candidate show:

- product, manufacturer, exact variant/SKU, and authoritative URL;
- design-brief fit and regional supplier/availability evidence;
- resolved profile/classification;
- overall and critical completeness returned by Specsket;
- verified highlights;
- missing critical and recommended properties;
- specification risk and next evidence action.

When multiple candidates are involved, use a compact comparison table. Rank by the user's project stage and stated priorities; do not manufacture a numeric design or procurement score.

If a compatible visualization capability is available, use it when it materially improves candidate comparison, critical-vs-recommended gaps, profile hierarchy, variants, or supplier geography. Use source-backed product imagery only. A visualization supplements the evidence-backed text and must not invent values or scores. If unavailable, use an organized table and concise property groups.

## Hand off to ingestion

Wait for the user to select a product. Do not create speculative records for every candidate.

When the user asks to stage a selected candidate, use `specsket-product-ingestion`. Carry forward the exact evidence catalog and copy the server-returned `product_v2_fields` into the record unchanged. If any observation or search-trace entry changes, call the assessment tool again rather than recreating its digest. Revalidate against the current schema and profile; discovery results are not a staging receipt. Staging remains review-only and never publishes or approves a product.
