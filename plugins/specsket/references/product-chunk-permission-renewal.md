# Renew an untouched original chunk's staging permission

This is a permission-only recovery for an existing durable original. It does not replace incomplete-draft capture recovery, reconcile an uncertain write, dispatch records, approve a product, or publish anything.

## Establish availability and eligibility

Call `specsket_get_capabilities` through the existing configured connection. Require `product_chunk_stage_intent.enabled: true`, its `renewal_operation: specsket_renew_product_chunk_staging_permission`, and that exact tool in this chat's actual callable inventory. The advertised scope is `untouched_prepared_generation_zero`. Also require the supported original read and dispatch tools for the intended complete journey. A capability, installed plugin version, or successful health check does not create a callable handle.

If the capability or renewal metadata is absent or disabled, do not invent renewal support. Existing authorized reads and ordinary unprepared validation retain their current contracts. If the server advertises renewal but its tool is missing, use only the host's supported refresh/restart of the existing MCP connection or a new conversation where supported, then check capabilities and callable tools again. Report the connection limitation if no supported refresh is available. Do not reset OAuth, create another client, switch to hosted MCP, use a privileged API, or bypass storage gates to obtain the missing operation. A demonstrated authentication rejection requires its separate supported recovery; missing attachment alone is not that proof.

Read `specsket_get_product_chunk_stage_intent` and `specsket_get_ingestion_job` with the exact original IDs under current authorization. Preserve the original operation and job, chunk index and idempotency key, request/content digests, execution/partition, expected job version, immutable `stage_request`, historical validation receipt and checkpoint history. This branch is eligible only for the unchanged `prepared` original at `claim_generation: 0`, with no claim, result, native effect, custody or caller history. The server checks the complete scaffold and current actor, OAuth client, integration authorization/epoch, source and storage authority after its locks. Public status, zero counters or a next action alone do not prove those conditions. Any historical caller, including an expired or revoked one, denies this minimal branch; do not delete history or revoke/recreate grants to force eligibility.

When returned, `staging_permission_state: expired` or `source_changed` with `next_action: renew_staging_permission` identifies a possible renewal action, not an authorization or effect-absence proof. `historical_receipt_invalid` / `review_original_validation` requires governed review of the original validation; never reinterpret an invalid historical signature as elapsed expiry. Running, uncertain or completed generations continue on their existing read/reconciliation paths.

## Renew the same original

Explain the permission-only effect and follow the existing authorization and write-confirmation rules. Call only:

- `job_id`: the unchanged original job.
- `operation_id`: the unchanged original operation.
- `expected_intent_updated_at`: the exact current original `updated_at` returned by its supported read.
- `renewal_idempotency_key`: one stable renewal key, 8–200 characters. This is separate from the original chunk idempotency key, which remains unchanged.

Do not submit replacement records, destinations, receipts, credentials, actor/grant overrides, a chosen expiry or a claim generation. The server revalidates the saved semantics under current rules and appends a separate audited permission; it does not rewrite the original receipt, payload, digests, history or CAS. Return only the safe permission ID, revision, expiry, original operation ID and renewal key exposed by the response.

After an uncertain renewal response, read the same original's `staging_permission` and state. If still eligible, the exact same renewal key may reconcile/replay only under current authorization; it returns the canonical permission and never makes an expired canonical permission fresh. A later distinct renewal key is a new permission write and must again satisfy the untouched/no-effect checks and existing confirmation rules. Neither renewal nor a new key resolves an ambiguous dispatch.

## Continue the original workflow

Read the same original and verify the canonical permission, unchanged identity/digests and job CAS. Renewal itself creates no caller, media or candidate. When fresh local storage admission is required, use the existing normal validation and `specsket_prepare_product_ingestion_records` workflow with the exact retained semantic payload, job, chunk key and content digest. A new validation JTI/expiry can differ while the deterministic preparation binding must still match the server's renewal preparation. Do not copy private credentials or manufacture a preparation. Changed schema, mapping, warnings, destination, topology or semantic binding requires governed resolution; do not edit the immutable original to bypass denial.

Supported storage admission is separately authorized and must match current preparation, source, storage commit and epoch. Renewal does not revive an expired approval, caller, grant or execution/scope deadline and does not authorize a new scope. Keep one writer for the operation and its fixture/media custody. After those gates pass, dispatch the same named original once with only its original `job_id` and `operation_id`. The server binds the current permission revision at first claim and rechecks authority and expiry through native/media effects; clients never substitute the permission credential into the saved request.

After an ambiguous dispatch, reconcile the same original job/chunk/key/digests; do not renew to bypass uncertainty, blindly redispatch, fall back to ordinary record staging or create another operation/job/key. Reopen the original receipt and saved private candidate through supported reads. Report actual accepted/rejected results, native verification and permanent review URLs separately. A renewal receipt, capability advertisement or candidate count alone does not establish successful ingestion, approval, publication or a finished reliability release.
