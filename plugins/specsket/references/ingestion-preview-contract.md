# Product ingestion preview contract

Use this contract for the pre-validation product preview. The preview is an evidence audit, not a shortened status summary and not a substitute for `specsket_validate_records`.

## Ordering and schema authority

Complete bounded official-source discovery before declaring the preview complete. Fetch the `current` product schema and, when advertised, the product presentation contracts. The fetched response is authoritative: render one row for every returned product field, including fields whose value is missing, uncertain, not applicable, or supplied only as presentation feedback. Never omit a field because it is empty, unfamiliar, server-generated, or absent from the source-control coverage matrix below.

Organize the rows under the actual live Specsket detail tabs, in this order:

### Product Information

Include identity, description, SKU/reference, brand/manufacturer/collection, supplier information, supplier and representative contacts, product/manufacturer URLs, taxonomy and classifications, origin, maintenance, warranty, lead-time/delivery evidence, applications, variants, product/project media, and any other current-schema field consumed by this tab.

### Technical Data & Specification

Include technical attributes, profile properties and metadata, specification notes, technical cards, drawings, evidence search/analysis trace, and every other current-schema field consumed by this tab.

### Instruction

Include every current-schema installation, assembly, care, or instruction field consumed by this tab.

### Download

Include every current-schema document and technical-file field consumed by this tab, including unavailable or protected document references.

### Compatible Products

Include every current-schema relationship field consumed by this tab.

### Ask BATE

Include every current-schema question or prompt field consumed by this tab.

Do not create a seventh pseudo-tab for classifications, media, variants, unresolved fields, or evidence. Place each field under the tab that consumes its current schema `section`; Product Information owns current `information`, `classification`, `media`, and `variants` semantics even when the field ID uses one of those prefixes.

## Required field row

Every field row must show all of the following:

1. exact current schema field ID and label;
2. literal detected value, or a literal item count followed by the detected values when the field is plural;
3. proposed normalized or mapped value, preserving the original literal alongside any normalization;
4. evidence and observation status, with exact source locators and conflicts rather than a bare status word;
5. scope or applicability, including parent, product, family, variant, combination, market, document, or unknown scope as relevant;
6. next action, either `none` or a bounded action naming the official source types or URLs to check, the mapping decision needed, or the vendor confirmation required.

Never show only `verified`, `partially_discovered`, a confidence score, or another status without the literal detected value or count. A normalized value must not overwrite or paraphrase away its detected source literal.

Use `not_found`, `insufficient_evidence`, `conflicting_evidence`, `not_applicable`, or `requires_vendor_confirmation` for unresolved fields. Show the state as the literal value when no supported value exists, explain the evidence already checked, and list only bounded manufacturer-controlled sources that will be searched next. Only `verified` observations carry a proposed product value.

## Images

For each image-bearing field, show the total detected image count and counts grouped by parent/product and by each documented variant or combination. Then list every image as a clickable credential-free HTTPS source URL with its proposed role, parent/variant association, evidence locator, and any duplicate, ownership, or relevance uncertainty. Counts never replace the URL list. Do not treat a room scene, sibling product, other collection, swatch, drawing, or teaser image as a product image without showing that uncertainty.

## Documents and applicability

Follow bounded manufacturer-controlled product-page links to PDFs, declarations, certifications, installation documents, maintenance documents, warranties, catalogues, and other prominent technical files. For every discovered document, show:

- the exact detected title;
- a clickable credential-free HTTPS file URL when available and a clickable source-page URL when the file is protected, missing, or gated;
- document type and candidate Download-tab destination;
- availability state such as `available`, `registration_required`, `login_required`, `request_from_manufacturer`, `broken_or_missing`, or `not_found`;
- one preserved applicability classification: `product-specific`, `family-level`, `material-class`, `manufacturer-platform`, `market-specific`, `expired/stale`, or `unrelated`;
- the exact product, family, material class, manufacturer platform, or market label it applies to when known;
- evidence/status, last-checked date when available, and the next action.

