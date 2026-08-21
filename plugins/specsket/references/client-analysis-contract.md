# Client-side product analysis contract

Use this contract for user-provided catalogs, spreadsheets, PDFs, documents, and product pages when capabilities advertise `analysis_location: client` and the live product schema is `product@3`.

## Analysis boundary

ChatGPT owns semantic analysis. Specsket owns authenticated schema/taxonomy discovery, deterministic validation, exact-payload receipts, staging, and review routing. Do not call server specification-analysis jobs in this mode.

## Required analysis sequence

1. Inventory every file the host actually exposes and create stable source IDs. For an official URL, inventory the supplied page and bounded relevant manufacturer-controlled product/document links automatically.
2. Build a preliminary product/SKU index and source graph before detailed extraction; classify single product, parent with variants, mixed collection, listing, document, or non-product.
3. Link the same product, variant, finish, accessory, drawing, and supporting document across files.
4. Group only genuinely equivalent products into a family.
5. Create one dynamic profile per family and narrowly scoped product extensions only when evidence requires them.
6. Search the supplied files for every expected property.
7. Preserve exact evidence locators and explicit unresolved states.
8. Check family consistency, duplicate product identities, conflicts, and unsupported values.
9. Save a versioned checkpoint after every completed family.

Do not infer orderable combinations by calculating the Cartesian product of separate option lists. Recommend a product topology from documented identity and SKU relationships, show its record and combination counts, and preserve a user override in the checkpoint. If an orderable distinction cannot use Colors, Dimensions, or Material Finish, split the topology instead of mislabeling the axis.

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
