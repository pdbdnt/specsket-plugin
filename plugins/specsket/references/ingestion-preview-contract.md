# Product ingestion preview contract

Use this contract for the pre-validation product preview. The preview is an evidence audit, not a shortened status summary and not a substitute for `specsket_validate_records`.

## Ordering and schema authority

Complete bounded official-source discovery before declaring the preview complete. Fetch the `current` product schema and, when advertised, the product presentation contracts. The fetched response is authoritative: render one row for every returned product field, including fields whose value is missing, uncertain, not applicable, or supplied only as presentation feedback. Never omit a field because it is empty, unfamiliar, server-generated, or absent from the source-control coverage matrix below.

When the current capability exposes the read-only ingestion planner, include its complete limit preflight in the preview: parent record IDs and counts, combinations per parent, immutable job structure and unresolved-node count, deterministic partition proposal, repeated structure-evidence/body cost, logical media roles, exact URL-digest duplicate count, expected record-local fetches, workload warnings, and separate `fits_record`, `fits_job`, and `fits_chunk` results. Plugin-only agents that cannot compute `product_structure_digest` skip the planner, show `fits_record`, `fits_job` and `fits_chunk` as `unknown`, and size chunks as in the strict sequence (1-2 heavy products per partition). A local validation chunk without the complete intended job binding must display `fits_job: unknown`; never present local chunk success as whole-job feasibility.

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

## Technical Data modelling: groups, cards and attributes

Choose the Technical Data container by the shape of the source, not by convenience. Container and field names below are the ones the live `product-presentation` and `product` entity schemas use; confirm them against the live schema and field catalog before building records.

- **Parameter (technical) groups** hold key/value facts: a label with a short value, typically from a specification table, performance row, dimensions list or "Technical properties" block (for example `Tensile strength = 1.8 N/mm2`, `Pile height = 3.0 mm`). Build presentation `technical_groups` with one group per source table or section heading, each property carrying its literal label, value, unit when stated and exact evidence, and the group scope (`all_variants`, or `variant_combinations` when the value differs by variant). Bind each group in the applicability sidecar with a `technical_group` occurrence. Do not store key/value rows as technical cards.
- **Technical cards** hold narrative: multi-sentence prose, usage or advantage statements, storage, health and safety text, approval or compliance statements, notes. A card is not a substitute for a group row.
- **Technical attributes** hold only the specific scalar facts that the live field catalog defines as dedicated attributes. If a fact has such a dedicated attribute, use it; every other labelled value goes in a parameter group. When in doubt, use a group.
- **Footnotes and test-condition notes** belong in `technical.specification_notes`, linked to the rows they qualify, not in cards or groups.
- A value that is a vendor typo, a header fragment, a shifted table cell or contradicts another official source is not `verified`: mark the property `conflicting_evidence` or `insufficient_evidence`, record the conflict with both evidence locations, and show the literal printed value together with the reason.

Before staging a vendor's first batch, when the review queue already holds a saved or approved candidate for a comparable page shape, compare the plan with it. If none exists, state that and continue with the contract rules alone; never fabricate a comparison. An existing candidate is a reference for structure, not proof of correctness: the contract and live schema win. Differences in container type (groups versus cards), variant structure, image association, documents and applicability must be explained or fixed before continuing.

## Field coverage and source documents

Coverage is measured against the live catalog (`field_catalog.entries` / `active_field_ids` from `specsket_get_product_research_contract`), not against what a generator happens to emit. Before validation, walk every active field whose `discovery_policy` is `always_search` or `search_when_applicable` (fields marked not searchable or profile-dynamic, such as `bate.question_options`, `technical.profile_key`, `technical.client_analysis_metadata`, `technical.analysis_trace` and `technical.properties`, follow their own rules) and give each exactly one outcome: a source-backed value, or an explicit researched state (`not_found`, `insufficient_evidence`, `conflicting_evidence`, `not_applicable`, `requires_vendor_confirmation`) with a note naming the sources checked. Record the researched state where the live contract supports it (the preview field row, the checkpoint disposition, or a `technical.properties` observation); omitting a field is never an outcome.