Applicability and availability are separate states. Preserve both through the preview, checkpoint, normalized document/reference object, validation payload, and staging payload. Do not silently promote a family, material-class, manufacturer-platform, market-specific, expired/stale, or unrelated document to product-specific. Never attach a corporate certificate or another collection's certificate as the product's compliance evidence.

When the current document contract accepts `applies_to_scope`, use only an exactly supported and evidenced scope: `sku`, `product`, `product_family`, `collection`, or `manufacturer`. These storage scopes do not replace the richer applicability classification. If no supported scope represents `material-class`, `market-specific`, `expired/stale`, or `unrelated` exactly, retain that classification in the preview, evidence catalog, and checkpoint, mark the proposed attachment as blocked or excluded, and do not coerce it into a product scope merely to pass validation.

Keep screenshots supplied as presentation feedback outside field evidence unless a screenshot independently contains readable product evidence with an exact locator. If it does, cite only the independently evidenced content; do not treat the screenshot's visual arrangement as proof of a product value.

### Certification discovery and accounting

Expand every manufacturer-controlled certification accordion, modal, tab, selector panel, and other interactive certification surface visible on the official product or collection page. Capture labels before normalization so repeated labels remain countable occurrences. When a label opens a gated modal rather than a file, retain the official source page, the gate text, and the actual availability state; never manufacture a per-certificate URL.

For a configurable product, build a certification coverage matrix over every distinct selector state represented by the documented combinations. The matrix must name the selector tuple or combination/SKU IDs checked and the literal certification labels returned. Identical selector states may share one normalized coverage state only when the official UI was checked and proved identical. A collection-level panel is not proof that each SKU has independently certified status.

Before the preview can be complete, account for every detected certification occurrence in exactly one outcome:

- `normalized`: retained as an evidence-backed certification statement or Download reference;
- `excluded`: not attached, with the exact applicability reason;
- `unresolved`: retained with the evidence already checked and one bounded next action.

Show the literal count and names for detected, normalized, excluded, and unresolved occurrences. Preserve availability and applicability independently. Repeated labels may normalize to separate references when they represent separate official entries; do not deduplicate them by label alone. Corporate or manufacturer certifications remain `manufacturer-platform`/`manufacturer` evidence, even when listed on a collection page. Gated, unavailable, or unlinked labels remain visible in the accounting instead of disappearing because no direct file exists.

## Completion gate

The preview is complete only when every field returned by the fetched current schema appears exactly once under a consuming live tab, every plural field shows its literal count and values, every image/document URL is clickable, unresolved fields have explicit states and bounded next searches, document availability plus applicability remain distinct, and the certification discovery/accounting gate is satisfied. Present the immutable topology choice only after this gate passes. Perform final validation only after the user accepts or overrides that topology and the resulting presentation envelope is rebuilt.

## Source-control coverage matrix

This matrix exists only to make repository tests detect drift between this instruction contract and the current source schema. It is not a submission allowlist and never overrides the schema fetched during a live job.

- Product Information: `information.name`, `information.description`, `information.sku`, `information.base_sku`, `information.reference_code`, `information.brand`, `information.manufacturer`, `information.collection`, `information.product_types`, `information.product_subtypes`, `information.functional_categories`, `information.style_tags`, `information.surface_application`, `information.zones`, `information.product_url`, `information.website`, `information.country_of_origin`, `information.lead_time`, `information.warranty`, `information.maintenance`, `information.representative_contact`, `information.supplier_contact`, `classification.csi`, `classification.omniclass`, `classification.uniformat`, `classification.uniclass`, `classification.unspsc`, `media.images`, `media.project_images`, `variants.items`
- Technical Data & Specification: `media.technical_drawings`, `technical.attributes`, `technical.specification_notes`, `technical.cards`, `technical.profile_key`, `technical.properties`, `technical.analysis_trace`, `technical.client_analysis_metadata`
- Instruction: `installation.instructions`
- Download: `downloads.documents`, `downloads.technical_files`
- Compatible Products: `compatible.relationships`
- Ask BATE: `bate.question_options`
