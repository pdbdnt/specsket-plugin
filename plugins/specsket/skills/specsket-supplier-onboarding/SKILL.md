---
name: specsket-supplier-onboarding
description: Analyze user-provided supplier research, spreadsheets, PDFs, documents, or official websites and stage normalized supplier proposals in Specsket for platform-admin review. Use when an administrator asks to research, map, normalize, sync, or push suppliers into Specsket.
---

# Specsket supplier onboarding

Use this skill only for the `supplier_onboarding` workflow and `supplier_proposal` entity.

Read [the workflow contract](../../references/workflow-contract.md) and [the evidence contract](../../references/evidence-contract.md) before calling write tools.

Call `specsket_get_capabilities` first. If that tool or the **Specsket MCP Tools Beta** connection is unavailable, stop the workflow and explain that the public plugin supplies instructions but live Specsket access requires a separate Streamable HTTP MCP connection to `https://integrations.specsket.com/mcp`, followed by Specsket OAuth and a new chat. Do not infer the user's identity, permissions, destination, schemas, or live Specsket state, and do not claim that anything was validated or staged.

The active principal must be `platform_admin`, and both `supplier_onboarding.validate` and `supplier_onboarding.stage` must be enabled. Fetch `supplier_proposal@1`; do not reuse product fields for supplier identities.

Use official, credential-free HTTPS sources supplied or approved by the user. Normalize website hostnames and country codes, but rely on Specsket validation for authoritative public-suffix identity and duplicate detection. Attach exact evidence to the supplier name, official website, domains, primary country, categories, and capabilities.

Fix every validation error through the same workflow. Before staging, show the proposed suppliers, evidence coverage, duplicate/conflict warnings, and explain that the destination is an admin proposal queue, not the live supplier directory. Explain that, after completion, Specsket will create a 60-second one-time link that signs the browser into the same platform-admin account connected through OAuth; if the browser currently uses another Specsket account, the user must explicitly confirm the switch. Ask once for explicit confirmation of create, stage, complete, signed-in link creation, and the browser-open attempt.

After confirmation, create the job, stage the exact validated proposal set, inspect all results, and complete it. Always print the permanent `review_url`. Then call `specsket_create_review_session` with the completed `job_id`. Print an **Open signed-in Specsket review** link using `browser_launch_url`, note its expiry, and ask the host to open that URL in the visible in-app browser when available. Never say that the browser opened unless the browser action returned success. If browser opening is unavailable or fails, keep the signed-in link clickable and repeat the permanent fallback URL. If the signed-in link expires, create another only after the user asks to open the completed review again. Never approve a proposal, create a supplier directly, switch to product ingestion as a workaround, or claim success before completion.
