# Client-side product analysis contract

Use this contract for user-provided catalogs, spreadsheets, PDFs, documents, and product pages when capabilities advertise `analysis_location: client` and the live product schema is `product@3`.

## Analysis boundary

ChatGPT owns semantic analysis. Specsket owns authenticated schema/taxonomy discovery, deterministic validation, exact-payload receipts, staging, and review routing. Do not call server specification-analysis jobs in this mode.

## Required analysis sequence

1. Inventory every file the host actually exposes and create stable source IDs. For an official URL, inventory the supplied page and bounded relevant manufacturer-controlled product/document links automatically.
2. Build a preliminary product/source-identity index and source graph before detailed extraction; distinguish manufacturer SKU, item/article/product/catalogue/order/reference number, and canonical variant URL identity, then classify single product, parent with variants, mixed collection, listing, document, or non-product.
3. Link the same product, variant, finish, accessory, drawing, and supporting document across files.
4. Group only genuinely equivalent products into a family.
5. Create one dynamic profile per family and narrowly scoped product extensions only when evidence requires them.
6. Search the supplied files and bounded official sources for every expected property and current-schema field.
7. Preserve exact evidence locators and explicit unresolved states.
8. Render the complete field-and-evidence preview under the actual product detail tabs according to the [ingestion preview contract](ingestion-preview-contract.md), including literal values or counts, clickable image/document URLs, and distinct document availability and applicability states.
9. Check family consistency, duplicate product identities, conflicts, and unsupported values.
10. Present and record the immutable topology choice only after the preview completion gate passes.
11. Save a versioned checkpoint after every completed family.

Do not infer orderable combinations by calculating the Cartesian product of separate option lists. If the page omits order identifiers, inspect prominent official datasheet and order-table rows before declaring the matrix unavailable. Preserve the literal manufacturer identifier type; a canonical official variant URL is a valid typed source identity but its slug is not automatically a manufacturer SKU. After the complete preview, recommend a product topology from documented identity relationships, show its record and combination counts, and preserve a user override in the checkpoint. Prove that every orderable identity has one unique tuple across only Colors, Dimensions, and Material Finish. If an orderable distinction cannot use those supported axes, or two identifiers collapse to the same supported tuple, split the topology instead of mislabeling the axis. SKU/article/code identity and synthetic documented-state axes are never selectable presentation axes.

## Property versus value inference

Domain reasoning may determine that a property belongs in a family profile. It may not supply the product's value. A value is `verified` only when exact evidence exists. Representation-only normalization must preserve meaning. Typical industry values, model knowledge, or neighboring products are never substitutes for product evidence.

## Checkpoint

Create a downloadable JSON checkpoint containing:

- format and method version;
- canonical checkpoint digest;
- source manifest;
- product graph and unresolved links;
- proposed and selected product topology, exact combination matrix, and presentation record digests;
- family profile catalog;
- completed and unresolved records;
- evidence catalog;
- taxonomy search intents;
- client-reported phase timings when available.

Verify the digest before resuming. When the host cannot create or retain a durable artifact, use smaller user-controlled batches and say that resume depends on the user's downloaded copy.

## Observation states

- `verified`: exact value and evidence exist.
- `not_found`: recorded supplied source types were checked and no value was found.
- `insufficient_evidence`: limited evidence exists but does not support a verified value.
- `conflicting_evidence`: at least two cited sources disagree.
- `not_applicable`: the property does not apply, with a reason.
- `requires_vendor_confirmation`: supplied evidence cannot resolve the property.

Only verified observations carry a value. Never fabricate a value to improve completeness.

## Source-qualified QA and unresolved observations

For an explicitly authorized synthetic QA fixture, retain fictional authorship and the exact approved source ID in the source manifest, evidence catalog and analysis trace. A fixture author's classification of its fictional PDF as `technical_datasheet` may use that existing enum only when that classification is documented and the PDF was actually checked; it does not turn the file into manufacturer evidence. Do not invent a QA enum or relabel a fictional source as a manufacturer product page, supplier authority or certificate. Preserve the source's actual byte hash, locator and supplied qualification, and keep source byte verification distinct from canonical source-object binding.

For a `not_found` observation, omit `value`, `value_origin` and `derivation`, retain its declared profile `property_key`, actual evidence anchors and qualifying note, and list the nonempty checked source types. The record's `technical.analysis_trace` must contain each listed `source_type`, a `source_ids` entry that resolves to the same source manifest, and the truthful outcome. Use `checked` when the supplied PDF was checked but the property's value was absent; do not claim the source file was missing. For example, a documented fictional QA technical datasheet contributes `checked_source_types: ["technical_datasheet"]` and a trace with `source_type: "technical_datasheet"`, the same actual PDF source ID, `outcome: "checked"`, and a note identifying fictional QA authorship. Preserve `conflicting_evidence` without a chosen value and retain both cited anchors; do not downgrade or resolve a conflict for structural validation. Every profile property still needs one explicit state; family/method and normalized classification context must match the record. The outer technical-property evidence must equal the exact nested union.

Source classification and a successful validator do not prove native preservation, managed document/image transfer, private saving or publication. Report those outcomes separately from client analysis and candidate staging under the workflow contract; leave unavailable progress counts `unknown` when the live response returns `null`.

## Capability and original-receipt boundaries

Capability descriptors, actual callable tools, prepared/committed/closed original receipts, native readback and business authorization are separate proof boundaries; use [the durable original workflow](workflow-contract.md#durable-original-chunk-staging) without rewriting dynamic profile/snapshot/context digests or immutable source/evidence bindings to make recovery fit. The scope's `capability_digest` binds the advertised planning limits, not a hash of the entire capability JSON. Optional additive metadata does not authorize new scope/job/staging work or prove installed-client reliability.

If optional Deep/server assessment is disabled, unavailable or not callable, preserve the complete requested extraction, variants, documents and exact evidence, and label any client-only analysis honestly. Do not call it Specsket-assessed or invent a server completeness result. Client analysis, deterministic validation, private candidate review, native saving and publication remain distinct observed outcomes.
