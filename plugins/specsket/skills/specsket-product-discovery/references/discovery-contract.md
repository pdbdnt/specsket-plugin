# Specification-aware discovery contract

## Profile composition

Specsket composes profiles from maintained universal, domain, product, and application modules selected by the live product context. A bounded model analysis may propose genuinely product-specific additions, but it cannot override maintained definitions, criticality, or weights. Novel additions remain recommended with weight 1.

Expected properties are guidance for evidence collection, not blindly required database fields. A tile can expect slip resistance, water absorption, abrasion, chemical resistance, fire classification, installation, maintenance, and warranty; a luminaire instead expects electrical, photometric, ingress, controls, and lifetime data. Unknown categories use the safe universal profile.

## Source escalation

Use the ordered source types returned in the profile snapshot. The sequence is flexible by category and may include manufacturer product pages, technical datasheets, catalogues, declarations of performance, test certificates, installation guides, maintenance guides, warranty documents, authoritative regional suppliers, and vendor confirmation.

Record each checked source type and outcome. Only use `not_found` after the relevant recorded sources were checked. Use `insufficient_evidence` when evidence exists but its scope or quality cannot verify the exact product. Use `conflicting_evidence` when authoritative sources disagree, and preserve at least two references plus the conflict note. Use `not_applicable` only when product context makes the property genuinely inapplicable. Use `requires_vendor_confirmation` when only the vendor can resolve a remaining gap.

## Scoring

Only `verified` applicable properties earn their maintained weight. `not_applicable` is removed from the denominator. Every unresolved state—`not_found`, `insufficient_evidence`, `conflicting_evidence`, and `requires_vendor_confirmation`—earns zero. Specsket returns both overall and critical completeness so marketing-rich records cannot conceal missing safety or performance information.

The snapshot ID and digest bind the exact owner-scoped maintained profile. The deterministic assessment digest also binds the complete observation set and ordered search trace. Never calculate an official score from a locally invented checklist or alter weights in chat.

## Evidence integrity

Every verified property requires exact evidence. Preserve value, unit, test or classification system, product, series, manufacturer-platform, or material-class scope, and evidence references. Never upgrade a manufacturer-platform claim into product verification. The outer technical-property field must include the union of every nested evidence reference, including all references for a conflict.
