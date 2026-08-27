---
name: specsket-project-intelligence
description: Analyze project briefs, requirements, meeting notes, PDFs, DOCX files, and existing Specsket project context into evidence-backed project information, rooms, layered requirement checklists, and review proposals through the Specsket MCP. Use when a designer wants to extract, rescan, organize, compare, or stage Project Intelligence for a selected project. Do not use it to approve canonical project changes.
---

# Specsket Project Intelligence

Use the OAuth-backed Specsket MCP as the only live data and staging path. This skill adds workflow guidance; it does not provide another backend.

## Select and bind the project

1. Call `specsket_get_capabilities`, then `specsket_get_project_intelligence_contract`.
2. List accessible projects and obtain explicit selection by stable project ID. Never infer selection from an open page, a prior chat, or a similar name.
3. Select the Project Intelligence target and read its complete context. Compare every finding with current project information, levels, rooms, approved requirement groups, requirements, overrides, notes, and eligible sources.

Platform administrators may select any project returned by the MCP. Designers remain limited to projects their connected Specsket account can access. Never use vendor context for Project Intelligence.

## Read sources and prove coverage

For stored project documents, start a durable `full` scan with `specsket_start_project_intelligence_scan`. Resume it with `specsket_get_project_intelligence_scan`; do not start a duplicate run merely because the ChatGPT conversation or target session changed. Delta, changed-source, and rejected-only modes are not available until the MCP contract advertises them.

Prefer `specsket_get_project_intelligence_source_segments` for each selected project document. Preserve the first page's `extraction_digest` on every continuation request and continue in ordinal order until `next_after_ordinal` is null. If the extraction becomes stale, restart from the first segment. After each bounded batch receives terminal classifications, persist monotonic progress with `specsket_checkpoint_project_intelligence_scan`. Send one immutable outcome for every returned segment (`analyzed`, `not_applicable`, `unreadable`, `excluded_with_reason`, or `failed_terminal`), the complete candidate proposal descriptors found in that segment, the matching candidate count, and a reason for every blocked outcome; checkpoint counters must equal the persisted outcome manifest. Specsket derives the canonical fingerprints. When the same semantic candidate occurs in multiple segments, reuse its external ID and proposal content, preserve every segment as evidence, and consolidate it into one staged proposal. Preserve page, section, or text-chunk locators, extraction method, confidence, segment/extraction digests, warnings, and blocked units.

Use source text only as a compatibility fallback. Never invent page numbers. A legacy `client_attested`, `coverage_incomplete`, unsupported, password-protected, failed-OCR, or byte-unavailable source prevents an exhaustive-coverage claim. Tell the user which source/unit is blocked and whether re-upload as PDF, DOCX, or TXT is required.

Treat each atomic statement as a candidate. Preserve conflicting statements separately. Do not silently merge obligations, authority layers, room scopes, or evidence.

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

## Respect prior decisions on rescans

Use current context and returned rescan classifications. Preserve `previously_rejected`, `previously_kept_current`, `still_needs_clarification`, `already_approved`, and `pending_elsewhere` states. Do not bulk reselect them. If source wording materially changed, present it as changed after the prior decision with both the history and new evidence.

## Review before staging

Before any staged write, show:

- exact selected project and access mode;
- included sources and coverage/assurance per source;
- blocked units and warnings;
- counts by project information, rooms, requirement groups, requirements, overrides, and recommendations;
- exact proposed destinations, including unresolved mappings;
- duplicates, conflicts, prior-decision matches, and open questions; and
- the permanent Specsket review destination.

Obtain explicit confirmation, then create the proposal bundle with the completed `analysis_run_id`, validate it, and stage it using fresh UUID idempotency keys and the returned validation receipt. A run cannot finalize while any source segment lacks a terminal coverage outcome. If validation reports stale context, source, contract, decision memory, or collision state, refresh and re-compare instead of forcing the old payload.

The current governed import accepts at most 500 proposals. Candidate-batch submission is not yet available; if analysis exceeds that bound, report the capacity blocker and do not claim that the entire source set was staged.

Never approve through MCP. Approval, edits, mapping, and final materialization stay in the signed-in Specsket review workspace. Always return the permanent review URL; create a signed-in handoff only after separately explaining the possible account switch and receiving confirmation.
