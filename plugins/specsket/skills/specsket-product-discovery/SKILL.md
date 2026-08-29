---
name: specsket-product-discovery
description: Research, compare, and recommend building products in either a fast preliminary shortlist or a deep Specsket-assessed comparison. Use when a user asks to find, source, research, shortlist, recommend, compare, or find alternatives among products, materials, fixtures, equipment, or suppliers.
---

# Specsket product discovery

Read [the discovery contract](references/discovery-contract.md), [the workflow contract](../../references/workflow-contract.md), [the field catalog contract](../../references/field-catalog-contract.md), and [the evidence contract](../../references/evidence-contract.md).

When checking a supplied URL or an official product page found during research, follow the workflow contract's visible source review: open it in the in-app browser by default, select desktop site or desktop viewport mode and the largest practical panel when those controls exist, keep the product currently being checked visible, and leave the most relevant official product page open for the user. Do not use mobile mode or device emulation unless the user requests mobile verification or desktop presentation is unavailable. Run structured web research and Specsket analysis alongside this visible pass; do not wait for a fetch failure before opening the browser.

Expand manufacturer-controlled certification accordions, download modals, tabs, selector panels, and other visible interactive certification surfaces. Record every literal label, including duplicate occurrences, and keep availability separate from applicability. A modal, gate, or unlinked label is still a discovered reference; never invent a certificate URL or promote a corporate certificate to product-specific evidence. For configurable products, compare every distinct certification selector state represented by documented combinations and reuse one state only after proving the official results are identical.

## Explain available Specsket fields

When the user asks which Specsket product fields are available, this is a live contract question rather than product discovery. Call `specsket_get_capabilities` and `specsket_get_product_research_contract`, then follow the field catalog contract. Show every active public field grouped by the returned Product Wizard tab and section, friendly labels first, with compatibility, presentation, and contextual dynamic fields separated. Do not silently truncate or present a hard-coded example as current.

## Choose the depth before using tools

For an underspecified request, ask at most three short numbered questions. Put the recommended answer first and always offer to proceed with reasonable assumptions. Ask only about choices that materially affect the shortlist, such as application, geography, budget tier, or required performance.

Use **Fast shortlist** by default for requests such as “find modern tiles in Egypt.” Use **Specsket-ready extraction** when the user wants evidence mapped to current Specsket fields or preparation for review staging. Use **Deep Specsket assessment** when the user explicitly wants authoritative completeness, critical-gap analysis, or server-directed technical research for one or two finalists. When the intent is ambiguous between these depths, ask one concise depth question.

### Fast shortlist

- Research broadly across manufacturers; do not assume a preferred manufacturer unless the user names one.
- When web research is available, make one bounded discovery pass and return up to five plausible products.
- Prefer official manufacturer product pages and technical documents. Supplier or distributor pages may support regional availability but are not product-performance proof.
- Label the result **Preliminary shortlist - not Specsket-assessed**.
- For each product, show manufacturer, product/series, official source, brief fit, and regional availability as either `availability_verified` or `availability_unverified`.
- Do not call Specsket analysis tools, invent profile fields, or display completeness percentages in this mode.
- If web research is unavailable, do not fabricate products or sources; ask the user for URLs or files.

## Prepare a Specsket-ready extraction

Call `specsket_get_capabilities`, then `specsket_get_product_research_contract`. Require product ingestion read/validate capability, use the advertised client-analysis product contract (normally `product@3`), and do not call the server start/poll/assess loop. ChatGPT performs source linking, family grouping, dynamic profile construction, evidence mapping, product-topology selection, and explicit unresolved states. This mode reports evidence coverage, not an official completeness percentage.

For an official collection or catalogue URL, automatically inspect the supplied page and bounded relevant manufacturer-controlled product pages and technical documents. Build a source graph before deciding record count, propose separate products, one parent with variants, or multiple parents with variants, and create only documented combinations. Continue into `specsket-product-ingestion` only after the user asks to stage; that separate workflow owns validation and write confirmation.
Present the proposed structure and resulting record/combination counts before staging so the user can keep the recommendation or choose separate product records.

### Project-aware designer discovery

When a verified designer asks to find products for a Specsket project and capabilities advertise `project_product_discovery.read`, use the additive project-aware workflow. It does not replace ordinary discovery or the live product field catalog.

