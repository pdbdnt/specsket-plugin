# Specsket plugin beta

This public package contains portable Specsket discovery, ingestion, and onboarding workflow skills plus marketplace metadata. It does not contain a registered ChatGPT app, MCP configuration, the Specsket application or MCP server implementation, credentials, database access, or private repository history.

Product discovery now defaults broad, ambiguous requests to a bounded preliminary shortlist of up to five products. Deep category-profile analysis is reserved for explicit specification-readiness requests or up to two selected candidates, reducing unnecessary model calls and database polling while keeping evidence and availability states visible.

OAuth selects one eligible vendor workspace, one verified designer-private workspace, or a platform-administrator context. Product and supplier writes are staged into existing human-review queues; the plugin cannot publish or approve them. After a confirmed workflow completes, the live MCP can create a 60-second, one-time signed-in browser link while still returning a permanent review URL.

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
- Successful writes return a review URL; they do not publish or approve records.
- The permanent review URL contains no credential. The optional signed-in browser link is single-use, expires after 60 seconds, and signs the browser into the Specsket account connected through OAuth. If another Specsket account is active, Specsket asks before switching it.
- Removing the MCP connection from ChatGPT or Codex removes it from that host but does not itself revoke the Specsket OAuth grant.
- Revoking the OAuth grant ends the client's OAuth access. Disabling integration access in `/admin/users` revokes active Specsket authorizations and blocks later tool calls.
- Never paste passwords, access tokens, private keys, or service-role credentials into a chat or support request.

Support: [support@specsket.com](mailto:support@specsket.com)
