---
name: specsket-product-ingestion
description: Normalize user-provided spreadsheets, PDFs, documents, or product pages into Specsket product fields with exact evidence, then validate and stage them in the connected vendor or designer-private product-review queue. Use when the user asks to import, map, normalize, sync, or push products into Specsket from files or source URLs.
---

# Specsket product ingestion

Use this skill only for the `product_ingestion` workflow and `product` entity.

Read [the workflow contract](../../references/workflow-contract.md) and [the evidence contract](../../references/evidence-contract.md) before calling write tools.

Call `specsket_get_capabilities` first. If that tool or the Specsket MCP connection is unavailable, stop the workflow and explain that the public plugin supplies instructions but live Specsket access requires a separate Streamable HTTP MCP connection to `https://integrations.specsket.com/mcp`, followed by Specsket OAuth and a new chat. Do not infer the user's identity, permissions, destination, schemas, or live Specsket state, and do not claim that anything was validated or staged.

Require `product_ingestion.validate` before normalizing and `product_ingestion.stage` before offering to stage. Fetch `product@1` through `specsket_get_entity_schema`; never infer field IDs from UI labels or old examples.

Inspect every provided file available in the current conversation. Preserve the source filename, MIME type, original-byte SHA-256 when the host exposes the bytes, and exact per-field locator. Use taxonomy search for functional categories and CSI rather than freehand IDs.

Validate the complete submission. Resolve every error through the same schema/taxonomy/validation path. Warnings may be presented to the user but must not be silently discarded.

Before calling any write tool, summarize the record count, source files, warnings, connected destination, and the fact that the result enters product review rather than publication. For a vendor principal, name the target vendor. For a designer principal, state that the records enter that designer's private review queue and cannot be assigned to a vendor. Ask for explicit confirmation. After confirmation, create the job, stage the exact validated records, complete it, and return the review URL.

Never call supplier onboarding tools for a product error. Never publish, approve, directly mutate product tables, or claim an import succeeded before job completion returns a review URL.