`always_search` fields (product name, SKU, brand, manufacturer, product types, images, variants, description, surface application, product URL, technical attributes, documents, technical files) must be researched on every product. `search_when_applicable` fields (subtypes, functional categories, collection, style tags, project images, zones, website, country of origin, lead time, warranty, maintenance, representative and supplier contact, technical drawings, technical cards, classification codes, specification notes, instructions, compatible products) must be researched whenever the page, a linked document or the vendor's site could carry them, and left empty only with a sourced reason. The platform validator itself blocks only on the product bundle shape, evidence on every populated field, `information.name`, and the readiness primary image when that capability is advertised; the coverage rule here is a plugin requirement on top of that, and a batch is not complete while any in-scope active field is unaccounted for.

Read the documents, not just the links. Open and extract text from every official document that is linked from the product page and names this product, its family or its material class (datasheets, TDS/PDS, catalogues, brochures, EPD/HPD, certificates, installation and maintenance guides, warranty terms), whether or not the page has an orderable matrix, and use them as evidence for collection, origin, packaging, ratings, standards, warranty, installation, maintenance and conflicts with the page. Follow the vendor's site-wide download, documents, certifications or accreditation pages once per vendor when the product page links to them. Do not record a field such as country of origin, warranty or an EPD as `not_found` while a linked or site-wide document that could answer it has not been read. A scanned or image-only PDF that cannot be read is recorded as `insufficient_evidence` with an "image-only document" note, never as absent data.

Field routing rules:
- **SKU.** A manufacturer-printed product number, item number, article number or product code that is labelled as such and identifies the orderable item (in an ordering table, on a datasheet or on the product page) is the manufacturer SKU and the variant identity; use the matching `manufacturer_*` source identity kind. A code that identifies the whole family or collection goes to `information.base_sku` or `information.reference_code` instead. Do not turn a product name or a colour number into a SKU. Use a generated identity only when no labelled code is printed anywhere official.
- **Documents versus technical files.** `downloads.documents` holds user-facing downloadable references (brochures, catalogues, certificates, EPD/HPD, SDS, installation or maintenance guides, warranty terms, linked document folders). `downloads.technical_files` holds technical datasheets and engineering files (TDS, PDS, CAD, BIM). Each linked file belongs to exactly one of the two, with its applicability scope (product-specific, family-level, material-class, manufacturer-platform).
- **Contacts.** A company phone, email or address printed in the page footer or contact section is the supplier contact (and the representative contact when it says so). Use the Product Wizard contact keys the live schema reads: `company`, `name`, `phone`, `mobile`, `email`, `address`.
- **Zones and surfaces.** Rooms and building types ("offices, meeting rooms, hotels", indoor or outdoor use) go to `information.zones`; `information.surface_application` holds the surfaces the product is applied to (for example Floor, Wall, Ceiling).
- **Project images and drawings.** Photographs of installed projects and the product's additional gallery views go to `media.project_images`; they are not discarded as lifestyle imagery. Technical drawings go to `media.technical_drawings`. Project galleries often sit on a separate projects page: cite that page when it is the source.
- **Related and accessory products** named on the product page go to `compatible.relationships` when the target can be identified; otherwise record why they were not mapped.

## Images

For each image-bearing field, show the total detected image count and counts grouped by parent/product and by each documented variant or combination. Then list every image as a clickable credential-free HTTPS source URL with its proposed role, parent/variant association, evidence locator, and any duplicate, ownership, or relevance uncertainty. Counts never replace the URL list. Identify the one selected product-owned primary for every output parent and show its ownership evidence; if it represents one member variant, name that exact identity without implying sibling coverage. Do not treat a room scene, sibling product, other collection, swatch, drawing, or teaser image as a product image without showing that uncertainty.

