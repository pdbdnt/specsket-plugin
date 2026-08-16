---
name: specsket-product-discovery
description: Research, compare, and recommend products using category-aware specification profiles, profile-directed source escalation, exact evidence states, supplier availability, and deterministic Specsket completeness before ingestion. Use when a user asks to find, source, research, shortlist, recommend, compare, or look for alternatives among building products, materials, fixtures, equipment, or suppliers.
---

# Specsket product discovery

Use this workflow before ingestion so products are judged on whether they are specifiable, not only whether they look suitable.

Read [the discovery contract](references/discovery-contract.md), [the workflow contract](../../references/workflow-contract.md), and [the evidence contract](../../references/evidence-contract.md).

## Connect the live contract

Call `specsket_get_capabilities` first. For authoritative category profiles and completeness, require `product_discovery.read`, `product_discovery.analyze`, and `product_discovery.assess`.

If the MCP is unavailable or either analysis capability is false, continue only as **Provisional research coverage (not Specsket-assessed)**. Repeat that exact label beside every provisional comparison or coverage summary. Do not calculate or display a percentage completeness score, critical-completeness score, risk tier, profile name, or weighted ranking from a client-created checklist. State the unavailable capability and explain that authoritative completeness requires Specsket MCP Tools Beta, OAuth, enabled capabilities, and a new chat after activation.

## Complete profile-directed research

1. Translate the brief into design intent, constraints, project stage, geography, application, and procurement needs.
2. Research broadly, then choose only serious candidates for specification analysis.
3. Resolve likely product type plus functional and CSI classifications where supported.
4. For each serious candidate, collect initial exact evidence and an ordered search trace. Start and poll `specsket_start_product_specification_analysis`.
5. Use the returned server profile as the source of truth for expected properties and source escalation. Search specifically for unresolved technical performance, documentation, installation, maintenance, warranty, standards, certifications, and regional availability.
6. Re-run analysis with the expanded evidence and a new stable idempotency key, then call `specsket_assess_specification_completeness`.
7. If `research_readiness.status` is `needs_escalation`, do not present the candidate as finished. Continue through relevant `next_source_types`, or record each unavailable source as `not_found`, `blocked`, or `not_applicable` with a reason. Re-run analysis and assessment whenever evidence or the search trace changes.
8. Stop escalating only when the assessment is `ready_to_present`, or `vendor_confirmation_required` is the honest next action. Do not exhaust irrelevant source classes blindly.
9. Preserve every expected-property state exactly: `verified`, `not_found`, `insufficient_evidence`, `conflicting_evidence`, `not_applicable`, or `requires_vendor_confirmation`.

Never infer performance values from material norms, related products, industry assumptions, or unnamed test systems. Series evidence must remain series scope. Manufacturer-wide claims must use `manufacturer_platform` scope and remain `insufficient_evidence` unless exact product evidence supports verification. Preserve all conflicting references.

## Present the complete server report

For every serious candidate, present the returned `specification_report`; do not compress it into highlights alone. Show:

- product, manufacturer, exact variant/SKU, authoritative URL, profile/category, and classification;
- design/brief suitability;
- procurement and regional availability;
- overall completeness, critical completeness, and risk;
- all six status counts and missing critical properties;
- grouped identity, physical, surface, performance, application, installation, lifecycle, documentation, classification, and genuine additional properties;
- each property's exact state, verified value/unit/test system when present, and material evidence note;
- searched source types, evidence links, research status, and next evidence action.

Keep design suitability, specification completeness, and procurement availability separate. Do not manufacture numeric design or procurement scores. For multiple products, show a compact comparison first, then each candidate's major critical/important gaps.

If compatible visualization is available, use it when it materially improves comparisons, completeness, profile hierarchy, variants, imagery, or supplier geography. Visualization is supplemental: preserve the full accessible evidence-backed report and never invent values, scores, or imagery.

## Keep discovery read-only

Do not validate, stage, ingest, publish, or approve during discovery. Wait for explicit user intent such as “stage this” or “add #2 to Specsket.”

When selected, use `specsket-product-ingestion`. Carry forward the exact evidence catalog and copy server-returned `product_v2_fields` unchanged. If any observation or search-trace entry changes, reassess instead of recreating a digest. Staging remains review-only and never publishes or approves.
