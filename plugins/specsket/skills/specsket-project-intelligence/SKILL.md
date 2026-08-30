---
name: specsket-project-intelligence
description: Analyze project briefs, requirements, meeting notes, PDFs, DOCX files, and existing Specsket project context into evidence-backed project information, rooms, layered requirement checklists, and review proposals through the Specsket MCP. Use when a designer wants to extract, rescan, organize, compare, or stage Project Intelligence for a selected project. Do not use it to approve canonical project changes.
---

# Specsket Project Intelligence

This skill is ChatGPT-side workflow guidance. Use the OAuth-backed Specsket MCP as the only live project-data and staging path. The plugin does not contain project data, upload document bytes, or replace the MCP.

If the Specsket capabilities or Project Intelligence contract tools are unavailable, stop before any live read or write. Explain that the Specsket MCP connection must be enabled and authenticated, then continue only in a new chat where both the plugin and MCP are active.

## Select and bind the project

1. Call `specsket_get_capabilities`, then `specsket_get_project_intelligence_contract`.
2. List accessible projects and obtain explicit selection by stable project ID. Never infer selection from an open page, a prior chat, or a similar name.
3. Select the Project Intelligence target and read its complete context. Compare every finding with current project information, levels, rooms, approved requirement groups, requirements, overrides, notes, and eligible sources.

Platform administrators may select any project returned by the MCP. Designers remain limited to projects their connected Specsket account can access. Never use vendor context for Project Intelligence.

## Analyze the complete original in ChatGPT

ChatGPT is the semantic analyst. Directly inspect the complete user-provided document available in the conversation, including every page, table, drawing, list, footnote, and visibly embedded text that can affect project requirements. Treat each atomic statement as a candidate. Preserve conflicting statements separately, and do not silently merge obligations, authority layers, room scopes, or evidence.

A document attached to ChatGPT is not automatically forwarded to Specsket. If the exact original is not already an active stored project source, call `specsket_prepare_project_intelligence_source_upload`, explain why the original must be preserved, and have the user upload that same file through the authenticated handoff. Never claim that a stored source and a ChatGPT attachment are identical from the filename alone.

After the exact original is stored, call `specsket_start_project_intelligence_scan`, then use `specsket_get_project_intelligence_scan` to resume or poll that run. Follow the live contract's current scan mode and limits. Do not start a duplicate merely because the conversation or target session changed. Track one terminal coverage result for every page:

- `analyzed`;
- `intentionally_excluded`, with a reason;
- `unreadable`, with a reason;
- `unsupported`, with a reason; or
- `clarification_required`, with a reason.

Persist these receipts as `page_coverage` in bounded analysis batches. State clearly that this is ChatGPT-reported analysis coverage, not independent proof that no requirement was missed. Never invent a page number or hide an unreadable, unsupported, password-protected, missing-original, or otherwise blocked page.

## Bind exact evidence after analysis

After directly reading the original, use `specsket_get_project_intelligence_source_units` to bind findings to immutable stored-source units. These units help establish exact quotations, page or section locations, available geometry, and artifact digests; they are not a substitute for reading the document in ChatGPT.

Continue with the returned opaque cursor until it is exhausted. While the live contract requires source-unit accounting, classify every returned unit exactly once and preserve all supporting and contradicting occurrences. Reuse the same candidate reference for repeated occurrences of the same semantic finding so Specsket can consolidate the proposal while retaining every evidence location.

Classify every assertion as exactly one of:

- `source_backed`;
- `inference`;
- `design_team_recommendation`; or
- `unresolved_question`.

Every actionable source-backed proposal and assertion must link to its strongest exact quotation through immutable evidence clause IDs. Inferences, design-team recommendations, and unresolved questions must not carry evidence IDs and must never appear as verified source quotations. Report ambiguous, unmatched, missing, stale, unreadable, or unavailable evidence as an explicit verification gap.

## Structure findings

Organize findings into explicit destinations:

- supported Project Information field;
- project note with category when no structured field fits;
- existing-room mapping or new-room creation under an existing level;
- layered requirement group;
- atomic project, level, room-type, or room requirement;
- scoped requirement override;
- design-team requirement recommendation; or
- product recommendation when supported by the live contract.

Keep authority, municipality, client/owner, consultant/standard, operational, and design-team recommendation layers distinct. A requirement group is a container; stage its child requirements as separate atomic proposals with their own scope, obligation, verification method, and evidence.

For each room candidate, report its linked room-type and room-specific requirements. A zero count is a visible coverage warning, not proof that the room has no requirements.

For every proposal, include its applicability and intended Specsket destination. Keep the proposal atomic enough that a reviewer can edit, accept, reject, keep current, or request clarification without unintentionally deciding a separate requirement.

## Respect prior decisions on rescans

Use current context and returned rescan classifications. Preserve `previously_rejected`, `previously_kept_current`, `still_needs_clarification`, `already_approved`, and `pending_elsewhere` states. Do not bulk reselect them. If source wording materially changed, present it as changed after the prior decision with both the history and new evidence.

## Submit, review, and finalize

Submit bounded batches with `specsket_submit_project_intelligence_analysis_batch`, following the current live limits and schema. A coverage-only batch is valid when a page produced no source units. Use deterministic payload digests and reuse the same idempotency key only for an identical retry. Poll `specsket_get_project_intelligence_scan` after submission and wait for the returned source-accounting and evidence-verification state; never call an assertion verified merely because ChatGPT supplied it.

Before any staged write, show:

- exact selected project and access mode;
- included sources and coverage/assurance per source;
- blocked units and warnings;
- counts by project information, rooms, requirement groups, requirements, overrides, and recommendations;
- exact proposed destinations, including unresolved mappings;
- duplicates, conflicts, prior-decision matches, and open questions; and
- that finalization will create a governed review workspace without approving project data.

Obtain explicit confirmation, then call `specsket_finalize_project_intelligence_scan` with the current run and target session. Finalization creates one evidence-bound review workspace; it does not approve or materialize project changes. Read the returned finalization status and permanent authenticated review URL before reporting success. If the contract, context, stored source, decision memory, evidence, or accounting state is stale or incomplete, refresh and reconcile it instead of forcing the old payload.

Never approve through MCP. Proposal edits, current-versus-proposed values, decision notes, PDF evidence highlights, reviewer annotations and discussion, approval, and final materialization stay in the signed-in Specsket review workspace. Always return its permanent review URL. Create a short-lived signed-in handoff only after separately explaining the possible account switch and receiving confirmation.

If a Project Intelligence tool returns `conversation_refresh_required`, do not retry with cached legacy arguments. Tell the user to start a new chat with the current plugin and MCP connection so ChatGPT receives the current versionless contract. Do not expose internal schema generations or ask users to choose between implementation versions.
