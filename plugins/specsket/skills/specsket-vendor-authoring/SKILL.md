---
name: specsket-vendor-authoring
description: Set up or maintain a vendor's Specsket company, storefront, collections, owned media and vendor products through the connected MCP, with every change reviewed and confirmed before it is saved or published. Use when a vendor or a Specsket administrator asks to set up, fill in, update, organize, preview or publish a storefront, create or reorder collections, or edit vendor products.
---

# Specsket vendor authoring

Use this workflow for company and storefront maintenance, collections, owned media and direct vendor-product authoring. Read the vendor identity and storefront section of [the workflow contract](../../references/workflow-contract.md) before any write. Product-ingestion candidates, supplier proposals and Project Intelligence keep their own review workflows; bulk listing from a website, catalog or spreadsheet belongs to the product-ingestion skill.

The rule throughout: suggest first, apply only after the user confirms the exact change. Nothing here is saved by preparing it, and nothing is public until the storefront is published.

## The journey

Work through these in order for "set up my storefront". For a narrower request, do step 1, then go to the step that matches.

### 1. Check access and pick the company

1. Call `specsket_get_capabilities`. Note the role (`principal.type`), whether `vendor_storefront` read, validate and stage are allowed, and whether `vendor_storefront_contract.draft_apply_via_mcp` and `publish_via_mcp` are true. The tool list already matches the connected role; a tool that is not listed is not available to this user.
2. Call `specsket_list_vendor_storefronts`. A vendor sees their own company. A platform administrator must choose one explicitly; ask which company when more than one could be meant. Never infer the company from a brand name, a domain or an open page.
3. If a needed operation is switched off, say which one and give the supported alternative from `vendor_capability_reasons` (usually the signed-in Specsket editor). Do not work around it.

A pending company is not activated, claimed or published by anything in this skill, and creating a company never creates a login.

### 2. Read what is there

Call `specsket_read_vendor_storefront` for the chosen company. With no `include` it returns three parts, each under its own key: `context` (live settings, permissions, field catalog), `draft` (the private storefront draft) and `readiness`. Keep the draft's `draft_revision` and `draft_digest`: every change set is bound to them. Add `include: ["contract"]` once when you need the change-set shapes or limits; it is large, so do not ask for it on each edit. If the answer has an `errors` key, that part could not be read: say so and do not treat it as empty.

Tell the user in a few lines what the storefront has and what is empty before proposing anything.

### 3. Company profile and storefront content

For profile population, follow [research completeness](../../references/vendor-research-completeness.md): research every relevant field from official sources by default, reuse verified canonical values instead of duplicating them, and record why a researched field stays blank. A field nobody researched is unfinished, not blank. When the user asks for specific edits only, make those edits and leave the rest alone; mention that other fields are still outstanding rather than filling them unasked.

Canonical company settings (name, country, website, contact) change the live company identity and use their own validate, stage and apply tools. Storefront fields change only the private draft. Say which of the two an edit touches.

### 4. Collections

Collections have no tools of their own. They are edited through the storefront change set, in `operations`, with `collection` set to `"collections"`:

- **Create**: `add` with a value holding an `id` (a new UUID you generate), a `name` and a `slug`. Other fields take their defaults.
- **Rename or edit**: `update` with the collection's `id` as `key` and only the fields that change.
- **Delete**: `remove` with the `id` as `key`. The products are kept and become unassigned.
- **Reorder collections**: `reorder` with `keys` listing every collection `id` once, in the new order. A partial list is rejected.
- **Assign products**: set `product_ids` on the collection in an `update`. A product belongs to one collection, so to move a product, update both: remove it from the old collection's `product_ids` and add it to the new one's.
- **Order products**: the order of `product_ids` is the order shown on the storefront. To reorder, send the collection's full `product_ids` list in the new order in an `update`.

Several of these can go in one change set and are reviewed and applied together.

New collections start hidden. Set `storefront_visible` to show one on the storefront and `featured` to feature it, and mention that a visible collection with no public products shows as empty.

### 5. Propose, review, confirm

