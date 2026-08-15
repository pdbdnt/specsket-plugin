# Integration workflow contract

1. Call `specsket_get_capabilities`; stop if the requested workflow is unavailable.
2. Fetch the current entity schema. Use only returned semantic field IDs.
3. Fetch current taxonomy versions and resolve taxonomy-backed values with `specsket_search_taxonomy`.
4. Normalize the attached sources and build exact field evidence.
5. Call `specsket_validate_records`.
6. If validation reports errors, correct the records and validate again. There is no alternate write path.
7. Show the normalized record count, warnings, and destination review queue to the user. A vendor product job targets that vendor's queue; a designer product job targets only the connected designer's private queue. Obtain explicit confirmation before any write tool.
8. Create one ingestion job with a stable idempotency key and expected record count.
9. Stage only the exact validated submission with its returned digest and receipt. Keep chunk indexes and idempotency keys stable across exact retries.
10. Inspect every `record_results` entry. Correct rejected records using their stable code; do not hide them behind aggregate counts or use another write path.
11. Complete the job and return its review URL. Completion only opens human review.

For a timeout or unknown result, query the job before retrying. Retry only with the same idempotency key and byte-equivalent semantic payload. For stale schema, taxonomy, receipt, changed input, authorization, or capability errors, fetch the current contract and repeat the failed step on the same canonical path.
