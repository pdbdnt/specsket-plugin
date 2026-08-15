# Specsket plugin beta

This public package contains portable Specsket workflow skills and marketplace metadata. It does not contain a registered ChatGPT app, MCP configuration, the Specsket application or MCP server implementation, credentials, database access, or private repository history.

OAuth selects one eligible vendor workspace, one verified designer-private workspace, or a platform-administrator context. Product and supplier writes are staged into existing human-review queues; the plugin cannot publish or approve them.

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
2. Add a custom MCP named **Specsket MCP Beta**.
3. Choose **Streamable HTTP** and enter `https://integrations.specsket.com/mcp`.
4. Save, choose **Authenticate**, and sign in with the Specsket account enabled by an administrator at `/admin/users`.
5. Start a new chat with **Specsket Beta** and the Specsket MCP connection enabled.

For Codex, register and authenticate the same hosted MCP separately:

```bash
codex mcp add specsket --url https://integrations.specsket.com/mcp
codex mcp login specsket
```

Each person installs and connects from their own ChatGPT or Codex account. Each Specsket login receives an independent authorization and remains limited by that user's role and permissions.

## Safety boundaries

- Review the destination, warnings, and evidence summary before approving any write action.
- Successful writes return a review URL; they do not publish or approve records.
- Removing the MCP connection from ChatGPT or Codex removes it from that host but does not itself revoke the Specsket OAuth grant.
- Revoking the OAuth grant ends the client's OAuth access. Disabling integration access in `/admin/users` revokes active Specsket authorizations and blocks later tool calls.
- Never paste passwords, access tokens, private keys, or service-role credentials into a chat or support request.

Support: [support@specsket.com](mailto:support@specsket.com)
