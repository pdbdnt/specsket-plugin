# Changelog

Changes to the Specsket plugin (skills and references). The Specsket MCP server is versioned separately; the plugin names the MCP version it was written for where that matters.

## 0.1.0-beta.103 (16 Nov 2026)
- Validation gotchas name the failure code `validation_schema_invalid` for a request that has the current wrapper and failed a schema check (older servers still answer `product_readiness_required` or `product_discovery_scope_required`; read `issues[]` either way). Written for the server release that adds the code and the S3 fills (primary image option type, image resolution, certification binding, corrected `local_summary` counts, unknown `client_analysis_metadata` keys removed).

## 0.1.0-beta.102 (16 Nov 2026)
- Hosted skills no longer depend on opening a bundled file: discovery, vendor authoring and supplier onboarding now state their must-know rules in their own body (live check before answering a brand or model question and the preliminary-shortlist label, confirm each exact change, no other write path, untrusted source pages), their reference reading is "when you can open files", and every skill that links a reference says to continue with the skill and the server's answers when a file cannot be opened. Web clients without a file tool (the host-like reviewer runs) had told users they could not open the workflow files. SKILL.md sizes: discovery +943 bytes, vendor authoring +698, supplier onboarding +564, ingestion +173; the files a web client had to open before its first write drop from 7, 4 and 2 (35.7, 27.0 and 6.4 KB) to none.

## 0.1.0-beta.101 (16 Nov 2026)

## 0.1.0-beta.100 (16 Nov 2026)
- Ingestion skill: load a tool's schema with ToolSearch before its first call (a "Required" error on an unloaded tool means the schema was not loaded), and write scope and chunk files for the local `*_from_file` tools inside the configured payload folder.

## 0.1.0-beta.99 (16 Nov 2026)
- Ingestion wording from campaign runs: primary image ids and the ownership evidence that must also be on the primary media reference (`PRIMARY_IMAGE_EVIDENCE_REQUIRED`), one-fragment `total_canonical_bytes`, evidence ids must match the scope bindings and be bound to the chunk's partition, omit `authority_key` on source parents, scope fragment `revision` starts at 1, verified and quantity values are numbers with a unit, build large variant matrices from a template and validate a small one first, a claimed partition's `integration_conflict`, `local_auto_enrol_busy` retry, and respect robots.txt and access refusals (no User-Agent, header, proxy or mirror retries) in the discovery and ingestion skills.

## 0.1.0-beta.98 (16 Nov 2026)
- The local auto-enrol 3-record limit is advertised in capabilities and the ingestion guidance plans jobs under it.

## 0.1.0-beta.97 (16 Nov 2026)
- Ingestion wording from the campaign ledger: the scope gotchas and step 2 say which key each scope object uses (output parents `output_parent_id`, execution partitions `partition_id` and `orderable_variant_ids`, not `key`; `scope_id` is a UUID), and the validation gotchas say a record with a variant matrix needs `selector_coverage.mode: configurable` even with one SKU, and that the cause can sit behind the code `product_readiness_required`.

## 0.1.0-beta.96 (16 Nov 2026)
- The product discovery and product ingestion skill descriptions had a colon followed by a space in an unquoted YAML value, which is not valid YAML. Claude Code then left both skills out of the list the model can invoke (they showed only as slash commands), so they did not load on their own in beta.94 and beta.95. The colon is now a semicolon, and the release check rejects an unquoted frontmatter value with a colon and a space.

## 0.1.0-beta.95 (15 Nov 2026)
- The ingestion authoring-rules reference ends with a short list of rule ids (AR1 to AR10) that the server now also returns as `authoring_rules` in the authoring kit core answer, so a model that does not open the reference still sees the requirements.
- Two ingestion references say that `specsket_get_entity_schema` and `specsket_prepare_product_ingestion_records` are operator-address or local-app tools; the default hosted tool list no longer shows them (MCP 0.67.0).

## 0.1.0-beta.94 (11 Nov 2026)
- The vendor, supplier, discovery and ingestion skill descriptions now tell the assistant to load the skill first, before calling Specsket tools, and say what each skill holds. In the mock eval suite on Haiku the vendor skill loaded in 0 of 3 runs before and 6 of 6 after; the supplier, discovery and ingestion skills moved the same way. Mock-only results, not a measure of real-vendor use.
- The Claude plugin manifest now carries the directory listing fields (display name, icon, documentation, support, privacy and terms URLs), and the Codex manifest points at a new square logo (`assets/logo-square.png`); the wide logo stays for the README.

## 0.1.0-beta.93 (11 Nov 2026)
- Wording fixes from the first plugin eval runs: the vendor and supplier skills now say to read their workflow contracts with the Read tool before the first proposal, preview or write, because a proposal was not being treated as a write and the contract was skipped (in test runs the read went from 0 of 3 to 3 of 3). The matching server-side rule, that a request to save without asking is not approval of a review, ships with the Specsket MCP server.

