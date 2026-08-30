# Specsket plugin beta

Install the public Specsket marketplace in ChatGPT or Codex to add specification-aware product discovery, evidence-backed product ingestion, and supplier-onboarding workflows.

The package is public, but functionality remains private. The plugin contains workflow instructions only; its repository does not register or embed the hosted MCP connection. Every live connection separately uses Specsket OAuth, the integration-user allowlist, workspace permissions, subscription capabilities, and request-time authorization checks. The package contains no application source code, MCP server implementation, database credentials, OAuth secrets, or private repository history.

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

The plugin and MCP are separate: the plugin supplies reusable workflow and display instructions, while the MCP supplies authenticated tools and live Specsket access. For Project Intelligence, ChatGPT itself reads the complete user-provided document and prepares page coverage, atomic proposals, applicability, destinations, and exact evidence. The MCP binds that analysis to privately stored originals and creates the governed review workspace; Specsket's lightweight document-ingestion service stores artifacts and deterministically indexes text and layout for evidence verification, but it does not perform semantic proposal generation or call an LLM. The organized Product Wizard field list is returned by the live MCP research contract rather than embedded in the plugin. Product ingestion now requires a pre-validation preview covering every current schema field under its consuming live tab, with literal detected values, clickable media/document evidence, and preserved document applicability. When the live contract advertises variant applicability, the workflow automatically discovers evidence-backed shared, exact-SKU/combination, excluded, and unresolved scope for technical features, installation, every Download section, and compatible products; no separate user call is required. For client-provided product files, ChatGPT can perform cross-file linking, dynamic family profiling, and evidence-bound normalization before the final MCP staging session. The MCP then supplies the current schema and taxonomy, deterministic validation, review-only staging, and signed-in review links without requiring a server AI job. A completed write workflow always returns a permanent review URL and may also create a 60-second, one-time link that opens the review signed in as the Specsket OAuth user. Specsket asks before replacing a different website session.

## Support and legal

- Product page: https://specsket.com/plugins/specsket
- Support: https://specsket.com/support
- Privacy: https://specsket.com/privacy
- Terms: https://specsket.com/terms

This package is source-available, not open source. The `LICENSE` file in the repository root permits installation and authorized use while reserving other rights. Contact `support@specsket.com` for help or additional permission.