**Parent primary image (required when the page shows one).** A product page almost always displays one main product image: the large gallery or carousel image (the default or active slide) beside the title and specification block. That image is the product's own image on its own official page, even when it is a rendered room scene or installed-use photo of this exact product, so it is the parent primary and goes first in `media.images` (role primary). Take it from the rendered page (the slide that is actually displayed by default, not an `og:image` social card or a slide chosen by convenience), verify that it loads, and cite the DOM element that shows it. The product has exactly one main product image in the Product Wizard. The other slides or gallery images of the same product (extra views, room renders) go to `media.project_images`, where the wizard shows them under Project Images; they are not extra `media.images` entries. When the page does not say which variant a slide shows, do not attach it to a variant. Colour, finish or size swatches are selector-value (variant) images and are never the parent primary. `media.project_images` takes the product's additional views and installation or project photos; the product page's own main image is never put there. A candidate that has variant images but no parent primary image while its page displays a product image is an extraction defect: fix it before staging. This applies whether or not `product-readiness@1` is advertised, because the live completion list requires a product-owned primary image. When the page itself has none but one member variant has a clear product image, you may nominate it as a representative member image only as the readiness contract describes (explicit representative association naming the represented member, never an automatic promotion); otherwise leave the parent image empty and record the reason. Only when the page truly shows no product-owned image (logo, banners, galleries of other products) leave the parent image empty and record that reason.

Separate physical source accounting from logical coverage. Identify canonical parent/product images, selector-value images, exact combination or SKU overrides, project/application images, technical drawings, duplicates, exclusions, and unresolved combinations. For each documented combination, show whether its resolved image is an exact evidenced override, an evidenced inheritance from the contract-declared primary selector value, an explicitly allowed parent fallback, or unresolved. The sum of those four combination states must equal the documented enabled-combination count. A selector thumbnail whose filename contains a representative SKU is selector evidence unless the official UI or document binds it to the exact combination.

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

Document entries may be obtained files, direct official file URLs, or source-page references with no file. The latter still require a title, description, availability state, official URL, scope, and evidence. Keep every Download section capable of both shared and exact-combination applicability; do not restrict SKU-specific scope to certifications. The same all/specific/unresolved model applies to installation entries and compatible-product relationships, while non-document premium technology and technical features remain in Technical Data & Specification.

When `product-variant-applicability@1` is advertised, the preview must account for every discovered occurrence in Technical Data groups/cards, Installation, Download, and Compatible Products. Show its stable occurrence ID, content evidence, applicability evidence, normalized/excluded/unresolved outcome, and exact combination/SKU coverage. This sidecar is automatic for all ingestion sources and must not contain manufacturer-specific logic.

### Certification discovery and accounting

Expand every manufacturer-controlled certification accordion, modal, tab, selector panel, and other interactive certification surface visible on the official product or collection page. Capture labels before normalization so repeated labels remain countable occurrences. When a label opens a gated modal rather than a file, retain the official source page, the gate text, and the actual availability state; never manufacture a per-certificate URL.

For a configurable product, build a certification coverage matrix over every distinct selector state represented by the documented combinations. The matrix must name the selector tuple or combination/SKU IDs checked and the literal certification labels returned. Identical selector states may share one normalized coverage state only when the official UI was checked and proved identical. A collection-level panel is not proof that each SKU has independently certified status.

Before the preview can be complete, account for every detected certification occurrence in exactly one outcome:

- `normalized`: retained as an evidence-backed certification statement or Download reference;
- `excluded`: not attached, with the exact applicability reason;
- `unresolved`: retained with the evidence already checked and one bounded next action.