## 0.1.0-beta.91 (8 Nov 2026)
- Three wording fixes from the hosted proof runs: `source_coverage.local_summary` counts selector states only and never documents (one run counted undownloadable documents and was refused); downloads run at most three at a time (a run of eight in parallel lost six to timeouts); and the address of a fact sheet or data sheet cited as the page of a certification or document entry is listed in `additional_official_urls`.

## 0.1.0-beta.90 (7 Nov 2026)
- Platform-admin destinations: do not list all vendors to find a destination (one run pulled a 41,000-character vendor list into the conversation); call the vendor lookup with the company name, and for the Warehouse leave the vendor id out because the server fills it. The ingestion skill also says where the authoring kit's `core` part stops: the ingestion contract blocks are read from `specsket_get_capabilities` (a coming MCP release removes them from the kit; older servers still repeat them), and `part: presentation` is read before resolving a `$ref` in `part: scope`. No MCP change.

## 0.1.0-beta.89 (7 Nov 2026)
- Less to read before the first write: the platform-admin existing-delete custody procedure is its own file, so vendor sessions no longer load it with the operator workflow contract (about 1,000 fewer tokens per vendor session); supplier onboarding reads a short workflow contract for its workflow (about 2,900 fewer tokens) instead of the whole product-import contract. No rule was removed: the longer files remain for the workflows that use them. Product ingestion also says to download a product's documents and images in one shell step with at most two tries per file, to record a file that still fails as unavailable (with its address and the error) and carry on, and to use the page text for a field when the page states it. No MCP change.
- Supplier onboarding now says to confirm before each write boundary (job creation, each staging operation, completion), matching the workflow contract; it previously said to ask once for all phases. The stricter rule is the one the contract always stated. A repository test now fails if a skill says to ask once.

