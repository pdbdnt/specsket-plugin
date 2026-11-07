# Specsket plugin beta

Install the public Specsket marketplace in ChatGPT or Codex to add specification-aware product discovery, evidence-backed product ingestion, and supplier-onboarding workflows.

The package is public, but functionality remains private. The plugin contains workflow instructions only; its repository does not register or embed the hosted MCP connection. Every live connection separately uses Specsket OAuth, the integration-user allowlist, workspace permissions, subscription capabilities, and request-time authorization checks. The package contains no application source code, MCP server implementation, database credentials, OAuth secrets, or private repository history.

## Works with

Tested hosts: Claude (claude.ai and Claude desktop), Claude Code, ChatGPT and Codex. Any MCP-capable assistant can connect the Specsket MCP server (hosted at `https://integrations.specsket.com/mcp`), but only the hosts listed here have been connected and tested with it.

## What this plugin runs, sends and fetches

- **Runs:** nothing. The plugin is Markdown skills, Markdown references and two images (icon and logo). It has no scripts, hooks, commands or binaries, and it does not register an MCP server; you add the Specsket MCP connection yourself (see the installation steps below).
- **Sends:** nothing by itself. When you also connect the Specsket MCP (`https://integrations.specsket.com/mcp`), your assistant sends Specsket what a task needs, under your own Specsket sign-in: search terms, product and supplier details, the content of documents or files you ask it to import, and your confirmations for changes. Changes are prepared for review first and saved only after you confirm. Every call is checked against your account's role and permissions and recorded in Specsket's assistant activity log (who, which tool, the result and timing, not the content), which platform administrators and your company's administrators can see.
- **Fetches:** the plugin fetches nothing. Your assistant may read public web pages you point it at (for example a supplier's product page) with its own browsing tools. When a product is staged for review, the Specsket server downloads the public product images and documents named in it, and sending a quotation request notifies the vendors you chose; those MCP tools are marked as reaching outside Specsket (`openWorldHint`).
- **Files on your computer:** only what you ask your assistant to read. The hosted MCP has no access to your files.

Changes between versions are listed in `CHANGELOG.md`.

## ChatGPT installation

1. Open **Plugins** and choose **Add plugin marketplace**.
2. Use `https://github.com/pdbdnt/specsket-plugin.git` as the source.
3. Use `main` as the Git ref and leave sparse paths empty.
4. Install **Specsket Workflows Beta**.
5. In your own ChatGPT account, enable Developer mode and add a custom MCP named **Specsket MCP Tools Beta**.
6. Choose **Streamable HTTP**, enter `https://integrations.specsket.com/mcp`, save, and choose **Authenticate**.
7. Sign in with a Specsket account enabled for integrations, then start a new chat with the plugin and MCP connection enabled.

In the composer, tag **Specsket Workflows Beta** (the blue workflow plugin). You do not need to tag the MCP separately on every prompt. Once **Specsket MCP Tools Beta** is enabled for the chat, ChatGPT can select its authenticated tools automatically when the workflow requires live Specsket access.

## Codex installation

From a clone of this repository:

```bash
codex plugin marketplace add .
codex plugin add specsket@specsket-team
codex mcp add specsket --url https://integrations.specsket.com/mcp
codex mcp login specsket
```

The plugin and MCP are separate: the plugin supplies reusable workflow and display instructions, while the MCP supplies authenticated tools and live Specsket access. A preliminary product shortlist can continue without MCP when it is explicitly labeled **Preliminary shortlist - not Specsket-assessed** and makes no live Specsket claims. For Project Intelligence, the assistant itself reads the complete user-provided document and prepares page coverage, atomic proposals, applicability, destinations, and exact evidence. The MCP binds that analysis to privately stored originals and creates the governed review workspace; Specsket's lightweight document-ingestion service stores artifacts and deterministically indexes text and layout for evidence verification, but it does not perform semantic proposal generation or call an LLM. The organized Product Wizard field list is returned by the live MCP research contract rather than embedded in the plugin. Product ingestion now requires a pre-validation preview covering every current schema field under its consuming live tab, with literal detected values, clickable media/document evidence, and preserved document applicability. When the live contract advertises variant applicability, the workflow automatically discovers evidence-backed shared, exact-SKU/combination, excluded, and unresolved scope for technical features, installation, every Download section, and compatible products; no separate user call is required. For client-provided product files, ChatGPT can perform cross-file linking, dynamic family profiling, and evidence-bound normalization before the final MCP staging session. The MCP then supplies the current schema and taxonomy, deterministic validation, review-only staging, and signed-in review links without requiring a server AI job. A completed write workflow always returns a permanent review URL and may also create a 60-second, one-time link that opens the review signed in as the Specsket OAuth user. Specsket asks before replacing a different website session.

## Claude Code installation

```text
/plugin marketplace add pdbdnt/specsket-plugin
/plugin install specsket@specsket-team
```

```bash
claude mcp add --transport http specsket https://integrations.specsket.com/mcp
```

Run `/mcp`, choose `specsket`, and authenticate. Update with `/plugin marketplace update specsket-team` and `/plugin update specsket@specsket-team`.

## Support and legal

- Product page: https://specsket.com/plugins/specsket
- Support: https://specsket.com/support
- Privacy: https://specsket.com/privacy
- Terms: https://specsket.com/terms

This package is source-available, not open source. The `LICENSE` file in the repository root permits installation and authorized use while reserving other rights. Contact `support@specsket.com` for help or additional permission.
