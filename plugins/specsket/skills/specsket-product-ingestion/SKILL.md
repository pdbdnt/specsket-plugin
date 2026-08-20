---
name: specsket-product-ingestion
description: Normalize user-provided spreadsheets, PDFs, documents, or product pages into Specsket product fields with exact evidence, then validate and stage them in the connected vendor or designer-private product-review queue. Use when the user asks to import, map, normalize, sync, or push products into Specsket from files or source URLs.
---

# Specsket product ingestion

Use this skill only for the `product_ingestion` workflow and `product` entity.

Read [the workflow contract](../../references/workflow-contract.md) and [the evidence contract](../../references/evidence-contract.md) before calling write tools.

Analyze attached client files before making product-specific MCP calls. Follow [the client analysis contract](../../references/client-analysis-contract.md), create resumable family checkpoints for large batches, and keep every value evidence-bound. A local path alone does not make files available to ChatGPT; inspect only files the host actually exposes.

When the staging package is ready, call `specsket_get_capabilities`. If that tool or the **Specsket MCP Tools Beta** connection is unavailable, stop the live staging phase and explain that the public plugin supplies instructions but live Specsket access requires a separate Streamable HTTP MCP connection to `https://integrations.specsket.com/mcp`, followed by Specsket OAuth and a new chat. Do not infer the user's identity, permissions, destination, schemas, or live Specsket state, and do not claim that anything was validated or staged.

Require `product_ingestion.validate` before normalizing and `product_ingestion.stage` before offering to stage. Fetch the `current` product schema through `specsket_get_entity_schema`; never hard-code a schema version or infer field IDs from UI labels or old examples.

When capabilities advertise `analysis_location: client`, do not call specification-analysis start, poll, or assessment tools. ChatGPT owns file inspection, cross-file product linking, family grouping, dynamic profile creation, and evidence mapping. Infer which properties are relevant to the family, but never infer an unsupported property value. Preserve `not_found`, `insufficient_evidence`, `conflicting_evidence`, `not_applicable`, and `requires_vendor_confirmation` explicitly.

Group only genuinely equivalent product contexts. Submit each family profile once in the request-level `product_profile_catalog`; each record references its `technical.profile_key` and carries record-specific observations, trace, metadata, and evidence. Use only the live schema's generated submission contract and limits.

When the current schema is `product@3`, include every profile property's explicit record state, value origin, value, unit, test/classification system, scope, and nested evidence references. The outer field evidence must be the exact union of all nested references, including every conflicting reference. Do not submit snapshot IDs or profile/assessment digests; validation calculates and signs those outputs. Only `unit_conversion@1` may be submitted as derived, and only when the generated live contract can be satisfied exactly.

Inspect every provided file available in the current conversation. Preserve the source filename, MIME type, original-byte SHA-256 when the host exposes the bytes, and exact per-field locator. Use taxonomy search for functional categories and CSI rather than freehand IDs.

Validate the complete submission. Resolve every error through the same schema/taxonomy/validation path. Warnings may be presented to the user but must not be silently discarded. Immediately before each staged chunk, refresh its validation receipt when less than 120 seconds remain; unchanged revalidation does not need a second confirmation, but any changed warnings, mappings, assessment, destination, or count does.

Before calling any write tool, summarize the record count, source files, warnings, connected destination, and the fact that the result enters product review rather than publication. For a vendor principal, name the target vendor. For a designer principal, state that the records enter that designer's private review queue and cannot be assigned to a vendor. Explain that, after completion, Specsket will create a 60-second one-time link that signs the browser into the same Specsket account connected through OAuth; if the browser currently uses another Specsket account, the user must explicitly confirm the switch. Ask once for explicit confirmation of create, stage, complete, signed-in link creation, and the browser-open attempt.

After confirmation, create the job, stage the exact validated records, inspect all results, and complete the job. Always print the permanent `review_url`. Then call `specsket_create_review_session` with the completed `job_id` and the first accepted `domain_record_id` the user should review. Print an **Open signed-in Specsket review** link using `browser_launch_url`, note its expiry, and ask the host to open that URL in the visible in-app browser when available. Never say that the browser opened unless the browser action returned success. If browser opening is unavailable or fails, keep the signed-in link clickable and repeat the permanent fallback URL. Never put the browser ticket anywhere else or claim it is permanent.

Never call supplier onboarding tools for a product error. Never publish, approve, directly mutate product tables, or claim an import succeeded before job completion returns a review URL. If the signed-in link expires, create another only after the user asks to open the completed review again.
