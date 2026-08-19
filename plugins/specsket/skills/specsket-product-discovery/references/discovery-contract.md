# Specification-aware discovery contract

## Discovery modes

Fast shortlist is a bounded research pass for orientation labeled **Preliminary shortlist - not Specsket-assessed**: up to five products, official sources where available, and explicit `availability_verified` or `availability_unverified` regional status. It does not require MCP access and never claims Specsket profile resolution or completeness.

Deep assessment is MCP-backed. Limit the default deep set to two selected candidates, batch the two initial analyses before profile-directed research, then batch the two final analyses before deterministic assessment. Honor each job's `retry_after_seconds` polling guidance.

## Profile composition

Specsket composes maintained universal, domain, product, and application modules from live product context. A bounded model analysis may propose genuinely absent product-specific additions, but it cannot override maintained definitions, criticality, weights, units, or test systems.

Canonical property aliases belong to profile definitions. Map observations to a compatible maintained property before accepting an analysis addition. Remove duplicate semantic additions from the assessment denominator. Preserve a novel addition only when no compatible maintained concept exists. Conflicting canonical and alias evidence must remain `conflicting_evidence`.

Expected properties are flexible guidance, not universal database requirements. Tile, lighting, plumbing, furniture, HVAC, and unknown products must receive different applicable profiles.

## Research completion loop

Use an initial analysis to resolve the profile. Let its expected properties and ordered source escalation drive deeper evidence search. Re-run analysis with expanded evidence, then request deterministic assessment. Do not run this expensive loop for every plausible product in a broad discovery pass.

When `research_readiness.status` is `needs_escalation`, continue relevant `next_source_types` before concluding a serious recommendation. A source attempt may end as `checked`, `not_found`, `blocked`, or `not_applicable`; unavailable outcomes require reasons. Stop only when research is `ready_to_present` or the remaining action is `vendor_confirmation_required`. Do not search irrelevant source classes merely to exhaust the list.

## Evidence states and scoring

Only `verified` applicable properties earn maintained weight. `not_applicable` leaves the denominator. `not_found`, `insufficient_evidence`, `conflicting_evidence`, and `requires_vendor_confirmation` earn zero and retain their distinct meanings.

Every verified property requires exact evidence. Preserve value, unit, test/classification system, scope, and evidence references. Never upgrade manufacturer-platform or material-class claims into product verification. Never invent a test method.

The server owns snapshot and assessment digests. Do not calculate an official score locally, alter weights, or rebuild `product_v2_fields`.

## Mandatory presentation

Present the complete server `specification_report` for each serious candidate:

- profile title/key and classification;
- overall completeness, critical completeness, and risk;
- every status count and missing critical property;
- grouped expected properties with exact states and verified values;
- searched source types, evidence links, research status, and next action;
- separate design suitability and procurement/regional availability.

Visualization may supplement but never replace this accessible evidence-backed report.

## Discovery boundary

Discovery and assessment are read-only. Validation and ingestion begin only after explicit selection. If evidence or search trace changes before staging, request a new assessment and copy its returned `product_v2_fields` unchanged.
