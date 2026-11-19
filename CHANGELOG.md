# Changelog

Changes to the Specsket plugin (skills and references). The Specsket MCP server is versioned separately; the plugin names the MCP version it was written for where that matters.

## 0.1.0-beta.139 (19 Nov 2026)
- Ingestion: the scope gotchas line about key names is replaced: source parents, orderable variants, output parents and execution partitions all carry `key`; a partition lists its output parents as `key:<name>`; ids, split fields (none by default) and digests are derived. The server accepts this form (it rejected `key` on output parents and partitions before, which cost one upload call in about two of three runs).

## 0.1.0-beta.138 (19 Nov 2026)
- Ingestion: one gotchas line reworded: to scope a compatible relationship to some variants, give the item `write_once_applies_to` with the combination keys (the server builds the occurrence). Kit text (served by the server, not in the plugin files): an image needs no source of its own, its evidence cites the page source.

## 0.1.0-beta.137 (19 Nov 2026)
- Ingestion: step 4 of the skill says `specsket_validate_records` carries `submission`, `product_presentation`, `product_variant_applicability` and `source_coverage` as separate arguments in one call, never nested inside `submission` (a nested request made one panel answer 65 KB, which the client refused to show; MCP 0.71.0 now unwraps it, but a separate call keeps the chunk handle and the compact answer). Two redundant sentences were removed, so the skill is not longer. No kit shape changed (the kit digest is unchanged).

## 0.1.0-beta.136 (19 Nov 2026)
- Ingestion: the gotchas reference no longer says a compliance or certification claim without a download cannot be `product-specific`. A certification entry without a `file_url` (a standard named on the page) needs no binding; one with a `file_url` still binds to a combination-scoped download occurrence (for a true single SKU an `all_variants` download counts). Matches the server from staging after #667. No MCP change needed.

## 0.1.0-beta.135 (19 Nov 2026)
- Ingestion: reverts the beta.134 kit split. The authoring kit's core part carries the submission contract and authoring shapes again, and step 0 reads core alone. In the d7 panel the assistant read `presentation` and `authoring` in separate turns, so the early saving (8-11 KB per turn for turns 4-7) was paid back by a context jump at turn 8 and the peak was not lower (115K against 110.5K at d6). The revert follows the pre-agreed gate, not a clean measurement (19 runs, 5 lost to rate limits, 3 to category pages). The MCP keeps the `authoring` part value; against any app version it is harmless, because an unknown part falls back to core.

## 0.1.0-beta.134 (19 Nov 2026)
- Ingestion: the authoring kit has a fifth part, `authoring` (the product@3 submission contract and the `authoring_shapes` used only while writing a chunk). The assistant reads it together with `part: presentation` at step 3 instead of reading it with `part: core` at step 0, so about 20 KB less sits in context for the early turns (core 34.9 KB to 14.9 KB; total kit bytes unchanged). Core keeps a stub that names the part. Needs MCP 0.70.0 (the part enum; the MCP is deployed first, then the app). An older plugin keeps working: it reads core, scope and presentation "as the note says", and the core stub and note name the part.

## 0.1.0-beta.133 (18 Nov 2026)
- Ingestion: the skill is about a quarter shorter (27.0 KB to 20.7 KB). It no longer restates what the authoring kit's shapes (`authoring_shapes`, `server_fills`) and the server's errors already say; the rules the assistant must follow are unchanged. No MCP change.

## 0.1.0-beta.132 (19 Nov 2026)
- Ingestion: one `specsket_search_taxonomy` call with `queries` and no `taxonomy` searches the functional categories and CSI together and needs no version (needs MCP 0.69.8; an older MCP still takes one taxonomy and a version per call).

## 0.1.0-beta.131 (19 Nov 2026)
- Ingestion: the assistant reads the kit's `part: scope` at step 2 right before the scope upload and `part: presentation` at step 3 after the scope is finalized, instead of both before the scope (what is read stays in context for every later call), and calls capabilities with `focus: "product_ingestion"` to leave out the vendor and project blocks. Needs MCP 0.69.7 for `focus`; an older MCP ignores nothing here because the argument is simply not listed, so the assistant calls capabilities without it.

