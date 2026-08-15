---
name: specsket-product-ingestion
description: Normalize user-provided spreadsheets, PDFs, documents, or product pages into Specsket product fields with exact evidence, then validate and stage them in the connected vendor or designer-private product-review queue. Use when the user asks to import, map, normalize, sync, or push products into Specsket from files or source URLs.
---

# Specsket product ingestion

Use this skill only for the `product_ingestion` workflow and `product` entity.

Read [the workflow contract](../../references/workflow-contract.md) and [the evidence contract](../../references/evidence-contract.md) before calling write tools.

Call `specsket_get_capabilities` first. If that tool or the **Specsket MCP Tools Beta** connection is unavailable, stop the workflow and explain that the public plugin supplies instructions but live Specsket access requires a separate Streamable HTTP MCP connection to `https://integrations.specsket.com/mcp`, followed by Specsket OAuth and a new chat. Do not infer the user's identity, permissions, destination, schemas, or live Specsket state, and do not claim that anything was validated or staged.

Require `product_ingestion.validate` before normalizing and `product_ingestion.stage` before offering to stage. Fetch `product@1` through `specsket_get_entity_schema`; never infer field IDs from UI labels or old examples.

Inspect every provided file available in the current conversation. Preserve the source filename, MIME type, original-byte SHA-256 when the host exposes the bytes, and exact per-field locator. Use taxonomy search for functional categories and CSI rather than freehand IDs.

Validate the complete submission. Resolve every error through the same schema/taxonomy/validation path. Warnings may be presented to the user but must not be silently discarded.

Before calling any write tool, summarize the record count, source files, warnings, connected destination, and the fact that the result enters product review rather than publication. For a vendor principal, name the target vendor. For a designer principal, state that the records enter that designer's private review queue and cannot be assigned to a vendor. Explain that, after completion, Specsket will create a 60-second one-time link that signs the browser into the same Specsket account connected through OAuth; if the browser currently uses another Specsket account, the user must explicitly confirm the switch. Ask once for explicit confirmation of create, stage, complete, signed-in link creation, and the browser-open attempt.

After confirmation, create the job, stage the exact validated records, inspect all results, and complete the job. Always print the permanent `review_url`. Then call `specsket_create_review_session` with the completed `job_id` and the first accepted `domain_record_id` the user should review. Print an **Open signed-in Specsket review** link using `browser_launch_url`, note its expiry, and ask the host to open that URL in the visible in-app browser when available. Never say that the browser opened unless the browser action returned success. If browser opening is unavailable or fails, keep the signed-in link clickable and repeat the permanent fallback URL. Never put the browser ticket anywhere else or claim it is permanent.

Never call supplier onboarding tools for a product error. Never publish, approve, directly mutate product tables, or claim an import succeeded before job completion returns a review URL. If the signed-in link expires, create another only after the user asks to open the completed review again.
