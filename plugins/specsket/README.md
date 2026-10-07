# Specsket plugin beta

This public package contains portable Specsket discovery, ingestion, and onboarding workflow skills plus marketplace metadata. It does not contain the authoritative Product Wizard field list; that principal-aware organized catalog comes from the connected live MCP research contract. The ingestion skill uses that live schema and catalog to require a complete tab-organized preview with literal detected values and evidence before topology selection and validation. It also does not contain a registered ChatGPT app, MCP configuration, the Specsket application or MCP server implementation, credentials, database access, or private repository history.

For Project Intelligence, ChatGPT reads and interprets the complete user-provided document. The plugin tells ChatGPT how to report page coverage, proposals, applicability, destinations, and exact evidence; the MCP securely binds that analysis to privately stored originals and creates a governed Specsket review workspace. Specsket's document-ingestion service stores artifacts and deterministically indexes text and layout for evidence verification without performing semantic proposal generation or calling an LLM.

Product discovery now defaults broad, ambiguous requests to a bounded preliminary shortlist of up to five products. Deep category-profile analysis is reserved for explicit specification-readiness requests or up to two selected candidates, reducing unnecessary model calls and database polling while keeping evidence and availability states visible.

Product ingestion packages canonical parent images separately from selector and exact-combination images, preserves unresolved image states, and reconciles physical assets with logical combination coverage under the live presentation contract. The live read-only planner separates record, immutable-job, chunk, and operational media workloads before any write, while presentation v3 preserves evidence-bound media and part ownership through Product Wizard review. Release validation includes a generic contract fixture plus an acceptance fixture; supplier-specific names belong only in acceptance data, never in reusable workflow logic.

OAuth selects one eligible vendor workspace, one verified designer-private workspace, or a platform-administrator context. Product-ingestion candidates and supplier proposals are staged into existing human-review queues; those research workflows cannot publish or approve them. After a confirmed workflow completes, the live MCP can create a 60-second, one-time signed-in browser link while still returning a permanent review URL.

For vendor authoring, first check live capabilities and the actual callable tools. Permitted platform administrators can explicitly confirm reviewed company creation and activation. Permitted vendor administrators can edit their connected company, while platform administrators select an exact allowed company. Canonical settings, private storefront drafts, owned image/PDF imports, product changes and public publication are separate reviewed operations. Show each exact effect and obtain its confirmation; saving a private draft does not publish it. Existing product-ingestion candidates and supplier proposals retain their human-review approval paths. See [the workflow contract](references/workflow-contract.md).

For an unchanged prepared generation-zero chunk whose saved validation expired, [original permission renewal](references/product-chunk-permission-renewal.md) uses a separate audited permission only when live capabilities and the actual callable tool both permit it. The immutable original remains unchanged; uncertain writes retain their reconciliation and custody rules. Plugin installation does not itself refresh the host’s MCP tools or enable this operation.

## Install the beta marketplace

In ChatGPT, open **Plugins**, choose **Add plugin marketplace**, and enter:

- Source: `https://github.com/pdbdnt/specsket-plugin.git`
- Git ref: `main`
- Sparse paths: leave empty

In Codex, install from the cloned repository root:

```bash
codex plugin marketplace add .
codex plugin add specsket@specsket-team
```

The plugin installs the workflow instructions only. To use live Specsket tools in ChatGPT, each tester must also:

1. Enable Developer mode in their own ChatGPT account.
2. Add a custom MCP named **Specsket MCP Tools Beta**.
3. Choose **Streamable HTTP** and enter `https://integrations.specsket.com/mcp`.
4. Save, choose **Authenticate**, and sign in with the Specsket account enabled by an administrator at `/admin/users`.
5. Start a new chat with **Specsket Workflows Beta** and the Specsket MCP connection enabled.

In the composer, tag **Specsket Workflows Beta** (the blue workflow plugin). You do not need to tag the MCP separately on every prompt. Once **Specsket MCP Tools Beta** is enabled for the chat, ChatGPT can select its authenticated tools automatically when the workflow requires live Specsket access.

For Codex, register and authenticate the same hosted MCP separately:

```bash
codex mcp add specsket --url https://integrations.specsket.com/mcp
codex mcp login specsket
```

Each person installs and connects from their own ChatGPT or Codex account. Each Specsket login receives an independent authorization and remains limited by that user's role and permissions.

## Safety boundaries

- Review the destination, warnings, and evidence summary before approving any write action.
- Ingestion and supplier writes return proposals for human review. Capability-enabled vendor authoring returns exact review IDs and durable receipts; publication requires a separate explicitly confirmed operation. Never infer success from capability advertisement alone.
- The permanent review URL contains no credential. The optional signed-in browser link is single-use, expires after 60 seconds, and signs the browser into the Specsket account connected through OAuth. If another Specsket account is active, Specsket asks before switching it.
- Removing the MCP connection from ChatGPT or Codex removes it from that host but does not itself revoke the Specsket OAuth grant.
- Revoking the OAuth grant ends the client's OAuth access. Disabling integration access in `/admin/users` revokes active Specsket authorizations and blocks later tool calls.
- Never paste passwords, access tokens, private keys, or service-role credentials into a chat or support request.

Support: [support@specsket.com](mailto:support@specsket.com)
