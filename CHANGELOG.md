# Changelog

Changes to the Specsket plugin (skills and references). The Specsket MCP server is versioned separately; the plugin names the MCP version it was written for where that matters.

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
