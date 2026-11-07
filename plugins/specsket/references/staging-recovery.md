# Staging recovery: when an outcome occurs

Part of [the workflow contract](workflow-contract.md); every rule there applies.

## When the 502 is the app, not the MCP

A `502 integration_upstream_transport_failed` from every tool while the MCP health check is fine means the Specsket app behind it is not reachable. Stop, do not retry writes, and tell the user the app needs to be started; ask whoever runs the local Specsket runtime to restart the app.

## Untouched original permission renewal

When `product_chunk_stage_intent.enabled` is true, `renewal_operation` names `specsket_renew_product_chunk_staging_permission`, and that exact tool is callable, follow [the original permission renewal contract](product-chunk-permission-renewal.md). Read the same original and job first; use only `job_id`, `operation_id`, exact `expected_intent_updated_at` and a stable `renewal_idempotency_key`. The server revalidates unchanged prepared generation-zero work and appends separate audited permission while preserving the original operation/job/key/digests/receipt/history/CAS. No claim, effect, custody or historical caller is allowed; current actor/client/authorization epoch and source/storage checks still apply. Renewal creates no candidate, revives no grant, and does not authorize storage or dispatch. Reconcile an uncertain renewal through the same original and exact renewal key; reconcile an uncertain dispatch through its existing custody path. Missing or disabled metadata and missing callable handles never authorize renewal or an API fallback.

## Known pre-native no-effect disposition

When live capabilities advertise `no_effect_disposition_operation: specsket_resolve_product_chunk_preflight_no_effect` and the exact action is callable, follow [the no-effect disposition contract](product-chunk-preflight-no-effect.md). It applies only to the immutable known first-generation `needs_action` runtime refusal after complete no-effect/writer/custody and current-authority checks. Use the exact original CAS and stable disposition key; append the canonical receipt and fail only its empty job without changing the terminal original or allocating a successor. Unknown outcomes require the same original/job canonical readback or exact same-key authorized replay. An expired historical permission or revoked caller grants no current staging authority; never redispatch the failed original. A separately reviewed fresh job may proceed only after resolution, current duplicate/source checks and its own normal workflow.

## Canonical zero-stage closure

Classify a no-stage original as terminal zero-effect closure only when its normal canonical intent readback retains the original IDs/bindings and returns `status: needs_action`, `result.code: zero_stage_no_effect_closed`, `result.phase: resolved`, and `next_action: prepare_fresh_execution_or_job`, with the matching original job `status: failed` and `failure_summary: zero_stage_no_effect_closed`. Report accepted/rejected counts separately; zero counts alone never prove closure. Verify the original key, digests, generation and job/partition identity against the preserved receipt through supported authorized reads. This conjunction is the canonical governed closure receipt for that original only; no fabricated stage or delete receipt is required or permitted.

The public projection deliberately omits private journal fields. Do not demand secret custody data or infer closure of another unresolved operation from this receipt. No separate client closure tool is promised, and clients must not dispatch an unknown original to manufacture closure. If the canonical proof is denied, unavailable, contradictory or incomplete, retain `unknown` and seek governed resolution. Do not dispatch a terminal original again. The returned next action describes a possible separately authorized fresh execution/job; it does not authorize preparation, writes, business resumption or completion of the remaining client/native reliability gates.

An original whose stage exists but was already natively deleted is not zero-stage. Do not classify or retry it here; stop and follow [existing-delete custody reconciliation](existing-delete-custody-reconciliation.md), a separate platform-admin operator procedure with its own grant.
