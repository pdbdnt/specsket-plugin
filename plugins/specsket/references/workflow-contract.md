# Integration workflow contract

1. Call `specsket_get_capabilities`; stop if the requested workflow is unavailable.
2. Fetch the current entity schema. Use only returned semantic field IDs. Do not pin an old schema version.
3. Fetch current taxonomy versions and resolve taxonomy-backed values with `specsket_search_taxonomy`.
4. For products, resolve the category-aware specification profile before normalization, follow its source-escalation plan, preserve explicit missing states, and calculate authoritative completeness against its snapshot. Normalize the attached sources and build exact field evidence.
5. Call `specsket_validate_records`.
6. If validation reports errors, correct the records and validate again. There is no alternate write path.
7. Show the normalized record count, warnings, and destination review queue to the user. A vendor product job targets that vendor's queue; a designer product job targets only the connected designer's private queue. Explain that the confirmed workflow will create, stage, and complete the job, then create a 60-second one-time browser link that signs the browser into the same Specsket account authorized through OAuth. If another Specsket account is already active in that browser, Specsket will require an explicit account-switch confirmation. Obtain one explicit confirmation for that sequence before any write tool.
8. Create one ingestion job with a stable idempotency key and expected record count.
9. Stage only the exact validated submission with its returned digest and receipt. Keep chunk indexes and idempotency keys stable across exact retries.
10. Inspect every `record_results` entry. Correct rejected records using their stable code; do not hide them behind aggregate counts or use another write path.
11. Complete the job and always return its permanent, non-secret review URL. Completion only opens human review.
12. Call `specsket_create_review_session` for the completed job. For a product job, pass the accepted candidate the user should see first. Immediately show both the permanent `review_url` and the short-lived signed-in browser link. Ask the host to open `browser_launch_url` in its visible in-app browser when that capability is available. Never claim the browser opened unless the browser action actually succeeded. If browser opening is unavailable or fails, give the user a clearly labeled **Open signed-in Specsket review** link plus the permanent fallback URL. Do not expose, persist, or reuse an expired browser link; create another only after the user asks to open the review again.

For a timeout or unknown result, query the job before retrying. Retry only with the same idempotency key and byte-equivalent semantic payload. For stale schema, taxonomy, receipt, changed input, authorization, or capability errors, fetch the current contract and repeat the failed step on the same canonical path.
