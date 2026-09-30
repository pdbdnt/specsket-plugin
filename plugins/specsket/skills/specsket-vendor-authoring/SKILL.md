---
name: specsket-vendor-authoring
description: Create or maintain a permitted Specsket company, storefront, owned media, collections and vendor products through the connected MCP, including reviewed publication and recovery.
---

# Specsket vendor authoring

Use this workflow for company/storefront maintenance and direct vendor-product authoring. Read the vendor identity and storefront section of [the workflow contract](../../references/workflow-contract.md) before writes. Product-ingestion candidates, supplier proposals and Project Intelligence retain their separate review workflows.

1. Read live capabilities and inspect the tools actually callable in this conversation. Select the exact permitted company and current role; a flag alone does not make a missing tool callable. Return the supported native handoff when an operation is unavailable.
2. Read canonical company settings, private draft, products and readiness. Resolve identity and country before creation. A pending company is not activated, claimed or published; company creation never creates a human account.
3. Prepare exact scoped changes and show their effects, sources and immutable review target/revision/digest. Use existing explicit user authorization when it covers those effects; request confirmation only for missing scope. Canonical settings, draft saves, media imports, product publication and storefront publication have different effects.
4. Apply through enabled reviewed MCP operations. Preserve expected versions, owned storage slots, manufacturer/market evidence and stable idempotency keys. Validate product/media custody before linking into collections. Read receipts and saved state rather than assuming success from tool discovery or a prepared review.
5. Show authenticated private preview and readiness before publication. Confirm public effects within the user's authorization, then verify the returned publication IDs and anonymous visibility separately. Do not claim image decoding or browser verification unless actually performed.
6. After interruption or reconnect, read exact operations/reviews/receipts before retry. Reuse a key only for the same request and current access; reread/review conflicts instead of overwriting newer edits.

Existing-member role changes/removal require separate governance permission. Claims need human authority proof; no automatic account creation, invitation send, ownership transfer or inherited brand access is supported. Never bypass missing scope with service-role credentials, direct database writes or copied browser tokens.