## 0.1.0-beta.130 (18 Nov 2026)
- Ingestion: step 0 no longer lists what each authoring-kit part carries (the kit's own `note` and the scope part already say it); the assistant reads `part: core`, then `scope` before uploading the scope and `presentation` before writing `product_presentation`; `part: legacy` is for an older flow or for a schema that a one-line stub in the kit names (the rarely written readiness, routing-manifest and parts schemas). Works with every MCP version; MCP 0.69.6 additionally leaves the Project Intelligence rules out of `specsket_get_capabilities` for principals without Project Intelligence.

## 0.1.0-beta.129 (18 Nov 2026)
- Ingestion: a verified `technical.properties` observation with a value and evidence no longer needs `value_origin`; leave it out for a value read from the source (`normalized` if you converted it, `derived` with a `derivation`). The example in the validation gotchas drops the field. Needs the server that stores a verified observation without an origin (staging after S70); an older server still asks for it.

## 0.1.0-beta.128 (18 Nov 2026)
- Ingestion: after the user's yes the assistant no longer asks again for a changed record count, a product that turned out unavailable, an unlabelled code or an unsupported axis. The yes covers the confirmed products, every partition and the recommended structure up to staging (job creation and staging keep their own confirmation); facts found later get the default and the first line of the summary; it asks only when no confirmed product can be sourced or the account or the confirmed products would change. Three "new confirmation" clauses and a leftover "ask for confirmation" in the collision rule are deleted (the reading path grows by 31 bytes net). The kit's AR3 rule says the same.
- Needs MCP 0.69.4 for the kept-submission flow (a call that lacks sidecars is kept under a handle and completed with `base_chunk` and the missing parts) and for `execution_progress` in a collection's validate answers; older MCP versions still work, they only lack those two aids.

## 0.1.0-beta.127 (17 Nov 2026)
- Ingestion: a variant with no code anywhere official states `identity_not_published` (checked evidence ids and a note) with its `documented_tuple` in the scope, instead of a typed or invented code; its combination writes `sku_origin: "generated_from_source_identity"` and leaves `sku` and `source_identity` out, and the server fills them from the scope. Correction to beta.126: `source_frontier_status` is `exhausted` when every product asked for was found (the listing pages, pagination and subcategories opened for a category scope) and the opened pages list each one's variants, otherwise `incomplete` with `source_frontier_reason`; a single named product is not always `exhausted`. Needs the server release with sweeps S48-S51 (the server fills the identity and SKU of a generated-SKU combination from the scope).

## 0.1.0-beta.126 (17 Nov 2026)
- Ingestion skill: says when `source_frontier_status` is `exhausted` (the opened pages list every variant of the product asked for; a single named product always) and `incomplete`; the one draft confirmation also covers re-creating the draft for the same product, and after the user's yes to the plan the assistant carries it out without asking again. The scope upload error for a page outside the declared domain now names the domain to declare. Two skill sentences shortened.

## 0.1.0-beta.125 (17 Nov 2026)
- Editing an existing product: the vendor-authoring skill says to find it (`specsket_list_vendor_products` `query`), read it and prepare a change, never to re-ingest it, and to show the review's `changes`/`added`/`removed` and name removed or disabled items before confirming. The ingestion skill no longer lists "sync" and sends rows that match an existing product to vendor authoring; its repeated trigger sentence is gone.

## 0.1.0-beta.124 (17 Nov 2026)
- Scope: in a scope of one fragment and one partition, the server derives the partition's `evidence_refs` as the union plus the evidence you bound that no parent or variant cites (all or none; more than 20 are left out); the gotcha line says to bind such a datasheet or certificate and leave the field out.

## 0.1.0-beta.123 (17 Nov 2026)
- A generated internal SKU the author writes wrongly is replaced by the one the source identity defines and listed in `derived.values.generated_sku_replaced`; the gotcha line no longer says a written SKU fails. A single product written without a variant matrix gets its `inspected_states` even when `unresolved_states` are stated. `record_manifest: null` is dropped like an omitted manifest. An unrecognized key, a Material Finish value without `finish`, and a refused `inspected_states` fill now say in `expected` what to remove or add.

## 0.1.0-beta.122 (17 Nov 2026)
- Workflow contract: the product and its sidecars go in one validation call (the old sentence about not adding sidecars to the product read as "send them separately"). An `unresolved_states` list no longer stops the server from building `inspected_states` for the enabled combinations. An unknown applicability or coverage evidence reference is one issue per reference and names the evidence ids that exist.

## 0.1.0-beta.121 (17 Nov 2026)
- A not_found observation may leave `checked_source_types` out: the server uses the analysis_trace entries whose outcome is `checked` (a stated list is kept, also an empty one). The example no longer shows the key; a missing `analysis_trace` is answered with its entry shape and the profile's source types.

## 0.1.0-beta.120 (16 Nov 2026)
- A vendor or designer leaves `record_manifest` and its digest out: the server writes the one legal routing (the kit's `routing_manifest` shape and the schema text say so). Platform administrators still write it.

## 0.1.0-beta.119 (16 Nov 2026)
- Minimal source_coverage works end to end: `unresolved_states` is filled as an empty list when left out, and a single product without a variant matrix states `selector_coverage.evidence_refs` next to `inspected_page` (the server builds its one state). Needs the server release with S26. Reading path 113,627 bytes.

## 0.1.0-beta.118 (16 Nov 2026)
- Vendor authoring: adding a product from a page, website, catalogue, PDF or spreadsheet belongs to the ingestion skill; authoring may do it only when `product_ingestion.validate` is false, and must say it skips evidence validation and the review queue (the old text assigned only bulk listing).

## 0.1.0-beta.117 (16 Nov 2026)
- Source coverage: the scope and partition digests and each record's `source_parent_id`, `output_parent_id` and `orderable_variant_ids` are in the kit's `server_fills` list (the server takes them from the execution and partition you name by `execution_id` and `execution_partition_id`); the minimal shape no longer shows them. Reading path 113,664 bytes.

## 0.1.0-beta.116 (16 Nov 2026)
- Source coverage: with `chunk_write_once_3` copy the kit's `source_coverage_minimal` shape and leave out everything in its `server_fills` list (`parent_coverage`, `completeness_scope`, `local_summary`, counts, `mode`, `axes`, `inspected_states`), stating the one inspected page as `inspected_page`. One list replaces the per-field sentences; reading path 113,643 bytes (preview layout is decided, not asked: filled and unresolved fields by default), after cutting the older-MCP stage_request recipe from the staging gotchas (needs the server release with S16-S19 and S21).

## 0.1.0-beta.115 (16 Nov 2026)
- Selector coverage: state the one official page you inspected as `selector_coverage.inspected_page` and leave `axes` and `inspected_states` out; the server writes one inspected state per enabled combination (needs the server release with S17). Two phrases cut from the validation gotchas. Reading path 114,658 bytes.

## 0.1.0-beta.114 (16 Nov 2026)
- Ingestion skill, scope step: open every variant, size or finish page you will cite before uploading the scope and bind each as its own source (a finalized scope cannot gain sources; Y5, A1 and A7 each needed a second scope). Two redundant scope-gotcha phrases cut. Reading path 114,693 bytes.

## 0.1.0-beta.113 (16 Nov 2026)
- Two small trims (a repeated "do not ask again", an example the error already shows) so the reading path has headroom: 114,734 bytes.

## 0.1.0-beta.112 (16 Nov 2026)
- Finalize with `open_execution: {}`: the server derives the idempotency key (needs MCP 0.67.6); after a failed job send `open_execution: { replaces_execution_id }` to start over. Scope gotchas shortened (older-server variants, warnings the server explains). The private draft (scope upload, finalize, open the execution) is now ONE confirmation, with Dennis's approval of 16 Nov; staging, job creation, completion and approval keep their own, and the consistency guard still forbids any confirmation spanning them. Reading path 115,576 -> 114,800 bytes.

## 0.1.0-beta.111 (16 Nov 2026)
- Validation gotchas: an `unresolved_states` entry may name only the axes it could determine (server 0.67.x accepts this); the inventory-warning name was cut from the selector URL line (the warning carries the URL). Reading path 115,576 bytes.

## 0.1.0-beta.110 (16 Nov 2026)
- Validation gotchas shortened by about 2,500 bytes: lines the server now makes unnecessary are removed or cut to the rule that still matters (derived `primary_image_option_type_id`, `image_resolution` fills, number coercion, `client_analysis_metadata`, axis keys and enum errors that carry `expected`). Rules whose failure carries no fix are kept. Hosted reading path 118,125 to 115,581 bytes; ceiling lowered to 115,600.

## 0.1.0-beta.109 (16 Nov 2026)
- Ingestion skill: the scope-binding rule is stated exactly. Every `evidence_ref` that `source_coverage` will cite (each state, document and certification) must be bound in the scope before the first upload; evidence only fields and the presentation cite goes in the chunk's own `evidence_catalog`. Replaces the beta.106 wording about "every page": in the last batch two runs bound one ref or omitted the one their coverage then cited, and ran into the evidence-binding refusal, because they read the unit as a page, not an evidence ref.

## 0.1.0-beta.108 (16 Nov 2026)
- Ingestion skill and workflow contract: the private draft scope and execution that validation needs are confirmed before each write, not once for both: first before the scope upload, then before finalizing and opening the execution (the existing merged phase). Still in plain words (nothing staged, no product created, the draft stays private and cannot be discarded yet), "do not stage or submit" neither forbids nor consents, and a decline stops at the unvalidated field preview. Replaces the single "ask once" confirmation of beta.104, which the plugin consistency guard and workflow contract step 8 do not allow.

## 0.1.0-beta.107 (16 Nov 2026)
- Validation gotchas: a true single SKU (one combination, one value per option type) may state `non_configurable` coverage with `axes: []` and one inspected state with `selections: {}`; every other variant matrix still needs `configurable`. Written for the server release that accepts the shape (5 of 6 first validates in the last fresh batch failed only on this). The older note about the failing answer's headline code is dropped: failures list their causes in `issues[]`.

## 0.1.0-beta.106 (16 Nov 2026)
- Ingestion skill: bind in the scope every page you will cite in the chunk, variant pages included, before the first upload (a finalized scope cannot change). In a fresh run the scope bound only the parent page, the chunk then cited the variant pages, and the run stopped at the evidence-binding refusal. Two sentences the server's errors now carry were dropped to keep the skill the same size.

## 0.1.0-beta.105 (16 Nov 2026)
- Ingestion skill: the reference reading is now "with file access", and a client that cannot open files is told to continue from the kit's `authoring_shapes` and `authoring_rules` and the `expected` values in errors (the first validate is a safe, read-only way to get exact fixes; every confirmation still applies) instead of stopping. In the last fresh runs the three that continued this way reached validate and were clean after one fix; the one that stopped treated "Read first ... in full" as a precondition.

## 0.1.0-beta.104 (16 Nov 2026)
- Ingestion skill and workflow contract: validating a chunk, including a request to validate or preview without staging, needs a private draft scope and execution. The skill now says so, asks one plain confirmation before creating them (nothing staged, no product created, the draft stays private and cannot be discarded yet), stops at the unvalidated field preview if the user declines, and keeps staging behind its own confirmation. Fixes runs that refused to create the scope because the user said "do not stage or submit".

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
