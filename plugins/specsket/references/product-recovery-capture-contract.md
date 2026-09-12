# Recoverable product capture

Use only when live `specsket_get_capabilities` advertises `product_recovery_capture_contract` and the prepare, stage and status operations are callable. This contract saves incomplete review drafts; it does not claim source completeness, manufacturer verification, successful file transfer, or publication. Keep strict validation available when requested. Never convert an existing signed/project-aware submission into this envelope or use recovery to evade a hard integrity/access error.

## Prepare and confirm

Fetch the current product schema and live field catalog. Inspect accessible source material, retain literal observations and evidence, and identify one intended parent product per record. Do not invent variant combinations or drop unsupported sections just to make ingestion pass. Explain incomplete discovery or ambiguous parent boundaries; unresolved structure remains a review requirement.

An explicit request to ingest authorizes saving the preparation draft. Call `specsket_prepare_product_capture` with a `capture` object containing:

- `version`: the advertised capture version; currently `product-recovery-capture@1`.
- `capture_key`: a stable 8–200-character idempotency key for these exact inputs.
- `ingestion_mode`: `hybrid_review`.
- `schema_version`: the current supported product schema returned by Specsket, never a version guessed from these instructions.
- `taxonomy_versions`, `sources`, and `evidence_catalog`: current versions and the existing source/evidence envelope.
- `records`: ordered product objects. Prefer `external_record_id` and `fields`, where each current semantic field carries `{ value, evidence_refs }`. Preserve unresolved literal sections on that same object instead of fabricating accepted fields. Do not embed project, assessment, receipt, digest or checksum envelopes in a record. Non-object items are individually rejected.
- `destinations`: optional ordinal-based routes, each `{ ordinal, destination }`, using the current authorized destination contract. Platform-admin ambiguity stays unassigned; vendor and designer ownership stays bound to OAuth.

A host attachment ID or filename alone is not a stored Specsket file. Preserve available file metadata and extracted evidence. A Specsket artifact UUID is accepted only for an authorized, verified original upload with matching integrity metadata; never invent one. Host-local file IDs produce a visible missing-transfer issue. Tell the user when only extracted text/metadata was retained.

Preparation returns an immutable capture and signed handle, but no candidates. Show parseable/malformed counts, every unresolved issue, proposed destinations, and missing files. Ask for explicit confirmation immediately before staging. If the user changes content, structure or destinations, prepare new immutable inputs with a new key and obtain a fresh confirmation; never patch the signed handle.

## Stage and reconcile

Call `specsket_stage_product_capture` with the exact returned `capture_id`, `revision`, `input_digest`, `recovery_digest`, `validation_receipt`, and `destination_manifest`, plus one stable staging `idempotency_key`. Do not recopy product records. This operation handles admission, per-record persistence and completion; do not call legacy job creation/staging/completion tools for the capture.

Inspect all `record_results`. Successful siblings remain saved when another record fails. After a timeout or ambiguous response, call `specsket_get_product_capture`; retry only the same handle/key when the result permits it. Authentication, integrity, destination and revoked-access errors remain hard failures. Do not repeatedly retry operator-action or malformed records.

Return the permanent `review_url` and distinguish **saved candidates**, **needs input**, **retryable failures**, and **rejected items**. Frozen preparation issues are not current Wizard readiness; the in-app recovery panel evaluates the saved revision. Never describe a partially staged capture as fully successful or promise a failed file was imported.

Explain the next in-app actions: open **Needs input from ingestion**, inspect original values, edit/save Product Wizard fields, use **Review corrections** to account for source scope, and retry failed media where offered. Optional omissions require an explicit reason. Review confirmation records manual review, not manufacturer verification; approval remains a separate in-app action and is blocked by unresolved requirements or stale edits. Parent-count changes require a new capture. Do not use MCP to approve or publish.

Offer a short-lived signed-in review session only after the capture's underlying job is completed and the user separately confirms link creation. Always retain the permanent review URL as the fallback.
