# Live product field catalog contract

The public plugin never owns or hard-codes the authoritative Specsket product field list. The connected Specsket MCP returns the principal-aware live catalog through `specsket_get_product_research_contract`.

## When the user asks about Specsket fields

For questions such as “What Specsket fields do you have?”, “Which Product Wizard fields can you search?”, or “Show the product schema by tab”:

1. Call `specsket_get_capabilities` and then `specsket_get_product_research_contract`.
2. Require `field_catalog.version` to be present. Do not reconstruct the hierarchy from examples, cached plugin text, raw UI knowledge, or semantic IDs alone.
3. Report the active product contract and its field count. Keep principal-supported compatibility counts, presentation-descriptor count, and contextual dynamic-property status separate; never combine them into one misleading total.
4. Render every active public semantic field in the returned order:
   - Product Wizard tab
   - section
   - friendly field label
5. Distinguish separately:
   - compatibility-only semantic fields;
   - structured presentation descriptors such as options, Dimensions thickness, exact combinations, and scoped Technical Data;
   - contextual dynamic technical properties, which expand only inside the Ready request-authored profile or Deep server-profile workflow.
6. Friendly labels come first. Include machine field IDs only when the user asks for technical identifiers or when a compact optional technical column helps implementation.
7. Never silently truncate the catalog. Use compact grouped tables for a long response and offer the same live data as JSON when useful.

If the MCP call fails or an older research response has no `field_catalog`, say that the current organized field list cannot be confirmed. Cached examples may be described only as non-authoritative and potentially stale. Do not claim that the plugin itself contains the current list.

The catalog is descriptive and read-only. It is not an entity submission, presentation envelope, validation receipt, or staging authorization.