Show the literal count and names for detected, normalized, excluded, and unresolved occurrences. Preserve availability and applicability independently. Repeated labels may normalize to separate references when they represent separate official entries; do not deduplicate them by label alone. Corporate or manufacturer certifications remain `manufacturer-platform`/`manufacturer` evidence, even when listed on a collection page. Gated, unavailable, or unlinked labels remain visible in the accounting instead of disappearing because no direct file exists.

## Completion gate

When live capabilities enable batch-wide discovery scope, begin this gate with the request's `cap_mode`, cap unit, optional identity-kind filter, and completeness scope. Show discovered parent count, submitted parent count, resulting output/split-parent count, exact manufacturer-SKU count, canonical source-identity count and counts by identity kind, documented and submitted combination counts, excluded and unresolved identity counts, source-frontier status, split lineage, and the shared scope digest. Keep semantic validation and staging status separate. A successful record validation never upgrades discovery or parent-topology completeness.

Before declaring this preview complete, show the capability-advertised coverage receipt and, when enabled, the `product-readiness@1` record for every output parent. New governed chunks use `product-source-coverage@2`; legacy `@1` records display `Legacy scope unverified` and never claim parent completeness. Include:

- every exact inspected official product-detail URL as a clickable link;
- selector mode, axes, literal option pools, and documented/inspected/unresolved state counts;
- typed source identities for every inspected state, preserving resolved manufacturer SKU/item/article/product/catalogue/order/reference identifiers when exposed and canonical official variant URL identity otherwise;
- detected and accounted document occurrences, separated by available, gated, request-only, missing, normalized, excluded, and unresolved status;
- detected and accounted certification occurrences, distinguishing certificate files, unlinked or request-only references, test/compliance claims, and manufacturer certifications;
- for configurable records, the one-to-one typed-identity reconciliation between inspected selector states and enabled presentation combinations, explicit manufacturer-versus-generated SKU origin, plus each normalized product-specific certification's bound applicability occurrence, exact combination IDs, and SKU aliases;
- every preserved conflict and bounded next action;
- the final `complete` or `incomplete` accounting state.

Source coverage is complete when every discovered occurrence is accounted for. It does not imply that a gated or request-only file was downloaded. An incomplete receipt may be checkpointed, but it cannot proceed to topology recommendation, validation, or staging. Configurable products cannot be complete while any documented selector state remains unresolved.

The preview is complete only when every field returned by the fetched current schema appears exactly once under a consuming live tab, every plural field shows its literal count and values, every image/document URL is clickable, unresolved fields have explicit states and bounded next searches, document availability plus applicability remain distinct, and the certification discovery/accounting gate is satisfied. Present the immutable topology choice only after this gate passes. Perform final validation only after the user accepts or overrides that topology and the resulting presentation envelope is rebuilt.

## Source-control coverage matrix

This matrix exists only to make repository tests detect drift between this instruction contract and the current source schema. It is not a submission allowlist and never overrides the schema fetched during a live job.

- Product Information: `information.name`, `information.description`, `information.sku`, `information.base_sku`, `information.reference_code`, `information.brand`, `information.manufacturer`, `information.collection`, `information.product_types`, `information.product_subtypes`, `information.functional_categories`, `information.style_tags`, `information.surface_application`, `information.zones`, `information.product_url`, `information.website`, `information.country_of_origin`, `information.lead_time`, `information.warranty`, `information.maintenance`, `information.representative_contact`, `information.supplier_contact`, `classification.csi`, `classification.omniclass`, `classification.uniformat`, `classification.uniclass`, `classification.unspsc`, `media.images`, `media.project_images`, `variants.items`
- Technical Data & Specification: `media.technical_drawings`, `technical.attributes`, `technical.specification_notes`, `technical.cards`, `technical.profile_key`, `technical.properties`, `technical.analysis_trace`, `technical.client_analysis_metadata`
- Instruction: `installation.instructions`
- Download: `downloads.documents`, `downloads.technical_files`
- Compatible Products: `compatible.relationships`
- Ask BATE: `bate.question_options`
