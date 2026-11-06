# Source-control coverage matrix

Part of [the ingestion preview contract](ingestion-preview-contract.md). Assistants do not need to read it.

This matrix exists only to make repository tests detect drift between this instruction contract and the current source schema. It is not a submission allowlist and never overrides the schema fetched during a live job.

- Product Information: `information.name`, `information.description`, `information.sku`, `information.base_sku`, `information.reference_code`, `information.brand`, `information.manufacturer`, `information.collection`, `information.product_types`, `information.product_subtypes`, `information.functional_categories`, `information.style_tags`, `information.surface_application`, `information.zones`, `information.product_url`, `information.website`, `information.country_of_origin`, `information.lead_time`, `information.warranty`, `information.maintenance`, `information.representative_contact`, `information.supplier_contact`, `classification.csi`, `classification.omniclass`, `classification.uniformat`, `classification.uniclass`, `classification.unspsc`, `media.images`, `media.project_images`, `variants.items`
- Technical Data & Specification: `media.technical_drawings`, `technical.attributes`, `technical.specification_notes`, `technical.cards`, `technical.profile_key`, `technical.properties`, `technical.analysis_trace`, `technical.client_analysis_metadata`
- Instruction: `installation.instructions`
- Download: `downloads.documents`, `downloads.technical_files`
- Compatible Products: `compatible.relationships`
- Ask BATE: `bate.question_options`
