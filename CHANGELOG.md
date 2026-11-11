# Changelog

Changes to the Specsket plugin (skills and references). The Specsket MCP server is versioned separately; the plugin names the MCP version it was written for where that matters.

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