1. Fetch the project-aware contract, list accessible projects, and require explicit stable-ID selection. Never infer a project from a name, open page, or prior chat.
2. Select a Project Intelligence target and load its current V2 context. Ask for rooms only when room selection changes the applicable product criteria.
3. Account for every server-derived effective searchable requirement. Mandatory and client-required criteria cannot be downgraded. Keep contextual, satisfied, waived, and not-applicable requirements visible without fabricating product needs.
4. Call `specsket_validate_project_product_search_brief` before public product research. Send only the returned `public_query_projection` to search. Never add back project/client identity, raw requirement text, source filenames, internal IDs, contacts, exact addresses, private URLs, or undisclosed budget data.
5. Search and extract through the normal Ready workflow, using the unchanged current product schema, field catalog, evidence, presentation, and applicability contracts.
6. Give each selected record an evidence-bound `project-product-fit-assessment@1`. `blocked` and `insufficient_evidence` products may be shown but cannot stage.
7. Continue into project-aware product ingestion only after explicit selection and write confirmation. Approval still adds a private product to **My Product Library** only; it does not add the product to the project, room, Master List, moodboard, specification, or BOQ.

If the project-aware capability is absent, continue only as ordinary product discovery and clearly state that the results are not revision-bound to Project Intelligence.

## Run a deep Specsket assessment

Call `specsket_get_capabilities` before the first Specsket-backed step. Require `product_discovery.read`, `product_discovery.analyze`, and `product_discovery.assess`.
Then call `specsket_get_product_research_contract` and follow its Deep route. Deep uses the existing server `product@2` completeness workflow even when Ready extraction advertises `product@3`.

If the MCP is unavailable or either analysis capability is false, continue only as **Provisional research coverage (not Specsket-assessed)**. Repeat that exact label beside every provisional comparison or coverage summary. Do not calculate or display a percentage completeness score, critical-completeness score, risk tier, profile name, or weighted ranking from a client-created checklist. Never shorten this label to "Spec completeness". State the unavailable capability and explain that authoritative completeness requires Specsket MCP Tools Beta, OAuth, enabled capabilities, and a new chat after activation.

1. Translate the brief into design intent, constraints, project stage, geography, application, and procurement needs.
2. Research broadly and select no more than two candidates for deep analysis unless the user explicitly asks for more.
3. Collect initial exact evidence and an ordered search trace for both selected candidates.
4. Start at most two initial analyses concurrently. Poll each job using its returned `retry_after_seconds`; do not tight-loop.
5. Use each returned server profile as the source of truth for expected properties and source escalation. Research unresolved performance, documentation, installation, maintenance, warranty, standards, certifications, and regional availability.
6. Start at most two final analyses concurrently with expanded evidence and new stable idempotency keys. Poll as instructed, then call `specsket_assess_specification_completeness`.
7. If `research_readiness.status` is `needs_escalation`, continue relevant `next_source_types`, or record each unavailable source as `not_found`, `blocked`, or `not_applicable` with a reason.
8. Stop when the assessment is `ready_to_present`, or when `vendor_confirmation_required` is the honest next action.
9. Preserve every expected-property state exactly: `verified`, `not_found`, `insufficient_evidence`, `conflicting_evidence`, `not_applicable`, or `requires_vendor_confirmation`.

Never infer performance values from material norms, related products, industry assumptions, or unnamed test systems. Keep series and manufacturer-platform claims at their actual scope. Preserve conflicting references.

## Present deep results

Show a compact comparison first. Then present the complete returned `specification_report` for every deeply assessed candidate, including identity, authoritative URL, profile/classification, design fit, regional availability, completeness, risk, status counts, missing critical properties, grouped property states, evidence links, research status, and next action.

Keep design suitability, specification completeness, and procurement availability separate. Do not manufacture numeric design or procurement scores. Visualization may supplement but never replace the accessible evidence-backed report.

## Keep discovery read-only

Do not validate, stage, ingest, publish, or approve during discovery. Wait for explicit selection such as “stage this” or “add #2 to Specsket.” Then use `specsket-product-ingestion`, carry forward the evidence catalog, and copy server-returned `product_v2_fields` unchanged. Reassess whenever evidence or the search trace changes.
