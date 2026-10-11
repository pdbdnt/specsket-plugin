# Product image and grouping readiness contract

Use this contract whenever live capabilities advertise `product-readiness@1`. It is a staging-readiness rule for standalone products and output parents, not a substitute for discovery scope, semantic fields, presentation media, or variant applicability.

## Research boundary

Fast discovery may retain image-less or ambiguously grouped leads only in an explicitly unresolved section. Do not present them as ready recommendations. Ready extraction and Deep-to-ingestion handoff must resolve the rule before validation, regardless of technical assessment results. User acknowledgement alone does not waive it: recover exact evidence, use an attributable authorized image, or revise the selected scope and preview it again.

## Meaningful output parents

Capture source parent identity, every selected orderable identity, manufacturer base name, source design/model/decor/texture distinctions, and evidence before constructing output parents. Derive semantic groups before checking supported option tuples.

Retain source-controlled ordering distinctions even when Specsket does not expose them as selectable axes. Split only for an evidenced unsupported ordering distinction, explicit user topology choice, or the live combination limit. Every selected identity must belong to exactly one output parent. Never omit, duplicate, sample, Cartesian-expand, or treat missing discriminator evidence as equality.

Each output parent must retain its immutable source/output parent IDs, exact member identities, split reason, evidenced discriminator keys and values, manufacturer base name, and source-supported display name. Stable identity comes from the source group, not request row position. Do not invent anonymous `Part N`, `Lane N`, `Bucket N`, or `Identity Group N` product names. A genuinely manufacturer-authored numbered name is allowed only with exact naming evidence. If no truthful supported partition and name exists, leave the topology unresolved.

## One parent primary image

Every output parent requires exactly one `owner_kind: product`, matching `owner_id`, `role: primary` media reference. Gallery images follow it in canonical order, and `media.images` must contain that same primary first followed by the same gallery order. The variant primary selector is separate and never satisfies this rule.

Prefer an official product hero or packshot with exact ownership evidence. When no parent hero exists, an evidenced member-variant image may be explicitly nominated as representative; retain the represented orderable identity and do not imply identical appearance across siblings. Do not automatically promote the first variant image. One physical asset may serve both evidenced roles without duplicating its bytes.

Scenes, swatches, drawings, logos, siblings, filenames, URL slugs, proximity, and visual similarity do not prove product ownership. Split design/decor parents require evidence for their own image. Preserve ownership provenance separately from fetch availability.

## Preview, validation, and staging

Checkpoint the selected primary, media reference, association kind, represented variant when applicable, ownership evidence, grouping provenance, and unresolved reasons. Show names, split reasons, exact member counts, selected primaries, and unresolved items before confirmation.

Pass the exact `product-readiness@1` sidecar through validation and staging and bind its per-record digests in job creation. Missing, extra, changed, ambiguous, or mismatched records must fail. Staging must use Specsket's supported safe media path; a transient fetch or decode failure is retryable and cannot create an accepted image-less candidate. Legacy records remain readable but do not gain current readiness status.

Keep parent-primary readiness separate from variant-image coverage. Preserve every non-image field and verify semantic submission through pending-import summary and full Product Wizard. A changed image, group, member set, or name requires revalidation; a changed immutable topology requires the corresponding new scope/job lifecycle.