1. Build one change set bound to the current `draft_revision` and `draft_digest`, with a fresh UUID `idempotency_key`. Use the `change_set_schema_version` that capabilities report (the version with a `research` block applies only when capabilities report a research schema version). Omitted fields and omitted optional lists stay unchanged.
2. Call `specsket_propose_vendor_storefront_changes` with the change set. It validates and, when the change set is valid, stages it in the same call. `staged: false` means nothing was staged: fix the issues listed under `validation` and propose again. `staged: true` returns the staged review under `review`. Staging changes nothing. If the response is lost, call `specsket_reconcile_vendor_storefront_stage` before proposing again.
3. Read the staged review with `specsket_get_vendor_review` (`kind: "storefront_change"`) and show the user the changes in plain words: what is added, changed, removed and reordered, and what is deleted for good.
4. Ask for confirmation. Existing explicit authorization covers only the effects the user already named; ask again for anything beyond it, and always for a deletion.
5. When `draft_apply_via_mcp` is true, call `specsket_confirm_vendor_review` with `kind: "storefront_change"`, the review's id, its current revision as `expected_review_revision`, its payload digest and `confirmed: true`. Otherwise give the permanent review link so the user can approve it in Specsket.
6. Read the draft again and report what is now saved. A staged review is not a saved change, and an applied review is not a published storefront.

Every kind of review is read and confirmed with the same two tools. The `kind` is `storefront_change`, `settings`, `image`, `product_change`, `membership_change` or `storefront_publication`; use the kind of the tool that created the review. Only `storefront_change` takes `expected_review_revision`.

If apply answers `delete_requires_app_approval`, the administrators require deletions to be approved in Specsket: give the user the review link from the message and stop; do not look for another way to apply it. If any call answers `integration_quota_exceeded`, tell the user the limit and reset time from the message and stop until then.

If apply answers that the confirmation does not match the staged review, or that the draft changed, do not retry the same call. Read the review and the draft again, show the user the difference, and prepare a new proposal on the current draft. Never overwrite a newer edit.

### 6. Media and vendor products

Use the media tools only for the exact slot (company logo, storefront cover, collection cover, gallery image, resource document) and show source and target before confirming. Importing or adopting media does not publish anything. Say plainly whether an image was only found, imported, saved, or is publicly visible.

For a single product edit, read the product, prepare the change, show the proposal and confirm it (`kind: "product_change"`). New saves are private drafts and product publication is its own confirmation. Validate product and media ownership before linking a product into a collection.

A product read and a product review both carry `listing_quality` (a score out of 100 with the required and recommended fields still missing, by name) and `possible_duplicates` (other products of the same company with the same SKU or name). Tell the user what is missing and offer to fill it. When a duplicate is listed, ask whether to update the existing product instead before saving a new one. `possible_duplicates: null` means the check could not run; do not report that there are none.

To remove a product, archive it: prepare a change with action `archive_product`, the product's id, its current `expected_updated_at` and an empty `form_patch`. Archiving cannot be combined with edits and deletes nothing; the product can be restored from the archive in Specsket. Read the proposal's `effect` and tell the user plainly when a public product will leave the public catalog as soon as the review is applied, without waiting for a storefront publication. Then confirm and apply like any product change.

### 7. Preview and publish

Read readiness with `specsket_read_vendor_storefront` (`include: ["readiness"]`) and show blockers separately from recommendations. Offer the private preview. Publishing is a separate, explicit step: prepare the publication review, show its public effect, and publish only after the user confirms that effect, by calling `specsket_confirm_vendor_review` with `kind: "storefront_publication"` and `publish: true` as well as `confirmed: true`. No other kind publishes a storefront. Then report the returned publication IDs, and check public visibility separately. Do not claim a browser or image check that was not performed.

When publication through MCP is off, tell the user to publish from the Specsket storefront editor after reviewing the draft.

## After an interruption

After a dropped connection or a lost response, read `specsket_get_vendor_authoring_operations` and the exact review or receipt before doing anything again. Reuse an idempotency key only for the identical request under current access. If the draft or company changed meanwhile, read it again and start a new review.

## Limits

- Changing an existing member's role or removing a member needs separate governance permission.
- Claims need human proof of authority. There is no automatic account creation, invitation, ownership transfer or inherited brand access.
- Never bypass a missing permission or a disabled operation with service-role credentials, direct database writes or copied browser tokens.

## Cutsheets of published products

For a cutsheet of one of the company's published products, use `specsket_generate_product_cutsheet` with the product id and give the returned 10-minute `download_url` to the user. Draft products cannot have a cutsheet generated through the assistant; publish first or use the Specsket web app.