## 0.1.0-beta.88 (7 Nov 2026)
- Two wording fixes found by a new consistency test: the optional host-side planner step says the planner is offered to platform administrators only (it is not listed for vendors or designers), and product ingestion names the review-session argument correctly (`candidate_id`, taken from the first accepted record's domain_record_id). No MCP change.

## 0.1.0-beta.87 (7 Nov 2026)
- Local staging also reaches a verified designer's own private review queue (the unassigned destination); the staging reference lists it among the local destinations capabilities report.

## 0.1.0-beta.86 (7 Nov 2026)
- Display names: the plugin is shown as "Specsket Workflows" (was "Specsket Workflows Beta") and the MCP connection as "Specsket for Designers and Vendors" ("Specsket Admin" for platform administrators); the plugin `name` stays `specsket`. The skills and references recognise either new name and still mention "Specsket MCP Tools Beta" as the older name. Written for MCP 0.61.0 (titles only).

## 0.1.0-beta.85 (7 Nov 2026)
- Project Intelligence: the skill and contract tell the assistant to read `analysis_batch_shapes` from `specsket_get_project_context` before building a batch (a fallback to the callable tool schema when an older server does not return it), that `payload_digest` is optional (Specsket derives it; never invent one), and how to propose floors (floors first, then rooms with `floor_proposal_external_id`; never both `level_id` and `floor_proposal_external_id`). Written for MCP 0.60.0 and the matching Specsket app release. Release order: app first, then MCP 0.60.0, then this plugin. On an older server the shapes fall back to the callable schema, but `payload_digest` is still required there, so the skill stops and asks for a server update instead of inventing a digest.

- Corrected the "Works with" section in both READMEs (it overstated what was tested in beta.84): it now says Claude Code (hosted and local MCP) and Codex (local MCP) are tested end to end, and that ChatGPT and Claude (claude.ai and Claude desktop) have documented connection steps with an end-to-end test against the hosted MCP pending.

## 0.1.0-beta.84 (7 Nov 2026)
- Assistant-neutral wording: the skills and references no longer name ChatGPT where they address or describe the running assistant (Project Intelligence, product discovery and ingestion), so they read correctly in Claude, Claude Code, Codex and ChatGPT. Both READMEs gain a Works with section listing the tested hosts (Claude, Claude Code, ChatGPT, Codex). MCP 0.59.0 matches (descriptions only).

## 0.1.0-beta.82 (7 Nov 2026)
- Local staging now reaches a platform administrator's warehouse and unassigned destinations (one destination per job); the staging reference says where capabilities list the allowed local destinations and that a warehouse job needs the owner's per-job enrolment, never a standing policy.

## 0.1.0-beta.81 (7 Nov 2026)
- From the first hosted runs (w14-b and w14-c): the scope gotchas say to upload every source the chunk will cite, including each PDF, with the scope, and to bind the evidence of downloads written once; the validation gotchas say an available, non-certificate download written once gets no `document_coverage` entry of yours (and unavailable files or certificates are not written once), and that a product-specific certification on a configurable record needs a combination-scoped download occurrence (otherwise use family-level). A server fix (same release) derives the profile's `normalized_context` even when `technical.profile_key` is also left to the server.

## 0.1.0-beta.80 (7 Nov 2026)
- Product search results from Specsket are ranked by relevance only (MCP 0.54.0): the discovery and designer skills no longer ask for Sponsored or Partner order or labels.
- Extra product photos (extra views, close-ups, room renders) go to `media.project_images`, as the preview contract says; the readiness contract and authoring rules now say so too. The validator still accepts product-owned gallery references after the primary in `media.images`.

## 0.1.0-beta.79 (7 Nov 2026)
- Product ingestion works the same through the hosted Specsket MCP and a local one. On the hosted service, which has no stage intents, `specsket_stage_validated_chunk` (MCP 0.53.0) stages through the records route and answers `route: records`: no separate dispatch, the product goes straight to review. An unknown outcome is checked with `specsket_get_ingestion_job` before any retry, and a retry uses the same keys.

## 0.1.0-beta.78 (7 Nov 2026)
- The validation gotchas say what a media reference's `owner_id` is for every owner kind: the record's own `external_record_id` for product (primary and gallery), project and technical drawing images, and a `key:` reference for option values, combinations, wireframes and parts. The readiness contract says the same for the primary image.
- A summarising web-fetch tool is not a rendering browser: when no browser is available and an official page is server-rendered, the assistant reads its raw HTML and quotes from that.

## 0.1.0-beta.77 (7 Nov 2026)
- Before the first field preview, product ingestion asks whether you want the full field list (the default) or only the filled and unresolved fields. Without an answer, or in an unattended run, it shows the full list. The shorter layout still names every other field under its tab, so nothing is dropped from the accounting.

## 0.1.0-beta.76 (7 Nov 2026)
- When an official page cannot be read (refused, behind a login, or shown only through JavaScript without a browser), product ingestion says which page was blocked and asks for a rendered pass or the page content and files; it never switches to another manufacturer or fills the gap itself.
- The preview contract's source-control coverage matrix, used only by repository tests, is in its own file, so assistants no longer read it.

## 0.1.0-beta.75 (7 Nov 2026)
- The remaining named sections of the workflow contract (rendered-page extraction, durable chunk staging, staging recovery) and each situation in the conditional rules are in their own small files, so an assistant no longer reads those long files in full.

## 0.1.0-beta.74 (7 Nov 2026)
- Product ingestion uploads a discovery scope from a local file when the local MCP offers it, and uses the short derived preview that validation from a file now returns (MCP 0.49.0).

## 0.1.0-beta.73 (7 Nov 2026)
- Ingestion references split into small per-step files, so an assistant reads about 30,000 characters less per import.

## 0.1.0-beta.72 (7 Nov 2026)
- The product readiness contract no longer sends gallery views to the main image list.

## 0.1.0-beta.71 (7 Nov 2026)
- Less reading before an import: general workflow steps are read only when named, rarer rules only when they apply (MCP 0.48.0).

## 0.1.0-beta.70 (7 Nov 2026)
- Directory-readiness wording, read-on-demand references with tables of contents, and designer ingestion guidance (MCP 0.47.0).

## 0.1.0-beta.69 (7 Nov 2026)
- Validate a chunk from a local file without it entering the conversation; clearer document-anchor errors; retried scope and job creation return the earlier result (MCP 0.46.0).

## 0.1.0-beta.65 to beta.68 (6 Nov 2026)
- Downloads, certificate files and page URLs cross-checked against coverage and the discovery scope (beta.65).
- Vendor typed variant scopes (beta.66).
- Opt-in write-once authoring: a document, specification-table scope, per-finish table, page URLs, shared row evidence and coverage counts can be written once and the server builds the rest (beta.67, beta.68).

## 0.1.0-beta.61 to beta.64 (6 Nov 2026)
- The authoring kit's presentation schemas split into parts that each fit one tool result (beta.61).
- Chunks reuse the evidence uploaded with the discovery scope (beta.62).
- Guidance for the compact validation answer and the source-of-truth checks (beta.63, beta.64).

## 0.1.0-beta.58 to beta.60 (6 Nov 2026)
- One authoring-kit read with exact product shapes, the four-write ingestion path, the ingestion tool set, and a shorter ingestion skill (about 2,800 words, rules moved into named references).
- Combination and selector image references are left to the server (beta.60).

## 0.1.0-beta.55 to beta.57 (22 to 23 Oct 2026)
- Operator-only rules moved out of the ingestion path; link checks across the plugin; the vendor product listing tool named; compact assistant-facing contracts (MCP 0.39.0).

## 0.1.0-beta.47 to beta.54 (16 to 19 Oct 2026)
- Designer skill: project needs, library search, fit against requirements, adding with confirmation, cutsheets, saved web results and vendor invitations (beta.47, beta.48).
- Procurement skill: request, compare and decide quotations (beta.49, beta.50).
- Product type and image rules, staged candidate summaries, linking a maker's product instead of copying it, and the certification rule (beta.51 to beta.54).

## 0.1.0-beta.39 to beta.46 (13 to 15 Oct 2026)
- Vendor authoring skill rewritten as a guided storefront journey; product order, archive, deletion approval and request limits (beta.39 to beta.41).
- Library search by text, key specification values and vendor parameters; product details, comparison and alternatives; cutsheets with a QR code (beta.42 to beta.46).
